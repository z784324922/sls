# Send logs to an SIEM system over HTTPS {#concept_265988 .concept}

This topic describes how to send logs on Alibaba Cloud to a security information and event management \(SIEM\) system by using Splunk HTTP Event Collector \(HEC\).

Assume that the SIEM system, such as Splunk, is deployed to an on-premises environment. To ensure security, no ports are opened to allow users to access the SIEM system from an external environment.

**Note:** Code examples in this topic are used for reference only. For more information about the latest code examples, see [GitHub](https://github.com/aliyun/aliyun-log-python-sdk/blob/master/tests/consumer_group_examples/sync_data_to_splunk.py) or [GitHub \(applicable to the Logstore that has multiple data sources\)](https://github.com/aliyun/aliyun-log-python-sdk/blob/master/tests/consumer_group_examples/sync_data_to_splunk_multiple_logstores.py).

## Workflow {#section_mny_9pq_yc8 .section}

We recommend that you write the required program based on consumer groups in Log Service. Then, you can call API operations provided by Splunk HEC to send logs to Splunk.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/219725/156810614447431_en-US.png)

## Example: Write a main program {#section_o11_g58_pr1 .section}

The following code shows the control logic of a main program:

``` {#codeblock_3sp_5he_rlh}
def main():
    option, settings = get_monitor_option()

    logger.info("*** start to consume data...")
    worker = ConsumerWorker(SyncData, option, args=(settings,) )
    worker.start(join=True)

if __name__ == '__main__':
    main()
```

## Example: Configure the program {#section_c31_4bd_axp .section}

-   Configure the following information:
    -   Log file of the program: facilitates subsequent testing or diagnosis of potential issues.
    -   Basic configuration items: includes consumer group settings and connection to Log Service.
    -   Advanced options for consumer groups: adjusts performance. We do not recommend that you change these settings.
    -   Parameters and options for the SIEM system \(Splunk in this example\).
-   Code example:

    Read the code comments in the following example and change parameter settings based on your business needs:

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
        # Basic configuration items
        ##########################
    
        # Obtain parameters and options for Log Service from environment variables.
        endpoint = os.environ.get('SLS_ENDPOINT', '')
        accessKeyId = os.environ.get('SLS_AK_ID', '')
        accessKey = os.environ.get('SLS_AK_KEY', '')
        project = os.environ.get('SLS_PROJECT', '')
        logstore = os.environ.get('SLS_LOGSTORE', '')
        consumer_group = os.environ.get('SLS_CG', '')
    
        # The starting point of data consumption. This parameter is valid when you run the program for the first time. When you run the program the next time, the consumption will continue from the latest consumption checkpoint.
        # You can use the BEGIN...END statement or a specific ISO time.
        cursor_start_time = "2018-12-26 0:0:0"
    
        ##########################
        # Advanced options
        ##########################
    
        # We do not recommend that you modify the consumer name, especially when concurrent consumption is required.
        consumer_name = "{0}-{1}".format(consumer_group, current_process().pid)
    
        # The heartbeat interval. If the server does not receive a heartbeat report for a specific shard within twice the specified interval, it indicates that the consumer is offline. In this case, the server will allocate the task to another consumer.
        # We recommend that you set a greater interval when the network performance is poor.
        heartbeat_interval = 20
    
        # The maximum interval between two data consumption processes. If data is generated at a fast speed, you do not need to adjust the setting of this parameter.
        data_fetch_interval = 1
    
        # Create a consumer group that contains the consumer.
        option = LogHubConfig(endpoint, accessKeyId, accessKey, project, logstore, consumer_group, consumer_name,
                              cursor_position=CursorPosition.SPECIAL_TIMER_CURSOR,
                              cursor_start_time=cursor_start_time,
                              heartbeat_interval=heartbeat_interval,
                              data_fetch_interval=data_fetch_interval)
    
        # Splunk options
        settings = {
                    "host": "10.1.2.3",
                    "port": 80,
                    "token": "a023nsdu123123123",
                    'https': False,             # Optional. A Boolean variable.
                    'timeout': 120,             # Optional. An integer.
                    'ssl_verify': True,         # Optional. A Boolean variable.
                    "sourcetype": "",           # Optional. The sourcetype field is a default field defined by Splunk.
                    "index": "",                # Optional. The index field is a default field defined by Splunk.
                    "source": "",               # Optional. The source field is a default field defined by Splunk.
                }
    
        return option, settings
    ```


## Example: Consume and send data {#section_fvf_cwj_0hh .section}

The following example shows how to collect data from Log Service and send the data to Splunk.

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
    The consumer consumes data from Log Service and sends it to Splunk.
    """
    def __init__(self, splunk_setting):
      """Initiate Splunk and check network connectivity."""
        super(SyncData, self).__init__()

        assert splunk_setting, ValueError("You need to configure settings of remote target")
        assert isinstance(splunk_setting, dict), ValueError("The settings should be dict to include necessary address and confidentials.")

        self.option = splunk_setting
        self.timeout = self.option.get("timeout", 120)

        # Test connectivity to Splunk.
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
            # Send data to Splunk.
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

                    # TODO: Add code to handle errors. For example, you can add the code to retry or send a notification in response to an error.

        logger.info("Complete send data to remote")

        self.save_checkpoint(check_point_tracker)
```

