# Log types {#reference_r1y_lvp_m2b .reference}

The service log function records multiple log types. This topic describes the fields used by each type of log in detail.

## Log types {#section_wcq_x4r_m2b .section}

When you enable the service log function, you can choose one of the following log types:

-   **Operational logs**: Record all operations to yourresources in a Project, including create, modify, delete, update, write, and read operations. They are stored in the **internal-operation\_log** Logstore of a specified Project.
-   **Other logs**: Include metering logs at a Logstore granularity, which are consumption group delay logs, Logtail error information, heartbeat information, and statistical logs. They are stored in the **internal-diagnostic\_log** Logstore of a specified Project.

|Log type|Logstore|Log source|Description|
|:-------|:-------|:---------|:----------|
|Operational logs|internal-operation\_log|[User operational logs](#)|All API requests and operational logs, including requests sent from consoles, consumer groups, SDKs, and clients.|
|Other logs|internal-diagnostic\_log|[Consumer group snapshot logs](#)|The consumption delay logs of a consumption group, which are reported every 2 minutes. To query snapshot logs of a certain consumption group, you need to specify `__topic__: consumergroup_log` in the query statement.|
|[Logtail alarm logs](#)|The Logtail error logs, which are recorded every 30 seconds. The errors of the same type occurring within 30 seconds only accumulate the total number of errors, but only one error message is randomly selected and sent. To query a certain alarm log, you need to specify `__topic__: logtail_alarm` in the query statement.|
|[Logtail collection logs](#)|The Logtail collection logs, which are recorded every 10 minutes. To query a certain Logtail collection log, you need to specify `__topic__: logtail_profile` in the query statement.|
|[Metering logs](#)|The user metering logs, which are collected every hour. The logs include information relating to storage space at a Logstore granularity, read and write traffic, index traffic, and the number of requests. To query a certain metering log, you need to specify `__topic__: metering` in the query statement.|
|[Logtail status logs](#)|The status logs that are regularly reported by Logtail. The logs are recorded every minute. To query a certain Logtail status log, you need to specify `__topic__: logtail_status` in the query statement.|

## User operational logs {#section_gnn_3pr_m2b .section}

Operational logs involve data read and write operations and other operations on various resources according to the `Method` field.

|Type|Method|
|:---|:-----|
|Data read operations|By calling the following APIs:-   GetLogStoreHistogram
-   GetLogStoreLogs
-   PullData
-   GetCursor
-   GetCursorTime

|
|Data write operations|By calling the following APIs:-   PostLogStoreLogs
-   WebTracking

|
|Other operations on resources|By calling the CreateProject and DeleteProject APIs.|

## Common fields {#section_h1x_jpr_m2b .section}

The following table lists common fields that can be used by various operations.

|Field|Description|Example|
|:----|:----------|:------|
|APIVersion|The API version|0.6.0|
|InvokerUid|The account ID of the user who performs the operation|1759218115323050|
|NetworkOut|The inbound Internet read traffic in bytes|10|
|Latency|The request delay in microseconds|123279|
|LogStore|The name of a Logstore|logstore-1|
|Method|The method being used|GetLogStoreLogs|
|Project|The name of a Project|project-1|
|NetOutFlow|The read traffic in bytes|120|
|RequestId|The request ID|8AEADC8B0AF2FA2592C9509E|
|SourceIP|The IP address of the client that sent the request|1.2.3.4|
|Status|The response status code|200|
|UserAgent|The user agent on the client|sls-java-sdk-v-0.6.1|

## Data read fields {#section_htk_npr_m2b .section}

The following tables lists the fields specific to read requests.

|Field|Description|Example|
|:----|:----------|:------|
|BeginTime|The request start time in Unix timestamps|1523868463|
|DataStatus|The response status, including `Complete`, `OK`, and `Unknown`.|OK|
|EndTime|The request end time in Unix timestamps|1523869363|
|Offset|The offset of the GetLog request|20|
|Query|The original query statement|UserAgent: \[consumer-group-java\]\*|
|RequestLines|The number of lines that are expected to be returned|100|
|ResponseLines|The number of lines of the query results|100|
|Reverse|Indicates whether to return logs in reverse order of log timestamps, where:-   1 indicates the reverse order.
-   0 indicates the normal order.

The default value is 0.|0|
|TermUnit|The number of words included in a search statement|0|
|Topic|The name of the read topic|topic-1|

## Data write fields {#section_ijr_4pr_m2b .section}

The following tables lists the fields specific to write requests.

|Field|Description|Example|
|:----|:----------|:------|
|InFlow|The number of the original write bytes|200|
|InputLines|The number of requested write lines|10|
|NetInflow|The number of write bytes after compression|100|
|Shard|The ID of the Shard to which data is written|1|
|Topic|The name of the topic to which data is written|topic-1|

## Consumption group snapshot logs {#section_c5n_ppr_m2b .section}

|Field|Description|Example|
|:----|:----------|:------|
|consumer\_group|The name of a consumption group|consumer-group-1|
|fallbehind|The period of time from the current consumption point to the most recent write log \(in seconds\)|12345|
|logstore|The name of a Logstore|logstore-1|
|project|The name of a Project|project-1|
|shard|The ID of a Shard|1|

## Logtail alarm logs {#section_sxm_qpr_m2b .section}

|Field|Description|Example|
|:----|:----------|:------|
|alarm\_count|The number of alarms in the sampling window|10|
|alarm\_message|The sample of original logs that triggered the alarm|M\_INFO\_COL,all\_status\_monitor,T22380,0,2018-04-17 10:48:25.0,AY66K,AM5,2018-04-17 10:48:25.0,2018-04-17 10:48:30.561,i- 23xebl5ni.1569395.715455,901,00789b|
|alarm\_type|The alarm type|REGISTER\_INOTIFY\_FAIL\_ALARM|
|logstore|The name of a Logstore|logstore-1|
|source\_ip|The IP address of the server at which Logtail runs|1.2.3.4|
|os|The operating system, such as Linux or Windows|Linux|
|project|The name of a Project|project-1|
|version|The Logtail version|0.14.2|

## Logtail collection logs {#section_ybn_rpr_m2b .section}

Based on the `file_name` field, Logtail collection logs can be divided into two types: single-file statistics and Logstore-level logs. For the second type, `logstore_statistics` in `file_name` indicates that the log collects statistics for the entire Logstore. For this log type, file-related fields, such as `file_dev` and `file_inode`, can be disregarded. The following tables lists fields that are used in Logtail collection logs.

|Field|Description|Example|
|:----|:----------|:------|
|logstore|The name of a Logstore|logstore-1|
|config\_name|The name of a Logtail Config, which is unique and consists of `##Config version##Project name$Config name`.|\#\#1.0\#\#project-1$logstore-1|
|error\_line|The raw log that caused the error|M\_INFO\_COL,all\_status\_monitor,T22380,0,2018-04-17 10:48:25.0,AY66K,AM5,2018-04-17 10:48:25.0,2018-04-17 10:48:30.561,i- 23xebl5ni.1569395.715455,901,00789b|
|file\_dev|The device ID of the log file|123|
|file\_inode|The inode of the log file|124|
|file\_name|The storage path of the log file or `logstore_statistics`|/abc/file\_1|
|file\_size|The size of the log file \(in bytes\)|12345|
|history\_data\_failures|The number of historical processing failures|0|
|last\_read\_time|The latest read time in the window \(in Unix timestamps\)|1525346677|
|project|The name of a Project|project-1|
|logtail\_version|The Logtail version|0.14.2|
|os|The OS|Windows|
|parse\_failures|The number of lines with a parsing failure in the log in the current window|12|
|read\_avg\_delay|The average value of the difference between the offset at every read and the file size in the current window|65|
|read\_count|The number of log reads in the current window|10|
|read\_offset|The offset of the current file read \(in bytes\)|12345|
|regex\_match\_failures|The number of regular expression matching failures|1|
|send\_failures|The number of request sending failures in the current window|12|
|source\_ip|The IP address of the server where Logtail runs|1.2.3.4|
|succeed\_lines|The number of log lines that are successfully processed|123|
|time\_format\_failures|The number of log time matching failures|122|
|total\_bytes|The total number of reads \(in bytes\)|12345|

The following table lists the fields that are valid only when `file_name` is `logstore_statistics`.

|Field|Description|Example|
|:----|:----------|:------|
|send\_block\_flag|Indicates whether the sending queue is blocked at the end of the window.|false|
|send\_discard\_error|The number of discarded data packets due to data exceptions or permission unavailability in the current window|0|
|send\_network\_error|The number of data packets that were not sent due to network errors in the current window|12|
|send\_queue\_size|The number of unsent data packets in the current send queue at the end of the window|3|
|send\_quota\_error|The number of data packets that were not sent due to quota insufficiency in the current window|0|
|send\_success\_count|The number of data packets that are successfully sent in the current window|12345|
|sender\_valid\_flag|Indicates whether the sending flag of the current Logstore is valid at the end of the window, where:-   The value `true` indicates that the flag is valid.
-   The value `false` indicates that the flag may be forbidden due to network or quota errors.

|true|
|max\_send\_success\_time|The maximum time period during which data can be successfully sent in the window \(in Unix timestamps\)|1525342763|
|max\_unsend\_time|The maximum time period during which no data is sent in the sending queue at the end of the window \(in Unix timestamps\). The field is 0 when the queue is empty.|1525342764|
|min\_unsend\_time|The minimum time period during which no data is sent in the sending queue at the end of the window \(in Unix timestamps\). The field is 0 when the queue is empty.|1525342764|

## Metering logs {#section_kgc_wpr_m2b .section}

|Field|Description|Example|
|:----|:----------|:------|
|begin\_time|The start time of a statistics window \(in Unix timestamps\)|1525341600|
|index\_flow|The index traffic in the statistics window \(in bytes\)|12312|
|inflow|The write traffic in the statistics window \(in bytes\)|12345|
|logstore|The name of a Logstore|logstore-1|
|network\_out|The outbound traffic from the statistics window to the Internet \(in bytes\)|12345|
|outflow|The read traffic in the statistics window \(in bytes\)|23456|
|project|The name of a Project|project-1|
|read\_count|The number of data reads in the statistics window|100|
|shard|The average number of Shards used in the statistics window|10.0|
|storage\_index|The total amount of the index storage in a Logstore at the statistics time point \(in bytes\)|10000000|
|storage\_raw|The total number of logs in a Logstore at the statistics time point \(in bytes\)|20000000|
|write\_count|The number of data writes in the statistics window|199|

## Logtail status logs {#section_wdh_xtt_ngb .section}

|Field|Description|Example|
|:----|:----------|:------|
|cpu|The load of a CPU|0.001333156|
|hostname|The host name|abc2.et12|
|instance\_id|The instance ID, which is randomly assigned|05AFE618-0701-11E8-A95B-00163E025256\_10.11.12.13\_1517456122|
|ip|The IP address|1.0.1.0|
|load|The average system load|0.01 0.04 0.05 2/376 5277|
|memory|The amount of the memory occupied by the Logtail process \(in MB\)|12|
|detail\_metric|The value of a metered item \(in JSON format\). For more information, see [detail\_metric](#).|[detail\_metric](#)|
|os|The OS|Linux|
|os\_cpu|The overall CPU usage|0.004120005|
|os\_detail|Details relating to the OS|2.6.32-220.23.8.tcp1.34.el6.x86\_64|
|status|The client status, which can be:-   ok
-   busy
-   many\_log\_files
-   process\_block
-   send\_block
-   send\_error

 For more information, see [Logtail Run Status](../../../../../reseller.en-US/FAQ/Log collection/Query local collection status.md#table_tkh_sjs_zdb).|busy|
|user|The user name|root|
|user\_defined\_id|The user-defined ID|aliyun-log-id|
|uuid|The UUID of the server|64F28D10-D100-492C-8FDC-0C62907F1234|
|version|The Logtail version|0.14.2|
|project|The Project to which the Logtail Config belongs|my-project|

The following table lists the fields that are included in `detail_metric`.

|Field|Description|Example|
|:----|:----------|:------|
|config\_count|The number of Logtail Configs|1|
|config\_get\_last\_time|The time when the Config information was last obtained|1525686673|
|config\_update\_count|The number of Config updates after Logtail is started|1|
|config\_update\_item\_count|The total number of configuration item updates after Logtail is started|1|
|config\_update\_last\_time|The time when the Config is last updated after Logtail is started|1|
|event\_tps|The TPS|1|
|last\_read\_event\_time|The time when the event was last obtained|1525686663|
|last\_send\_time|The time when data was last sent|1525686663|
|open\_fd|The number of files that are currently open|1|
|poll\_modify\_size|The number of files with listener modification events|1|
|polling\_dir\_cache|The number of scanned folders|1|
|polling\_file\_cache|The number of scanned files|1|
|process\_byte\_ps|The number of logs processed per second \(in bytes\)|1000|
|process\_lines\_ps|The number of logs processed per second|1000|
|process\_queue\_full|The number of sending queues that have reached the maximum queue length|1|
|process\_queue\_total|The number of processing queues|10|
|process\_tps|The processing TPS|0|
|reader\_count|The number of files being processed|1|
|region|The region to which Logtail belongs|cn-hangzhou,cn-shanghai|
|register\_handler|The number of folders registered with listeners|1|
|send\_byte\_ps|The number of raw logs sent per second \(in bytes\)|11111|
|send\_line\_ps|The number of logs sent per second|1000|
|send\_net\_bytes\_ps|The amount of network data sent per second \(in bytes\)|1000|
|send\_queue\_full|The number of sending queues that have reached the maximum queue length|1|
|send\_queue\_total|The number of sending queues|12|
|send\_tps|The sending TPS|0.075|
|sender\_invalid|The number of abnormal sending queues|0|

