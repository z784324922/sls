# Flume消费 {#concept_862261 .concept}

日志服务LogHub支持通过aliyun-log-flume插件与Flume进行对接，实现日志数据的写入和消费数据。

aliyun-log-flume是一个实现日志服务（Loghub）对接Flume的插件，可以通过Flume将日志服务与其它数据系统如HDFS，Kafka等打通。目前Flume官方支持的插件除了HDFS、Kafka之外，还有Hive、HBase、ElasticSearch等。 除此之外，对于常见的数据源在社区也都能找到对应的插件支持。 aliyun-log-flume为Loghub提供Sink和Source插件与Flume对接。

-   Sink：Flume读取其他数据源的数据然后写入Loghub。
-   Source：Flume消费Loghub的数据然后写入其他系统。

Github地址请单击：[Github](https://github.com/aliyun/aliyun-log-flume)。

## Loghub Sink {#section_ns9_5qu_wgh .section}

通过Sink的方式将其他数据源的数据通过Flume接入Loghub。目前支持两种解析格式：

-   SIMPLE：将整个Flume Event作为一个字段写入Loghub。
-   DELIMITED：将整个Flume Event作为分隔符分隔的数据，根据配置的列名解析成对应的字段写入Loghub。

支持的配置如下：

|配置项|是否必选|说明|
|---|----|--|
|type|必选|固定为com.aliyun.loghub.flume.sink.LoghubSink。|
|endpoint|必选|服务入口。|
|project|必选|Project名称。|
|logstore|必选|Logstore名称。|
|accessKeyId|必选|阿里云访问秘钥accessKeyId。|
|accessKey|必选|阿里云访问秘钥accessKey。|
|batchSize|可选|每批次写入Loghub的数据条数。默认1000条。|
|maxBufferSize|可选|缓存队列的大小。默认1000条。|
|serializer|可选|Event序列化格式，支持模式如下： -   **DELIMITED**：设置解析格式为分隔符模式。如果设置为该模式，则columns参数为必选。
-   **SIMPLE**：设置解析格式为单行模式。默认为该模式。
-   自定义**serializer**：设置解析格式为自定义的序列化器模式，设置为该模式时需要填写完整类名称。

 |
|columns|可选|serializer为**DELIMITED**时，必须指定该字段列表，用逗号分隔，顺序与实际数据中的字段顺序一致。|
|separatorChar|可选|serializer为**DELIMITED**时，用于指定数据的分隔符，必须为单个字符。默认为`,`。|
|quoteChar|可选|serializer为**DELIMITED**时，用于指定Quote字符。默认为`"`。|
|escapeChar|可选|serializer为**DELIMITED**时，用于指定转义字符。默认为`"`。|
|useRecordTime|可选|是否使用数据中的timestamp字段作为日志时间。如果为false则使用当前时间。默认为false。|

## Loghub Source {#section_gf0_owt_c1t .section}

通过Source的方式将Loghub的数据通过Flume投递到其他的数据源。目前支持两种输出格式：

-   DELIMITED：数据以分隔符日志的形式写入Flume。
-   JSON：数据以JSON日志的形式写入Flume。

支持的配置如下：

|配置项|是否必选|说明|
|---|----|--|
|type|必选|固定为com.aliyun.loghub.flume.source.LoghubSource。|
|endpoint|必选|服务入口。|
|project|必选|Project名称。|
|logstore|必选|Logstore名称。|
|accessKeyId|必选|阿里云访问秘钥accessKeyId。|
|accessKey|必选|阿里云访问秘钥accessKey。|
|heartbeatIntervalMs|可选|客户端和Loghub的心跳间隔，默认30000，单位毫秒。|
|fetchIntervalMs|可选|Loghub数据拉取间隔，默认100，单位毫秒。|
|fetchInOrder|可选|是否按顺序消费。默认false。|
|batchSize|可选|每批次读取的数据条数，默认100。|
|consumerGroup|可选|读取的消费组名称（随机产生）。|
|initialPosition|可选|读取数据的起点位置，支持begin，end，timestamp。默认为begin。 **说明：** 如果服务端已经存在checkpoint，会优先使用服务端的checkpoint。

 |
|timestamp|可选|当initialPosition为**timestamp**时，必须指定时间戳，Unix时间戳格式。|
|deserializer|必选|Event反序列化格式，支持的模式如下： -   **DELIMITED**：设置解析格式为分隔符模式。默认为该模式。如果设置为该模式，则columns参数为必选。
-   **JSON**：设置解析格式为JSON模式。
-   自定义**serializer**：设置解析格式为自定义的序列化器模式，设置为该模式时需要填写完整类名称。

 |
|columns|可选|deserializer为**DELIMITED**时，必须指定字段列表，用逗号分隔，顺序与实际数据中的字段顺序一致。|
|separatorChar|可选|deserializer为**DELIMITED**时，用于指定数据的分隔符，必须为单个字符。默认为`,`。|
|quoteChar|可选|deserializer为**DELIMITED**时，用于指定Quote字符。默认为`"`。|
|escapeChar|可选|deserializer为**DELIMITED**时，用于指定转义字符。默认为`"`。|
|appendTimestamp|可选|deserializer为**DELIMITED**时，是否将时间戳作为一个字段自动添加到每行末尾。默认为false。|
|sourceAsField|可选|deserializer为**JSON**时，是否将日志Source作为一个字段，字段名称为\_\_source\_\_。默认为false。|
|tagAsField|可选|deserializer为**JSON**时，是否将日志Tag作为字段，字段名称为\_\_tag\_\_：\{tag名称\}。默认为false。|
|timeAsField|可选|deserializer为**JSON**时，是否将日志时间作为一个字段，字段名称为\_\_time\_\_。默认为false。|
|useRecordTime|可选|是否使用日志的时间，如果为false则使用当前时间。默认为false。|