## Example: Start the program {#section_0lc_8w7_r9f .section}

Assume that the program is named sync\_data.py. The following code shows how to start the program:

``` {#codeblock_9hl_3ye_qkx}
export SLS_ENDPOINT=<Endpoint of your region>
export SLS_AK_ID=<YOUR AK ID>
export SLS_AK_KEY=<YOUR AK KEY>
export SLS_PROJECT=<SLS Project Name>
export SLS_LOGSTORE=<SLS Logstore Name>
export SLS_CG=<Consumer group name. You can set it to "syc_data".>

python3 sync_data.py
```

## Example: Send data from a Logstore that has multiple sources {#section_zis_947_7vs .section}

For a Logstore that has multiple data sources, you must set a public executor to limit the number of processes. For more information about the code example, see [Send logs from a multi-source Logstore to Splunk](https://github.com/aliyun/aliyun-log-python-sdk/blob/master/tests/consumer_group_examples/sync_data_to_splunk_multiple_logstores.py). Note that the main function of the following example is different from the preceding one.

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

## Limits {#section_7yq_jkd_l9v .section}

You can configure up to 10 consumer groups for each Logstore in Log Service. If the system displays the `ConsumerGroupQuotaExceed` error message, we recommend that you log on to the Log Service console to delete consumer groups that you no longer need.

## View and monitor data consumption {#section_t1f_n8k_40s .section}

-   You can log on to the Log Service console to view the status of a consumer group. For more information, see [View consumer group status](../../../../intl.en-US/Real-time subscription and consumption/Consumption by consumer groups/View consumer group status.md#).
-   You can use CloudMonitor to view latency associated with consumer groups and to configure alerts. For more information, see [Consumer group latency](intl.en-US/Real-time subscription and consumption/Consumption by consumer groups/Consumer group - Monitoring alarm.md#).

## Concurrent consumption {#section_0x6_agr_s1w .section}

You can start multiple consumer group-based programs for multiple consumers to consume data at the same time.

``` {#codeblock_dw6_2fj_m6i}
nohup python3 sync_data.py &
nohup python3 sync_data.py &
nohup python3 sync_data.py &
...
```

**Note:** The names of all consumers are unique within a consumer group because these names are suffixed with process IDs. The data of one shard can be consumed by only one consumer. If a Logstore contains 10 shards and each consumer group contains only one consumer, up to 10 consumer groups can consume the data of all shards at the same time.

## Throughput {#section_dvq_rgu_6e8 .section}

In preceding examples, Python 3 is used to run the program without limits on the bandwidth or receiving speed, such as the receiving speed on Splunk. A single consumer consumes about 20% of single-core CPU resources. In this case, the consumption speed of raw logs can reach 10 MB/s. Therefore, if 10 consumers consume data at the same time, the consumption speed of raw logs can reach 100 MB/s per CPU core. Each CPU core can consume up to 0.9 TB of raw logs every day.

## High availability {#section_4fw_3id_psf .section}

A consumer group stores checkpoints on the server. When the data consumption process of one consumer stops, another consumer automatically takes over the process and continues the process from the checkpoint of the last consumption. You can start consumers on different servers. If a server stops or is damaged, a consumer on another server can take over the consumption process and continue the process from the checkpoint. To have sufficient consumers, you can start more consumers than the number of shards on different servers.

## HTTPS {#section_4eg_i6h_9bv .section}

To use HTTPS to encrypt the data transmitted between your program and Log Service, you must set the prefix of the service endpoint to `https://`, for example, `https://cn-beijing.log.aliyuncs.com`.

The server certificate \*.aliyuncs.com issued by GlobalSign. Most Linux and Windows servers are preconfigured to trust this certificate by default. If a server does not trust this certificate, you can visit the following website to download and install a valid certificate: [Certificates](https://success.outsystems.com/Support/Enterprise_Customers/Installation/Install_a_trusted_root_CA__or_self-signed_certificate).

