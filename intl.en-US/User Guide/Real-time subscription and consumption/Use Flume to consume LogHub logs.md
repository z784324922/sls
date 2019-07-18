# Use Flume to consume LogHub logs {#concept_862261 .concept}

You can use the aliyun-log-flume plug-in to connect Flume to LogHub of Log Service to write and consume log data.

After connecting Flume to LogHub, you can connect Log Service to other data systems, such as Hadoop Distributed File System \(HDFS\) and Kafka, through Flume. Currently, Flume supports plug-ins for data systems such as HDFS, Kafka, Hive, HBase, and Elasticsearch. You can also find plug-ins for connecting Flume to common data sources in the Flume community. The aliyun-log-flume plug-in provides the LogHub sink and source plug-ins for connecting LogHub and Flume as follows:

-   Sink: uses Flume to read data from other data sources and then write data to LogHub.
-   Source: uses Flume to consume LogHub data and then write data to other systems.

## LogHub sink {#section_ns9_5qu_wgh .section}

You can use the LogHub sink to transmit data from other data sources to LogHub through Flume. Currently, the following parsing formats are supported:

-   SIMPLE: writes a Flume event to LogHub as a field.
-   DELIMITED: separates Flume events with a delimiter, parses an event into fields based on the configured column names, and then writes them to LogHub.

The following table lists the parameters that can be configured.

|Parameter|Description|Required|
|---------|-----------|--------|
|type|Set this parameter to com.aliyun.loghub.flume.sink.LoghubSink.|Yes|
|endpoint|The service endpoint of Log Service.|Yes|
|project|The name of the project.|Yes|
|logstore|The name of the Logstore.|Yes|
|accessKeyId|The AccessKey ID.|Yes|
|accessKey|The AccessKey Secret.|Yes|
|batchSize|The number of data entries to be written to LogHub each time. Default value: 1000.|No|
|maxBufferSize|The size of the cache queue. Default value: 1000.|No|
|serializer|The event serialization format. Valid values: **DELIMITED**, **SIMPLE**, and custom serializer. If you specify a custom serializer, enter the complete class name.******** Default value: **SIMPLE**.|No|
|columns|The configured column names. You must specify this parameter if you set the serializer parameter to **DELIMITED**. Separate multiple columns with a comma \(,\) and ensure that the columns are sorted in the same order as those in actual data.|No|
|separatorChar|The delimiter, which is a single character. You can specify this parameter if you set the serializer parameter to **DELIMITED**. Default value: comma \(`,`\).|No|
|quoteChar|The quote character. You can specify this parameter if you set the serializer parameter to **DELIMITED**. Default value: double quotation mark \(`"`\).|No|
|escapeChar|The escape character. You can specify this parameter if you set the serializer parameter to **DELIMITED**. Default value: double quotation mark \(`"`\).|No|
|useRecordTime|Specifies whether to use the value of the timestamp field as the log time. A value of false indicates that the current time is used. Default value: false.|No|

## LogHub source {#section_gf0_owt_c1t .section}

You can use the LogHub source to transmit data from LogHub to other data systems through Flume. Currently, the following output formats are supported:

-   DELIMITED: writes data to Flume as delimiter logs.
-   JSON: writes data to Flume as JSON logs.

The following table lists the parameters that can be configured.

|Parameter|Description|Required|
|---------|-----------|--------|
|type|Set this parameter to com.aliyun.loghub.flume.source.LoghubSource.|Yes|
|endpoint|The service endpoint of Log Service.|Yes|
|project|The name of the project.|Yes|
|logstore|The name of the Logstore.|Yes|
|accessKeyId|The AccessKey ID.|Yes|
|accessKey|The AccessKey Secret.|Yes|
|heartbeatIntervalMs|The heartbeat interval between the Flume client and LogHub, in milliseconds. Default value: 30000.|No|
|fetchIntervalMs|The interval for pulling data from LogHub, in milliseconds. Default value: 100.|No|
|fetchInOrder|Specifies whether to consume log data in order. Default value: false.|No|
|batchSize|The number of data entries to be read each time. Default value: 100.|No|
|consumerGroup|The name of the consumer group to be read \(which is randomly generated\).|No|
|initialPosition|The start point for reading data. Valid values: begin, end, and timestamp. Default value: begin. **Note:** If a checkpoint exists on the server, the checkpoint is used.

 |No|
|timestamp|The Unix timestamp. You must specify this parameter if you set the initialPosition parameter to **timestamp**.|No|
|deserializer|The event deserialization format. Valid values: **DELIMITED**, **JSON**, and custom deserializer. If you specify a custom deserializer, enter the complete class name.******** Default value: **DELIMITED**.|Yes|
|columns|The configured column names. You must specify this parameter if you set the deserializer parameter to **DELIMITED**. Separate multiple columns with a comma \(,\) and ensure that the columns are sorted in the same order as those in actual data.|No|
|separatorChar|The delimiter, which is a single character. You can specify this parameter if you set the deserializer parameter to **DELIMITED**. Default value: comma \(`,`\).|No|
|quoteChar|The quote character. You can specify this parameter if you set the deserializer parameter to **DELIMITED**. Default value: double quotation mark \(`"`\).|No|
|escapeChar|The escape character. You can specify this parameter if you set the deserializer parameter to **DELIMITED**. Default value: double quotation mark \(`"`\).|No|
|appendTimestamp|Specifies whether to automatically add the timestamp as a field to the end of each row. You can specify this parameter if you set the deserializer parameter to **DELIMITED**. Default value: false.|No|
|sourceAsField|Specifies whether to add the log source as a field with the field name \_\_source\_\_. You can specify this parameter if you set the deserializer parameter to **JSON**. Default value: false.|No|
|tagAsField|Specifies whether to add the log tags as a field with the field name \_\_tag\_\_: \{tag names\}. You can specify this parameter if you set the deserializer parameter to **JSON**. Default value: false.|No|
|timeAsField|Specifies whether to add the log time as a field with the field name \_\_time\_\_. You can specify this parameter if you set the deserializer parameter to **JSON**. Default value: false.|No|
|useRecordTime|Specifies whether to use the log time. A value of false indicates that the current time is used. Default value: false.|No|

