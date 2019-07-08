# 实时计算（Flink）消费 {#concept_g4n_24q_zdb .concept}

阿里云实时计算（Realtime Compute/Flink） 可以通过创建日志服务源表的方式，直接消费日志服务中的数据。

日志服务本身是流数据存储，流计算能将其作为流式数据输入。对于日志服务而言，每条日志包含一组key-value形式的字段，假设有如下的日志内容：

``` {#codeblock_cyo_i18_xuy}
__source__:  11.85.123.199
__tag__:__receive_time__:  1562125591
__topic__:  test-topic
a:  1234
b:  0
c:  hello
```

StreamCompute可以定义如下的DDL：

``` {#codeblock_ayq_rm4_93n}
create table sls_stream(
  a int,
  b int,
  c varchar
) with (
  type ='sls',
  endPoint ='<your endpoint>',
  accessId ='<your access key id>',
  accessKey ='<your access key>',
  startTime = '2017-07-05 00:00:00',
  project ='ali-cloud-streamtest',
  logStore ='stream-test',
  consumerGroup ='consumerGroupTest1'
);
```

除日志字段外，支持提取如下三个属性字段和tag中的自定义字段，如`__receive_time__`：

|字段名|说明|
|:--|:-|
|`__source__`|日志来源。|
|`__topic__`|日志主题。|
|`__timestamp__`|日志时间。|

属性字段的提取需要添加HEADER声明，如：

``` {#codeblock_piq_iyv_r6v}
create table sls_stream(
  __timestamp__ bigint HEADER,
  __receive_time__ bigint HEADER
  b int,
  c varchar
) with (
  type ='sls',
  endPoint ='<your endpoint>',
  accessId ='<your access key id>',
  accessKey ='<your access key>',
  startTime = '2017-07-05 00:00:00',
  project ='ali-cloud-streamtest',
  logStore ='stream-test',
  consumerGroup ='consumerGroupTest1'
);
```

**WITH参数说明** 

|参数|是否必选|说明|
|--|----|--|
|endPoint|是|日志服务endpoint。|
|accessId|是|访问日志服务的accessKey ID。|
|accessKey|是|访问日志服务的密钥accessKey。|
|project|是|日志服务Project名称。|
|logStore|是|日志服务Logstore名称。|
|consumerGroup|否|消费组名称。|
|startTime|否|消费日志开始的时间点。|
|heartBeatIntervalMills|否|消费客户端的心跳间隔时间。默认为10秒。|
|maxRetryTimes|否|读取最大尝试次数，默认5次。|
|batchGetSize|否|单次读取logGroup条数，默认为10条。如果flink的版本是1.4.2+，则默认为100条， 最大1000条。|
|columnErrorDebug|否|是否打开调试开关，如果打开，会把解析异常的日志打印出来。默认为false。|

**类型映射**

日志服务中的字段都是字符串类型，和流计算字段类型对应关系如下，强烈建议用户使用该对应关系进行DDL声明：

|日志服务字段类型|流计算字段类型|
|--------|-------|
|STRING|VARCHAR|

如果使用其他类型也会尝试自动转换，比如“1000”或者“2018-01-12 12:00:00”也可以定义为bigint和timestamp类型。

**说明：** 

-   Blink 2.2.0 以下的版本不支持Shard的扩容和缩容，如果分裂或者缩容了某个正在流计算读取的logStore，会导致任务持续出错，不可恢复，该情况下需要重新启动任务来使任务恢复正常。
-   所有blink版本均不支持对正在消费的logStore进行删除 重建。
-   1.6.0（含）版本指定consumerGroup在shards数目很多情况下可能会影响读取性能。
-   日志服务暂不支持MAP类型的数据。
-   对于不存在的字段会置为null。
-   字段顺序支持无序（建议字段顺序和表中定义一致）。
-   batchGetSize指明的是logGroup获取的数量，如果单条日志的大小和batchGetSize都很大，很有可能会导致频繁的GC。

**FAQ** 

-   如果一个Shard没有新数据写入，会导致job的整体延迟增加，或有些窗口聚合的job无输出，这个要多注意。遇到这种情况，只需要把并发数调整为RW的partition数量即可。
-   并发数建议和Shard个数一致，否则当两个Shard读取速度差异较大时，理论上在追数据场景存在数据被过滤掉的风险。
-   针对`__tag__:__hostname__`、`__tag__:__path__`等字段，去掉前面的tag，按照获取属性字段的方式获取即可。

    **说明：** 调试时无法抽取到该类型数据，建议用户用本地DEBUG方法，上线运行print到日志中查看。


