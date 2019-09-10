# Introduction {#concept_266013 .concept}

Log Service allows for sending logs to a security information and event management \(SIEM\) system. This ensures that all logs related to regulations and audits on Alibaba Cloud can be imported to your security operations center \(SOC\).

## Terms {#section_o72_0xg_i6l .section}

-   **SIEM**: security information and event management \(SIEM\) systems, such as Splunk and IBM QRadar.
-   **Splunk HEC**: Splunk HTTP Event Collector \(HEC\) can be used to receive and send logs over HTTP or HTTPS.

## Deployment suggestions {#section_b75_cww_0fu .section}

-   Hardware specifications:
    -   Operating system: Linux, such as Ubuntu x64.
    -   CPU: 2.0+ GHz x 8 cores.
    -   Memory: 32 GB \(recommended\) or 16 GB.
    -   Network interface controller \(NIC\): 1 Gbit/s.
    -   Available disk space: at least 2 GB. We recommend that you have an available disk space of 10 GB or greater.
-   Network specifications:

    The bandwidth between your network environment and Alibaba Cloud must be greater than the speed at which data is generated on Alibaba Cloud. Otherwise, logs cannot be consumed in real time. Assume that the peak speed for data generation is about twice that of the average speed and 1 TB of raw logs are generated every day. If data is compressed at a ratio of 5:1 before transmission, we recommend that you use a bandwidth of around 4 MB/s \(32 Mbit/s\).

-   Python: You can use Python to consume logs. For more information about using Java, see [Use a consumer group to consume logs](../../../../reseller.en-US/Real-time subscription and consumption/Consumption by consumer groups/Use a consumer group to consume logs.md#).

## Python SDK {#section_f4c_vlb_rlz .section}

-   We recommend that you use a standard CPython interpreter.
-   You can run the python3 -m pip install aliyun-log-python-sdk -U command to install the Log Service SDK for Python.
-   For more information about how to use the Log Service SDK for Python, see [User Guide](https://github.com/aliyun/aliyun-log-python-sdk/blob/master/README.md).

## Consumer library {#section_5cz_ogs_9vv .section}

The consumer library is an advanced log consumption mode in Log Service. The consumer library provides consumer groups to facilitate consumer management. In comparison to reading data by using the SDK, you can focus on the business logic rather than worrying about the implementation details of Log Service. In addition, the consumer library allows you to ignore failover and load balancing between consumers.

In Log Service, a Logstore can have multiple shards. The consumer library is used to allocate shards to consumers in a consumer group. The allocation rules are described as follows:

-   Each shard can only be allocated to one consumer.
-   One consumer can have multiple shards at the same time.

After a new consumer is added to a consumer group, the affiliation of shards with this consumer group is adjusted to balance consumption loads. However, the preceding allocation rules still apply and you cannot view the allocation details of shards.

The consumer library can also store checkpoints, which allows you to consume data starting from a breakpoint after a program crash is fixed. This ensures that the data is consumed only once.

Spark Streaming, Storm, and Flink Connector are all implemented based on the consumer library.

## Log sending methods {#section_kun_sg3_xki .section}

We recommend that you write the required program based on consumer groups to consume logs from Log Service in real time. Then, you can send logs to the SIEM system over HTTPS or Syslog.

-   For more information about how to send logs over HTTPS, see [Send logs to an SIEM system over HTTPS](reseller.en-US/Data shipping/Send logs to an SIEM system/Send logs to an SIEM system over HTTPS.md#).
-   For more information about how to send logs over Syslog, see [Send logs to an SIEM system over Syslog](reseller.en-US/Data shipping/Send logs to an SIEM system/Send logs to an SIEM system over Syslog.md#).

