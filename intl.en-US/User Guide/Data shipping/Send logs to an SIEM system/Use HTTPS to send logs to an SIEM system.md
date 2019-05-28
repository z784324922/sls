# Use HTTPS to send logs to an SIEM system {#concept_265988 .concept}

This topic uses an example to describe how to send Alibaba Cloud logs to an SIEM system \(Splunk in this example\) through the HTTP Event Collector \(HEC\).

## Prerequisites {#section_y95_12k_24t .section}

-   Splunk Enteprise Security is deployed on-premises to support the SIEM system, rather than in the cloud.
-   This SIEM system cannot be accessed through the Internet because there are no corresponding interfaces configured for Internet access.
-   PyPy3 is used to run the program, instead of CPython \(the standard interpreter of Python\).
-   The Python SDK of Log Service is installed by using the pypy3 -m pip install aliyun-log-python-sdk -U command. For more information about Python SDK, see [User Guide](https://github.com/aliyun/aliyun-log-python-sdk/blob/master/README.md).

**Note:** This topic uses a few of code examples. If you want to view all the latest code, you can obtain them at [Github](https://github.com/aliyun/aliyun-log-python-sdk/blob/master/tests/consumer_group_examples/sync_data_to_splunk.py), or [Github \(applicable to the Logstore that contains multiple data sources\)](https://github.com/aliyun/aliyun-log-python-sdk/blob/master/tests/consumer_group_examples/sync_data_to_splunk_multiple_logstores.py).

## Workflow {#section_mny_9pq_yc8 .section}

We recommend that you use the consumer groups in Log Service to configure the required program to consume log data in real time. Then, the program can send logs to the SIEM system \(Splunk\) by using the Splunk API \(that is, HEC\).

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/219720/155903369747818_en-US.png)

## Limits {#section_60k_e61_es3 .section}

For each Logstore in Log Service, up to ten consumer groups can be set. If the system displays the error message `ConsumerGroupQuotaExceed`, we recommend that you log on to the Log Service console to delete the consumer groups that you do not need.

## Control logic of the program {#section_o11_g58_pr1 .section}

The following code shows the control logic of the program:

``` {#codeblock_3sp_5he_rlh}
def main():
    option, settings = get_monitor_option()

    logger.info("*** start to consume data...")
    worker = ConsumerWorker(SyncData, option, args=(settings,) )
    worker.start(join=True)

if __name__ == '__main__':
    main()
```

## Configurations and a code example of the program {#section_c31_4bd_axp .section}

-   Configurations
    -   The log file of the program: This file is used to debug the program or diagnose any exceptions that may occur.
    -   The basic configurations of the program: These include the configurations used to connect to Log Service and those of the consumer group.
    -   The advanced configurations of the consumer group: These configurations are used to debug performance. We recommend that you do not modify them.
    -   The parameters and options related to the SIEM \(Splunk\).
-   A code example of the program

    You must follow the annotations of the code to customize it to meet your needs.

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
        # Basic options
        ##########################
    
        # Load parameters and options of SLS from environment variables.
        endpoint = os.environ.get('SLS_ENDPOINT', '')
        accessKeyId = os.environ.get('SLS_AK_ID', '')
        accessKey = os.environ.get('SLS_AK_KEY', '')
        project = os.environ.get('SLS_PROJECT', '')
        logstore = os.environ.get('SLS_LOGSTORE', '')
        consumer_group = os.environ.get('SLS_CG', '')
    
        # This parameter specifies the starting time when data consumption is initialized.
        # You can use begin or end when you set this parameter. The time can be set to use a specific ISO format.
        cursor_start_time = "2018-12-26 0:0:0"
    
        ##########################
        # Advanced options
        ##########################
    
        # Do not configure the consumer name, especially when you need to run this program in parallel.
        consumer_name = "{0}-{1}".format(consumer_group, current_process().pid)
    
        # Set the heartbeat interval. If the server does not receive the heartbeat report from the specified shard within two times of the specified interval, the server determines the consumer is off line and will reassign its task to another consumer.
        # We recommend that you set a greater interval when the network provides low performance.
        heartbeat_interval = 20
    
        # The maximum interval between two times of data consumption. If data are generated quickly, do not adjust this setting.
        data_fetch_interval = 1
    
        # Create a consumer group that contains a consumer.
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
                    'https': False,              # optional. bool
                    'timeout': 120,             # optional. int
                    'ssl_verify': True,         # optional. bool
                    "sourcetype": "",            # optional.
                    "index": "",                # optional.
                    "source": "",               # optional.
                }
    
        return option, settings
    ```


## An example of the code used to consume and send data {#section_fvf_cwj_0hh .section}

The following code shows the settings about for to obtain data from Log Service and how to send the data to the Splunk:

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
    This consumer obtains data from Log Service and send the data to Splunk.
    """
    def __init__(self, splunk_setting):
      """Initiate the Splunk server and its network connectivity."""
        super(SyncData, self).__init__()

        assert splunk_setting, ValueError("You need to configure settings of remote target")
        assert isinstance(splunk_setting, dict), ValueError("The settings should be dict to include necessary address and confidentials.")

        self.option = splunk_setting
        self.timeout = self.option.get("timeout", 120)

        # Test Splunk connectivity.
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

                    # TODO: Add the code to process errors. For example, you can add the code to retry or send a notification in response to an error.
            raise err

        logger.info("Complete send data to remote")

        self.save_checkpoint(check_point_tracker)
```

