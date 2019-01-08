# Overview {#concept_c34_tnq_zdb .concept}

Logs collected to the LogHub of the Log Service can be consumed in the following three methods:

|Approach|Scenario|Real time|Storage period|
|:-------|:-------|:--------|:-------------|
|Real-time consumption \(loghub\)|Stream computing and real-time computing|Real-time|Customize|
|Query and analysis \(LogSearch/Analytics\)|Online query and analysis|Real-time \(less than one second in 99.99% cases\)|Customize|
|Shipping and storage \(LogShipper\)|Full log storage for offline analysis|5–30 minutes|Depends on the storage system|

## Real-time consumption {#section_xjl_hdc_ry .section}

Logs are consumed after being written. Both log consumption and log query require the capability of reading logs. Logs in a shard are consumed as follows.

1.  Obtain a cursor based on a set of criteria such as time, Begin, and End.
2.  The system reads logs based on the cursor and step and returns the next cursor.
3.  Moves the cursor continuously to consume logs.

Besides the basic APIs, Log Service provides many methods to consume logs, such as SDKs, Storm spout, Spark Streaming client, Flink connector, consumer library, and Web console.

-   Use [Spark Streaming Client](reseller.en-US/User Guide/Real-time subscription and consumption/Use Spark Streaming to consume LogHub logs.md) to consume logs.
-   Use [Storm Spout](reseller.en-US/User Guide/Real-time subscription and consumption/Use Storm to consume LogHub logs.md) to consume logs.
-   Use [Flink Connector](reseller.en-US/User Guide/Real-time subscription and consumption/Use Flink to consume LogHub logs.md), including Flink consumer and Flink producer to consume logs.
-   Use [LogHub Consumer  Library](reseller.en-US/User Guide/Real-time subscription and consumption/Consumption by consumer groups/Consumer group - Usage.md)  to consume logs.  The consumer library is an advanced mode for LogHub consumers, which provides a lightweight computing framework and solves the issue of automatic shard allocation and order preservation when multiple consumers consume a Logstore at the same time.
-   Use [SDKs](../../../../../reseller.en-US/SDK Reference/Basic Descriptions /Overview .md) to consume logs. Log Service provides SDKs in multiple languages \(Java and Python\) that support the log consumption APIs. For more information about SDKs, see [Log Service SDK](../../../../../reseller.en-US/SDK Reference/Basic Descriptions /Overview .md).
-   Use cloud products to consume logs:
    -   [Use CloudMonitor to consume logs](reseller.en-US/User Guide/Real-time subscription and consumption/Use CloudMonitor to consume LogHub logs.md): Monitoring scenario.
    -   Use E-MapReduce to consume logs: See [Storm](reseller.en-US/User Guide/Real-time subscription and consumption/Use Storm to consume LogHub logs.md), [Spark Streaming](reseller.en-US/User Guide/Real-time subscription and consumption/Use Spark Streaming to consume LogHub logs.md).
    -   Use [Command-line tools CLI](http://aliyun-log-cli.readthedocs.io/en/latest/tutorials/tutorial_pull_logs.html)  to consume logs.

## Query and analysis {#section_jrv_ldc_ry .section}

[Overview of real-time query and analysis](reseller.en-US/User Guide/Index and query/Overview.md):

-   Query logs in the Log Service console: See [Query logs](reseller.en-US/User Guide/Index and query/Overview.md).
-   Query logs by using Log Service SDKs/APIs: Log Service provides RESTful APIs that are implemented based on HTTP protocol.  The Log Service APIs also provide a full-featured log query API.  For more information, see [Log Service APIs](../../../../../reseller.en-US/API Reference/Overview.md).

## Shipping and storage {#section_hzp_ndc_ry .section}

-   [Ship logs to OSS](reseller.en-US/User Guide/Data shipping/Ship logs to OSS/Ship logs to OSS.md): Store logs for a long term or use E-MapReduce to analyze logs.Analysis log.
-   [Use function calculations for custom delivery](reseller.en-US/User Guide/Real-time subscription and consumption/Use Fuction Compute to cosume LogHub Logs/Configure Function Compute log consumption.md).

## Others {#section_qlf_pdc_ry .section}

Secure Log Service: Log Service interconnects with cloud security products and uses ISV to consume logs of cloud products.

