# Overview {#concept_266013 .concept}

This topic describes some of the requirements and features involved if you use the POST method to send logs to an SIEM system.

## Terms {#section_o72_0xg_i6l .section}

-   **SIEM**: an abbreviation of Security Information and Event Management. It is a security management approach that can be implemented with Splunk and IBM QRadar.
-   **Splunk HEC**: the Splunk HTTP Event Collector. It is an HTTP or HTTPS interface that is used to receive logs.

## Recommended specifications for the target server {#section_b75_cww_0fu .section}

-   Hardware specifications

    -   Linux OS \(for example, Ubuntu x64\)
    -   At least one 8-core GPU running a minimum of 2.0 GHz
    -   At least 16 GB memory
    -   At least one NIC that transmits 1 GB/s
    -   At least one disk of 2 GB.
    **Note:** 

    -   Although 16 GB memory is the minimum specification required, we recommend that you upgrade to 32 GB of memory for better performance.
    -   Although 2 GB is the minimum storage specification required for the disk, we recommend that you upgrade to 10 GB storage for better performance.
-   Network

    The bandwidth value of the network that connects your server with Alibaba Cloud must be greater than the value of the rate at which data are generated in Alibaba Cloud. Otherwise, the generated data cannot be consumed in real time.

    We recommend that you set the bandwidth to 4 MB/s \(32 Mbps\) for the scenario that meets the following conditions:

    -   Data are generated at an even rate.
    -   The peak rate of data generation is twice of the even rate.
    -   Log data of 1 TB are generated each day.
    -   The log data to be transmitted are compressed by a 5:1 compression ratio.
-   Languages

    Both Python and Java can be used to compile the required program.

    In the two examples \([Use HTTPS to send logs to an SIEM system](reseller.en-US/User Guide/Data shipping/Send logs to an SIEM system/Use HTTPS to send logs to an SIEM system.md#) and [Use Syslog to send logs to an SIEM system](reseller.en-US/User Guide/Data shipping/Send logs to an SIEM system/Use Syslog to send logs to an SIEM system.md#)\), Python is used.

    For information about how the example in which Java is used, see [Use a consumer group to consume logs](reseller.en-US/User Guide/Real-time subscription and consumption/Consumption by consumer groups/Use a consumer group to consume logs.md#).


## Consumer library {#section_5cz_ogs_9vv .section}

The consumer library provides the advanced mode to consume logs in Log Service. Specifically, the consumer library uses consumer groups to abstract and manage the data consumption. In contrast, SDK is used to directly read data. With the consumer library, you can focus on how the data is consumed and sent. You can ignore to the details about how Log Service works, or the load balancing and failover event between consumers.

One Logstore in Log Service contains multiple shards. The consumer Library assign shards to the consumers of one consumer group by following these rules:

-   Each shard can only be assigned to one consumer.
-   Multiple shards can be assigned to the same consumer.

**Note:** 

-   These rules take effect each time when shards are assigned to consumers in a consumer group.
-   When a new consumer is added in a consumer group, the shards assigned to the consumers before will be reassigned to all the consumers in the consumer group. This is a method of load balancing in the consumer group.
-   The process in which shards are assigned to consumers is transparent.

In addition, the consumer library can also save check-points. This feature helps data consumption continue from the point where a failure occurred, avoiding re-consumption of the same data.

Spark Streaming, Storm, and Flink Connector all works on the base of the consumer library.

## Methods to send logs to an SIEM system {#section_kun_sg3_xki .section}

We recommend that you use consumer groups in Log Service to configure the required program to consume log data in real time, and then use send log data to an SIEM system through HTTPS or Syslog.

For more information, see [Use HTTPS to send logs to an SIEM system](reseller.en-US/User Guide/Data shipping/Send logs to an SIEM system/Use HTTPS to send logs to an SIEM system.md#) and [Use Syslog to send logs to an SIEM system](reseller.en-US/User Guide/Data shipping/Send logs to an SIEM system/Use Syslog to send logs to an SIEM system.md#).

