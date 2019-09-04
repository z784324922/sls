# Overview {#concept_c34_tnq_zdb .concept}

After LogHub collects logs, Log Service consumes these logs in the following ways.

|Method|Scenario|Real-time performance|Storage duration|
|:-----|:-------|:--------------------|:---------------|
|Real-time consumption \(LogHub\)|StreamCompute and Realtime Compute|Real time|Custom|
|Index and query \(LogSearch\)|Applicable to online query of recent hot data|Real time \(delayed for one second in 99.99% cases, and up to three seconds\)|Custom|
|Shipping and storage \(LogShipper\)|Applicable to full log storage for offline analysis|Delayed for 5-30 minutes|Depends on the storage system|

## Real-time consumption {#section_xjl_hdc_ry .section}

LogHub provides the API operation to pull logs and support real-time log consumption. Log Service consumes logs in a shard in the following steps:

1.  Obtain a cursor based on conditions such as time, Begin, and End.
2.  Read logs by using the cursor and step, and return the next cursor.
3.  Move the cursor continuously to consume logs.

**Note:** To consume or query logs, Log Service needs to read logs. For more information about the differences between consuming logs and querying logs, see [Differences between log consumption and log query](../../../../reseller.en-US/.md#).

**Consume logs by using SDKs**

Log Service provides SDKs in multiple programming languages such as Java, Python, and Go. These SDKs support log consumption based on API operations. For more information about the SDKs, see [Overview](../../../../reseller.en-US/SDK Reference/Basic Descriptions /Overview.md).

**Consume logs by using consumer groups**

[Consumer groups](reseller.en-US/Real-time subscription and consumption/Consumption by consumer groups/Use a consumer group to consume logs.md) are the advanced method Log Service provides for LogHub consumers to consume logs. Consumer groups provide a lightweight computing framework that allows multiple consumers to concurrently consume data in a Logstore. Consumer groups can also automatically assign shards, maintain the order of log processing, and resume transmission from a breakpoint. Go, Python, and Java SDKs support consumer groups.

**Consume logs by using StreamCompute** 

-   Use the [Spark Streaming client](reseller.en-US/Real-time subscription and consumption/Use Spark Streaming to consume LogHub logs.md) to consume logs.
-   Use [Storm Spout](reseller.en-US/Real-time subscription and consumption/Use Storm to consume LogHub logs.md) to consume logs.
-   Use the [Flink connector](reseller.en-US/Real-time subscription and consumption/Use Flink to consume LogHub logs.md) to consume logs. The Flink connector consists of the consumer and producer.

**Consume logs by using cloud services** 

-   Use [CloudMonitor](reseller.en-US/Real-time subscription and consumption/Use CloudMonitor to consume LogHub logs.md) to consume logs in monitoring scenarios.
-   Use [Function Compute](reseller.en-US/Real-time subscription and consumption/Use Fuction Compute to cosume LogHub Logs/Development guide.md#) to consume logs.
-   Use E-MapReduce to consume logs. For more information, see [Use Storm to consume LogHub logs](reseller.en-US/Real-time subscription and consumption/Use Storm to consume LogHub logs.md) and [Use Spark Streaming to consume LogHub logs](reseller.en-US/Real-time subscription and consumption/Use Spark Streaming to consume LogHub logs.md).

**Consume logs by using open-source services**

[Use Flume to consume LogHub logs](reseller.en-US/Real-time subscription and consumption/Use Flume to consume LogHub logs.md#): you can use Flume to consume logs and import logs to Hadoop file system \(HDFS\) instances.

## Query and analysis {#section_jrv_ldc_ry .section}

For more information, see [Overview](reseller.en-US/Index and query/Overview.md). You can query and analyze logs in the following ways:

-   Query logs in the Log Service console. For more information, see [Overview](reseller.en-US/Index and query/Overview.md).
-   Query logs by using the SDKs or API operations of Log Service. Log Service provides HTTP-based RESTful API operations. The API operations support full-featured log queries. For more information, see [Overview](../../../../reseller.en-US/API Reference/Overview.md).

## Shipping and storage {#section_hzp_ndc_ry .section}

-   [Ship logs to Object Storage Service \(OSS\)](reseller.en-US/Data shipping/Ship logs to OSS/Ship logs to OSS.md): stores logs for a long period or analyzes logs based on E-MapReduce.
-   [Use Function Compute to customize shipping](reseller.en-US/Real-time subscription and consumption/Use Fuction Compute to cosume LogHub Logs/Configure Function Compute log consumption.md).
-   [Ship data to MaxCompute by using DataWorks](reseller.en-US/Data shipping/Ship data to MaxCompute/Ship data to MaxCompute by using DataWorks.md): ships logs to MaxCompute by using Data Integration of DataWorks to perform big data analysis.

## Other method for log consumption {#section_qlf_pdc_ry .section}

Security log service: Log Service connects to cloud security services and uses an independent software vendor \(ISV\) to consume logs of cloud services.