## A example of the code used to start the program {#section_0lc_8w7_r9f .section}

Assume that the program is named sync\_data.py. The code used to start the program is as follows:

``` {#codeblock_9hl_3ye_qkx}
export SLS_ENDPOINT=<Endpoint of your region>
export SLS_AK_ID=<YOUR AK ID>
export SLS_AK_KEY=<YOUR AK KEY>
export SLS_PROJECT=<SLS Project Name>
export SLS_LOGSTORE=<SLS Logstore Name>
export SLS_CG=<SLS consumer group name. You can set it to syc_data.>

pypy3 sync_data.py
```

## An example code of the Logstore that contains multiple sources {#section_zis_947_7vs .section}

For the Logstore that contains multiple sources, you must set an executor to avoid process broken down. For more information about related examples, see[Send logs from a multi-source Logstore to Splunk](https://github.com/aliyun/aliyun-log-python-sdk/blob/master/tests/consumer_group_examples/sync_data_to_splunk_multiple_logstores.py).

**Note:** You must pay attention to the changes to the primary functions.

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

## View and monitor data consumption {#section_4cl_2l4_qol .section}

-   You can log on to the Log Service console to view the status of a consumer group. For more information, see [View consumer group status](reseller.en-US/User Guide/Real-time subscription and consumption/Consumption by consumer groups/View consumer group status.md#).
-   You can log on to the CloudMonitor to view delays associated with consumer groups and configure corresponding alarms. For more information, see [Consumer group delay](reseller.en-US/User Guide/Real-time subscription and consumption/Consumption by consumer groups/Consumer group - Monitoring alarm.md#).

## Run multiple consumers {#section_vba_w0q_hae .section}

For a program that is based on consumer groups, you can start the program multiple times to run multiple consumers.

``` {#codeblock_vhe_afy_7aq}
nohup pypy3 sync_data.py &
nohup pypy3 sync_data.py &
nohup pypy3 sync_data.py &
...
```

**Note:** The data of one shard can be consumed by only one consumer. If a Logstore contains ten shards, then up to ten consumers can consume the data of all of the shards.

## Throughput test scenario {#section_f4x_i0q_2wo .section}

Assume that you have an instance with a single core CPU. The data of raw logs is consumed at a rate of 5 MB/s only if the following conditions are met:

-   The bandwidth and rate to receive data is not limited.
-   PyPy3 is used to run the following example codes.
-   One consumer uses about 10% resources of a single-core CPU.

Therefore, ten consumers that use the same single-core CPU can consume the data of raw logs at the rate of 50 MB/s. Then, up to 4 TB data of raw logs can be consumed on such a CPU.

## High availability scenario {#section_gck_pa6_qpx .section}

A consumer group saves check-points on the server end. When one consumer stops, another consumer automatically takes over the data consumption task and continues the task from the check-point where the preceding consumer stopped.

Therefore, to achieve high availability, start multiple consumers that are located in different servers. That way, if one of these servers stops or is damaged, a consumer located in one of the other servers can take over the consumption task and continue the task from the check-point where the preceding consumer stopped.

To ensure sufficient consumers are available, set the number of the consumers that you start in different servers to be greater than the number of the shards.

## HTTPS {#section_4eg_i6h_9bv .section}

To use HTTPS to encrypt the data transmitted between your configured program and Log Service, you must set the prefix of the service endpoint to`https://`. For example,`https://cn-beijing.log.aliyuncs.com`.

The server certificate \*.aliyuncs.com issued by GlobalSign. Generally, most Linux or Windows server automatically trusts this certificate. If a server does not trust this certificate, you can download the valid certificate at[Certificates](https://success.outsystems.com/Support/Enterprise_Customers/Installation/Install_a_trusted_root_CA__or_self-signed_certificate).

