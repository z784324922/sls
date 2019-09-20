# Build a monitoring system {#concept_29094_zh .concept}

As the important infrastructure of Alibaba Cloud, Log Service supports the collection and distribution of log data in all clusters of Alibaba Cloud. Many applications, such as Table Store, MaxCompute, and CNZZ, use Logtail of Log Service to collect log data, call API operations to consume log data, and then import the data to the downstream real-time statistics system or offline system for further analysis and statistics. As the infrastructure, Log Service has the following features:

-   Reliability: After serving the internal users of Alibaba Group and taking on challenges during the Double 11 Shopping Festival for many years, Log Service has proven its capability to ensure data reliability and integrity.
-   Scalability: Log Service can add shards to quickly and dynamically extend the processing capability when data traffic increases.
-   Convenience: Log Service supports one-click management. For example, it allows you to collect logs from tens of thousands of machines with one click.

Log Service helps you collect logs, unifies the log format, and provides API operations for downstream systems to consume data. Downstream systems can be connected to multiple systems to consume data in various ways. For example, they can use Spark and Storm to compute data in real time, or use ElasticSearch to query data. In this way, data is collected once and consumed multiple times. Among many data consumption scenarios, monitoring is the most common one. This topic introduces a monitoring system that is provided by Alibaba Cloud based on Log Service.

Log Service collects monitoring data from all clusters as logs. It solves the problems of multi-cluster management and log collection from heterogeneous systems. Monitoring data is transformed into logs of the same format and sent to Log Service.

Log Service provides the following features for the monitoring system:

-   Unified machine management: After Logtail is installed on each server, all subsequent operations are performed in Log Service.
-   Unified configuration management: You only need to configure which log files are to be collected in Log Service. The configuration automatically applies to all relevant servers.
-   Structured data: All data is formatted to fit the data model of Log Service to facilitate downstream consumption.
-   Elastic service capability: Log Service allows you to read and write a large amount of data.

![](images/31900_en-US.png "Architecture of the monitoring system ")

## Build the monitoring system {#section_yw4_4dl_5fb .section}

1.  Collect monitoring data.

    Configure log collection in Log Service to ensure that logs are collected and sent to Log Service.

2.  Use middleware and call API operations to consume data.

    Use the SDK to call the PullLogs operation to consume log data in Log Service in batches and synchronize the data to the downstream real-time computing system.

3.  Build a Storm real-time computing system.

    Select Storm or another type of real-time computing system, configure computing rules, select the metrics for computing, and then write computing results to Table Store.

4.  Display monitoring information.

    Read the monitoring data stored in Table Store and display the monitoring data on the front-end GUI. Alternatively, read the monitoring data and configure alerts based on the data results.


