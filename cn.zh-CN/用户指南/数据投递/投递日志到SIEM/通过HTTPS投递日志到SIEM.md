# 通过HTTPS投递日志到SIEM {#concept_265988 .concept}

本文通过以 Splunk HEC 为例来展示如何通过HEC将阿里云相关日志投递到SIEM。

假设当前 SIEM（如Splunk）位于组织内部环境（on-premise），而不是云端。为了安全考虑，没有开放任何端口让外界环境来访问此SIEM。

**说明：** 本章节中的配置代码仅为示例，最新最全的代码示例请参考[Github](https://github.com/aliyun/aliyun-log-python-sdk/blob/master/tests/consumer_group_examples/sync_data_to_splunk.py)或[Github（多源日志库时）](https://github.com/aliyun/aliyun-log-python-sdk/blob/master/tests/consumer_group_examples/sync_data_to_splunk_multiple_logstores.py)。

## 投递流程 {#section_mny_9pq_yc8 .section}

推荐使用日志服务消费组构建程序进行实时消费，然后通过 Splunk API（HEC）来发送日志给Splunk。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/219725/155921009347431_zh-CN.png)

## 主程序示例 {#section_o11_g58_pr1 .section}

如下代码展示主程序控制逻辑：

``` {#codeblock_3sp_5he_rlh}
def main():
    option, settings = get_monitor_option()

    logger.info("*** start to consume data...")
    worker = ConsumerWorker(SyncData, option, args=(settings,) )
    worker.start(join=True)

if __name__ == '__main__':
    main()
```

## 程序配置示例 {#section_c31_4bd_axp .section}

-   配置内容：
    -   程序日志文件：以便后续测试或者诊断可能的问题。
    -   基本配置项：包括日志服务连接配置和消费组配置。
    -   消费组的高级选项：性能调参，不推荐修改。
    -   SIEM（Splunk）相关参数与选项。
-   代码示例：

    请仔细阅读代码中相关注释并根据需要调整选项：

    ``` {#codeblock_fjd_xoc_f5b}
    #encoding: utf8
    import os
    import logging
    from logging.handlers import RotatingFileHandler
    
    root = logging.getLogger()
    handler = RotatingFileHandler("{0}_{1}.log".format(os.path.basename(__file__), current_process().pid), maxBytes=100*1024*1024, backupCount=5)
    handler.setFormatter(logging.Formatter(fmt='[%(asctime)s] - [%(threadName)s] - {%(module)s:%(funcName)s:%(lineno)d} %(levelname)s - %(message)s', datefmt='%Y-%m-%d %H:%M:%S'))
    root.setLevel(logging.INFO)
    root.addHandler(handler)
    root.addHandler(logging.StreamHandler())
    
    logger = logging.getLogger(__name__)
    
    def get_option():
        ##########################
        # 基本选项
        ##########################
    
        # 从环境变量中加载日志服务参数与选项
        endpoint = os.environ.get('SLS_ENDPOINT', '')
        accessKeyId = os.environ.get('SLS_AK_ID', '')
        accessKey = os.environ.get('SLS_AK_KEY', '')
        project = os.environ.get('SLS_PROJECT', '')
        logstore = os.environ.get('SLS_LOGSTORE', '')
        consumer_group = os.environ.get('SLS_CG', '')
    
        # 消费的起点。这个参数在首次运行程序的时候有效，后续再次运行时将从上一次消费的保存点继续消费。
        # 可以使用“begin”，“end”，或者特定的 ISO 时间格式。
        cursor_start_time = "2018-12-26 0:0:0"
    
        ##########################
        # 一些高级选项
        ##########################
    
        # 一般不建议修改消费者名称，尤其是需要进行并发消费时。
        consumer_name = "{0}-{1}".format(consumer_group, current_process().pid)
    
        # 心跳时长，当服务器在2倍时间内没有收到特定 Shard 的心跳报告时，服务器会认为对应消费者离线并重新调配任务。
        # 所以当网络环境不佳时，不建议将时长设置的比较小。
        heartbeat_interval = 20
    
        # 消费数据的最大间隔，如果数据生成的速度很快，不需要调整这个参数。
        data_fetch_interval = 1
    
        # 构建一个消费组和消费者
        option = LogHubConfig(endpoint, accessKeyId, accessKey, project, logstore, consumer_group, consumer_name,
                              cursor_position=CursorPosition.SPECIAL_TIMER_CURSOR,
                              cursor_start_time=cursor_start_time,
                              heartbeat_interval=heartbeat_interval,
                              data_fetch_interval=data_fetch_interval)
    
        # Splunk选项
        settings = {
                    "host": "10.1.2.3",
                    "port": 80,
                    "token": "a023nsdu123123123",
                    'https': False,             # 可选, bool
                    'timeout': 120,             # 可选, int
                    'ssl_verify': True,         # 可选, bool
                    "sourcetype": "",           # 可选, sourcetype
                    "index": "",                # 可选, index
                    "source": "",               # 可选, source
                }
    
        return option, settings
    ```


