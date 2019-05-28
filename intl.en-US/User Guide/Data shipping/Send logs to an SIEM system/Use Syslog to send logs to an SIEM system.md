# Use Syslog to send logs to an SIEM system {#concept_266003 .concept}

This topic describes the required configurations and related information for how to use Syslog to send logs to your Security Information and Event Management \(SIEM\) software. Syslog is a log path through which most physical devices \(such as switches, routers, and servers\) send logs. In addition, almost SIEM software \(including IBM Qradar and HP Arcsight\) can receive logs that are sent with Syslog.

## Background information {#section_spi_ceg_9eg .section}

-   **Syslog**: This protocol defines the standard format of logs according to the standard protocols [RFC5424](https://datatracker.ietf.org/doc/rfc5424/) and [RFC3164](https://tools.ietf.org/html/rfc3164).

    The RFC3164 protocol was released in 2001 and the RFC5424 protocol released in 2009 is an upgraded version. We recommend that you use the RFC5424 protocol. It has fixed many of the issues of the previous version and is compatible with the RFC3164 protocol.

-   **Syslog over TCP/TLS**: If the TCP and TLS protocols are supported by your SIEM software, this method can be used to transfer data to your software. We recommend that you use this method.

    Both the TCP and UDP protocols support Syslog. These two protocols can guarantee the stable data transmission. The [RFC5425](https://tools.ietf.org/html/rfc5425) protocol also defines the TLS protocol.

-   **Syslog facility**: This is a program component defined by the earlier versions of Unix. For more information, see [Facility](https://tools.ietf.org/html/rfc3164#section-4.1.1). You can select `user-level messages` as the default facility.
-   **Syslog severity**: This refers to [Message priorities](https://tools.ietf.org/html/rfc3164#section-4.1.1). You can set the priority for the specified logs as needed. By default, `Informational: informational messages` is used.

**Note:** This topic uses a few of code examples. If you want to view all the latest code, you can obtain them at [Github](https://github.com/aliyun/aliyun-log-python-sdk/blob/master/tests/consumer_group_examples/sync_data_to_syslog.py).

## Prerequisites {#section_23t_dez_fsi .section}

-   PyPy3 is used to run the program, instead of CPython \(the standard interpreter of Python\).
-   The Python SDK of Log Service is installed by using the pypy3 -m pip install aliyun-log-python-sdk -U command. For more information about Python SDK, see [User Guide](https://github.com/aliyun/aliyun-log-python-sdk/blob/master/README.md).

## Workflow {#section_jjc_xc8_yxd .section}

We recommend that you configure the required program that is based on the consumer groups in Log Service. Then, the program can send logs to the SIEM system by using Syslog.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/219724/155903374647807_en-US.png)

## Limits {#section_jdo_iyt_asu .section}

Up to ten consumer groups can be set for each Logstore in Log Service. If the system displays the error message `ConsumerGroupQuotaExceed`, we recommend that you log on to the Log Service console to delete the consumer groups that you do not need.

## Control logic of the program {#section_i21_dm8_qw1 .section}

The following code shows the control logic of the program:

``` {#codeblock_s54_bnd_k02}
def main():
    option, settings = get_monitor_option()

    logger.info("*** start to consume data...")
    worker = ConsumerWorker(SyncData, option, args=(settings,) )
    worker.start(join=True)

if __name__ == '__main__':
    main()
```

## Configurations and a code example of the program {#section_rzl_wkm_dw9 .section}

-   Configurations
    -   The log file of the program: This file is used to debug the program or diagnose any exceptions that may occur.
    -   The basic configurations of the program: These include the configurations used to connect to Log Service and those of the consumer group.
    -   The advanced configurations of the consumer group: These configurations are used to debug performance. We recommend that you do not modify them.
    -   The parameters and options related to the Syslog server.

        **Note:** If SIEM supports the Syslog path that is based on TCP or TLS, set the proto parameter of the program to TLS and set a valid SSL certificate.

-   A code example of the program

    You must follow the annotations of the code to customize it to meet your needs.

    ``` {#codeblock_381_2s8_5zx}
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
    
        # syslog options
        settings = {
                    "host": "1.2.3.4", # required.
                    "port": 514,       # required.
                    "protocol": "tcp", # required. Valid: tcp | udp | tls (only applicable to Python3)
                    "sep": "||",       # required. Set the separator to separate key-value pairs.
                    "cert_path": None, # optional. Set the directory where the TLS certificate is located.                
                    "timeout": 120,    # optional. Set the timeout. The default timeout is 120 seconds.
                    "facility": syslogclient.FAC_USER,  # optional. You can refer to the values of other syslogclient.FAC_*.
                    "severity": syslogclient.SEV_INFO,  # optional. You can refer to the values of other syslogclient.SEV_*.
                    "hostname": None,  # optional. Set the host name. By default, the name of the host that you use is used.
                    "tag": None        # optional. Set the tag. By default, a hyphen (-) is used.
                }
    
        return option, settings
    ```


## An example of the code used to consume and send data {#section_c75_u87_b4b .section}

The following code shows the settings about for to obtain data from Log Service and how to send the data to the Syslog server:

**Note:** You can customize the format of the code according to your needs.

``` {#codeblock_m9d_y1r_wxn}
from syslogclient import SyslogClientRFC5424 as SyslogClient

class SyncData(ConsumerProcessorBase):
    """
    This consumer obtains data from Log Service and send the data to the Syslog server.
    """
    def __init__(self, splunk_setting):
      """Initiate the Syslog server and its network connectivity."""
        super(SyncData, self).__init__()   # remember to call base's init

        assert target_setting, ValueError("You need to configure settings of remote target")
        assert isinstance(target_setting, dict), ValueError("The settings should be dict to include necessary address and confidentials.")

        self.option = target_setting
        self.protocol = self.option['protocol']
        self.timeout = int(self.option.get('timeout', 120))
        self.sep = self.option.get('sep', "||")
        self.host = self.option["host"]
        self.port = int(self.option.get('port', 514))
        self.cert_path=self.option.get('cert_path', None)

        # try connection
        with SyslogClient(self.host, self.port, proto=self.protocol, timeout=self.timeout, cert_path=self.cert_path) as client:
            pass

    def process(self, log_groups, check_point_tracker):
        logs = PullLogResponse.loggroups_to_flattern_list(log_groups, time_as_str=True, decode_bytes=True)
        logger.info("Get data from shard {0}, log count: {1}".format(self.shard_id, len(logs)))
        try:
            with SyslogClient(self.host, self.port, proto=self.protocol, timeout=self.timeout, cert_path=self.cert_path) as client:
                for log in logs:
                    # Put your sync code here to send to remote.
                    # the format of log is just a dict with example as below (Note, all strings are unicode):
                    #    Python2: {"__time__": "12312312", "__topic__": "topic", u"field1": u"value1", u"field2": u"value2"}
                    #    Python3: {"__time__": "12312312", "__topic__": "topic", "field1": "value1", "field2": "value2"}
                    # suppose we only care about audit log
                    timestamp = datetime.fromtimestamp(int(log[u'__time__']))
                    del log['__time__']

                    io = six.StringIO()
                    first = True
          # TODO： Modify the content to be formatted as needed. The data is transmitted by using key-value pairs which are separated by ||.
                    for k, v in six.iteritems(log):
                        io.write("{0}{1}={2}".format(self.sep, k, v))

                    data = io.getvalue()

          # TODO： Modify the facility or severity as needed.
                    client.log(data, facility=self.option.get("facility", None), severity=self.option.get("severity", None), timestamp=timestamp, program=self.option.get("tag", None), hostname=self.option.get("hostname", None))

        except Exception as err:
            logger.debug("Failed to connect to remote syslog server ({0}). Exception: {1}".format(self.option, err))

            # TODO: Add the code to process errors. For example, you can add the code to retry or send a notification in response to an error.
            raise err

        logger.info("Complete send data to remote")

        self.save_checkpoint(check_point_tracker)
```

## A example of the code used to start the program {#section_303_b2r_hey .section}

Assume that the program is named sync\_data.py. The code used to start the program is as follows:

``` {#codeblock_gpz_k8p_3z1}
export SLS_ENDPOINT=<Endpoint of your region>
export SLS_AK_ID=<YOUR AK ID>
export SLS_AK_KEY=<YOUR AK KEY>
export SLS_PROJECT=<SLS Project Name>
export SLS_LOGSTORE=<SLS Logstore Name>
export SLS_CG=<SLS consumer group name. You can set it to syc_data.>

pypy3 sync_data.py
```

## View and monitor data consumption {#section_agz_8uj_89o .section}

-   You can log on to the Log Service console to view the status of a consumer group. For more information, see [View consumer group status](reseller.en-US/User Guide/Real-time subscription and consumption/Consumption by consumer groups/View consumer group status.md#).
-   You can log on to the CloudMonitor to view delays associated with consumer groups and configure corresponding alarms. For more information, see [Consumer group delay](reseller.en-US/User Guide/Real-time subscription and consumption/Consumption by consumer groups/Consumer group - Monitoring alarm.md#).

## Run multiple consumers {#section_ecj_uaq_1t6 .section}

For a program that is based on consumer groups, you can start the program multiple times to run multiple consumers.

``` {#codeblock_kwm_fr3_hdm}
nohup pypy3 sync_data.py &
nohup pypy3 sync_data.py &
nohup pypy3 sync_data.py &
...
```

**Note:** The data of one shard can be consumed by only one consumer. If a Logstore contains ten shards, then up to ten consumers can consume the data of all of the shards.

## Throughput test scenario {#section_hx8_avb_1hi .section}

Assume that you have an instance with a single core CPU. The data of raw logs is consumed at a rate of 5 MB/s only if the following conditions are met:

-   No limit of the bandwidth and rate to receive data exists.
-   PyPy3 is used to run the following example codes.
-   One consumer uses about 10% resources of a single-core CPU.

Therefore, ten consumers that use the same single-core CPU can consume the data of raw logs at the rate of 50 MB/s. Then, up to 4 TB data of raw logs can be consumed on such a CPU.

## High availability scenario {#section_4ky_8sv_ilu .section}

A consumer group saves check-points on the server end. When one consumer stops, another consumer automatically takes over the data consumption task and continues the task from the check-point where the preceding consumer stopped.

Therefore, to obtain high availability, you can start multiple consumers that are located in different servers. That way, if one of these servers stops or is damaged, a consumer located in one of the other servers can take over the consumption task and continue the task from the check-point where the preceding consumer stopped.

To ensure sufficient consumers are available, set the number of the consumers that you start in different servers to be greater than the number of the shards.