## 消费与投递示例 {#section_fvf_cwj_0hh .section}

如下代码展示如何从日志服务获取数据并投递到 Splunk。

``` {#codeblock_pci_umk_ve2}
from aliyun.log.consumer import *
from aliyun.log.pulllog_response import PullLogResponse
from multiprocessing import current_process
import time
import json
import socket
import requests

class SyncData(ConsumerProcessorBase):
    """
    这个消费者从日志服务消费数据并发送给 Splunk
    """
    def __init__(self, splunk_setting):
      """初始化并验证 Splunk 连通性"""
        super(SyncData, self).__init__()

        assert splunk_setting, ValueError("You need to configure settings of remote target")
        assert isinstance(splunk_setting, dict), ValueError("The settings should be dict to include necessary address and confidentials.")

        self.option = splunk_setting
        self.timeout = self.option.get("timeout", 120)

        # 测试 Splunk 连通性
        s = socket.socket()
        s.settimeout(self.timeout)
        s.connect((self.option["host"], self.option['port']))

        self.r = requests.session()
        self.r.max_redirects = 1
        self.r.verify = self.option.get("ssl_verify", True)
        self.r.headers['Authorization'] = "Splunk {}".format(self.option['token'])
        self.url = "{0}://{1}:{2}/services/collector/event".format("http" if not self.option.get('https') else "https", self.option['host'], self.option['port'])

        self.default_fields = {}
        if self.option.get("sourcetype"):
            self.default_fields['sourcetype'] = self.option.get("sourcetype")
        if self.option.get("source"):
            self.default_fields['source'] = self.option.get("source")
        if self.option.get("index"):
            self.default_fields['index'] = self.option.get("index")

    def process(self, log_groups, check_point_tracker):
        logs = PullLogResponse.loggroups_to_flattern_list(log_groups, time_as_str=True, decode_bytes=True)
        logger.info("Get data from shard {0}, log count: {1}".format(self.shard_id, len(logs)))
        for log in logs:
            # 发送数据到 Splunk
            event = {}
            event.update(self.default_fields)
            event['time'] = log[u'__time__']
            del log['__time__']

            json_topic = {"actiontrail_audit_event": ["event"] }
            topic = log.get("__topic__", "")
            if topic in json_topic:
                try:
                    for field in json_topic[topic]:
                        log[field] = json.loads(log[field])
                except Exception as ex:
                    pass
            event['event'] = json.dumps(log)

            data = json.dumps(event, sort_keys=True)

                try:
                    req = self.r.post(self.url, data=data, timeout=self.timeout)
                    req.raise_for_status()
                except Exception as err:
                    logger.debug("Failed to connect to remote Splunk server ({0}). Exception: {1}", self.url, err)

                    # TODO: 根据需要，添加一些重试或者报告

        logger.info("Complete send data to remote")

        self.save_checkpoint(check_point_tracker)
```

## 启动程序示例 {#section_0lc_8w7_r9f .section}

假设程序命名为sync\_data.py，启动如下：

``` {#codeblock_9hl_3ye_qkx}
export SLS_ENDPOINT=<Endpoint of your region>
export SLS_AK_ID=<YOUR AK ID>
export SLS_AK_KEY=<YOUR AK KEY>
export SLS_PROJECT=<SLS Project Name>
export SLS_LOGSTORE=<SLS Logstore Name>
export SLS_CG=<消费组名，可以简单命名为"syc_data">

pypy3 sync_data.py
```

## 多源日志库示例 {#section_zis_947_7vs .section}

针对多源日志库，需要公用一个 executor 以便避免进程过多，可以参考样例：[多源日志库时发送日志到Splunk](https://github.com/aliyun/aliyun-log-python-sdk/blob/master/tests/consumer_group_examples/sync_data_to_splunk_multiple_logstores.py)。核心的变化是主函数：

``` {#codeblock_2q8_vwy_gyp}
exeuctor, options, settings = get_option()

    logger.info("*** start to consume data...")
    workers = []

    for option in options:
        worker = ConsumerWorker(SyncData, option, args=(settings,) )
        workers.append(worker)
        worker.start()

    try:
        for i, worker in enumerate(workers):
            while worker.is_alive():
                worker.join(timeout=60)
            logger.info("worker project: {0} logstore: {1} exit unexpected, try to shutdown it".format(
                options[i].project, options[i].logstore))
            worker.shutdown()
    except KeyboardInterrupt:
        logger.info("*** try to exit **** ")
        for worker in workers:
            worker.shutdown()

        # wait for all workers to shutdown before shutting down executor
        for worker in workers:
            while worker.is_alive():
                worker.join(timeout=60)

    exeuctor.shutdown()
```

## 限制与约束 {#section_7yq_jkd_l9v .section}

每一个日志库（logstore）最多可以配置10个消费组，如果提示`ConsumerGroupQuotaExceed`则表示遇到限制，建议在控制台删除一些不用的消费组。

## 消费状态监控 {#section_t1f_n8k_40s .section}

-   在控制台查看[消费组状态](intl.zh-CN/用户指南/实时消费/消费组消费/消费组状态.md#)。
-   通过云监控查看[消费组延迟](intl.zh-CN/用户指南/实时消费/消费组消费/消费组监控与报警.md#)，并配置告警。

## 并发消费 {#section_0x6_agr_s1w .section}

基于消费组的程序，可以通过启动多次程序以达到并发的作用：

``` {#codeblock_dw6_2fj_m6i}
nohup pypy3 sync_data.py &
nohup pypy3 sync_data.py &
nohup pypy3 sync_data.py &
...
```

**说明：** 所有消费者的名称均不相同（消费者名以进程ID为后缀），且属于同一个消费组。 由于一个分区（Shard）只能被一个消费者消费，假设一个日志库有10个分区，那么最多有10个消费组同时消费。

## 吞吐量 {#section_dvq_rgu_6e8 .section}

基于测试，在没有带宽、接收端速率限制（如 Splunk 端）的情况下，用 pypy3 运行上述样例，单个消费者大约占用 10% 的单核 CPU 资源， 此时消费可以达到 5 MB/s 原始日志的速率。因此，10个消费者理论上可以达到 50 MB/s 原始日志每个 CPU 核，也就是每个 CPU 核每天可以消费 4 TB 原始日志。

## 高可用 {#section_4fw_3id_psf .section}

消费组将检测点（check-point）保存在服务器端，当一个消费者停止，另外一个消费者将自动接管并从断点继续消费。可以在不同机器上启动消费者，这样在一台机器停止或者损坏的情况下，其他机器上的消费者可以自动接管并从断点进行消费。理论上，为了备用，也可以通过不同机器启动大于 Shard 数量的消费者。

## HTTPS {#section_4eg_i6h_9bv .section}

如果服务入口（endpoint）配置为`https://`前缀，如`https://cn-beijing.log.aliyuncs.com`，程序与日志服务的连接将自动使用HTTPS加密。

服务器证书 \*.aliyuncs.com 是 GlobalSign 签发，默认大多数 Linux/Windows 的机器会自动信任此证书。如果某些特殊情况，机器不信任此证书，可以参考[这里](https://success.outsystems.com/Support/Enterprise_Customers/Installation/Install_a_trusted_root_CA__or_self-signed_certificate)下载并安装此证书。

