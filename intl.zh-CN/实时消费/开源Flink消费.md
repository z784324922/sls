# 开源Flink消费 {#task_1664085 .task}

Flink Log Connector是阿里云日志服务提供的，用于对接Flink的工具。本文档主要介绍如何对接Flink工具对日志数据进行消费。

-   已启用Access Key，并创建了Project和Logstore。详细步骤请参见[准备流程](../../../../cn.zh-CN/准备工作/准备流程.md#)。
-   若您选择使用子账号操作日志服务，请确认已正确设置了Logstore的RAM授权策略。详细内容请参见[授权RAM 用户](../../../../cn.zh-CN/访问控制RAM/授权RAM 用户.md#)。

Flink Log Connector包括两部分，消费者（Consumer）和生产者（Producer）。

-   消费者用于从日志服务中读取数据，支持exactly once语义，支持Shard负载均衡。
-   生产者用于将数据写入日志服务，使用Connector时，需要在项目中添加maven依赖：

    ``` {#codeblock_kz5_54t_0nn}
    <dependency>
       <groupId>com.aliyun.openservices</groupId>
       <artifactId>flink-log-connector</artifactId>
       <version>0.1.13</version>
    </dependency>
    <dependency>
       <groupId>com.google.protobuf</groupId>
       <artifactId>protobuf-java</artifactId>
       <version>2.5.0</version>
    </dependency>
    ```


Github地址请单击：[Github](https://github.com/aliyun/aliyun-log-flink-connector)。

## Log Consumer {#section_ar0_mek_a43 .section}

在Connector中， 类Flink Log Consumer提供了订阅日志服务中某一个Logstore的能力，实现了exactly once语义，在使用时，用户无需关心Logstore中Shard数量的变化，Consumer会自动感知。

Flink中每一个子任务负责消费Logstore中部分Shard，如果Logstore中Shard发生split或者merge，子任务消费的Shard也会随之改变。

Flink Log Consumer 会用到的阿里云日志服务接口如下：

-   GetCursorOrData

    用于从Shard中拉数据， 注意频繁的调用该接口可能会导致数据超过日志服务的Shard quota， 可以通过ConfigConstants.LOG\_FETCH\_DATA\_INTERVAL\_MILLIS和ConfigConstants.LOG\_MAX\_NUMBER\_PER\_FETCH控制接口调用的时间间隔和每次调用拉取的日志数量，Shard的quota参考文章[分区](../../../../cn.zh-CN/产品简介/基本概念/分区.md#)。

    ``` {#codeblock_9uc_h7n_tq0}
    configProps.put(ConfigConstants.LOG_FETCH_DATA_INTERVAL_MILLIS， "100");
    configProps.put(ConfigConstants.LOG_MAX_NUMBER_PER_FETCH， "100");
    ```

-   ListShards

    用于获取Logstore中所有的Shard列表，获取Shard状态等。如果您的Shard经常发生分裂合并，可以通过调整接口的调用周期来及时发现Shard的变化。

    ``` {#codeblock_yw4_fb6_hnl}
    // 设置每30s调用一次ListShards
    configProps.put(ConfigConstants.LOG_SHARDS_DISCOVERY_INTERVAL_MILLIS， "30000");
    ```

-   CreateConsumerGroup

    该接口调用只有当设置消费进度监控时才会发生，功能是创建ConsumerGroup，用于同步checkpoint。

-   ConsumerGroupUpdateCheckPoint

    该接口用户将Flink的snapshot同步到日志服务的ConsumerGroup中。


当RAM子账号使用Flink Log Consumer时，需要对以上几个API进行授权：

|接口|资源|
|:-|:-|
|GetCursorOrData|`acs:log:${regionName}:${projectOwnerAliUid}:project/${projectName}/logstore/${logstoreName}`|
|ListShards|`acs:log:${regionName}:${projectOwnerAliUid}:project/${projectName}/logstore/${logstoreName}`|
|CreateConsumerGroup|`acs:log:${regionName}:${projectOwnerAliUid}:project/${projectName}/logstore/${logstoreName}/consumergroup/*`|
|ConsumerGroupUpdateCheckPoint|`acs:log:${regionName}:${projectOwnerAliUid}:project/${projectName}/logstore/${logstoreName}/consumergroup/${consumerGroupName}`|

关于子账号及其授权操作请参见[简介](../../../../cn.zh-CN/访问控制RAM/简介.md#)。

1.  配置启动参数。 下面是一个简单的消费示例，我们使用java.util.Properties作为配置工具，所有Consumer的配置都可以在ConfigConstants中找到。

    ``` {#codeblock_k50_lwh_v4v}
    Properties configProps = new Properties();
    // 设置访问日志服务的域名
    configProps.put(ConfigConstants.LOG_ENDPOINT， "cn-hangzhou.log.aliyuncs.com");
    // 设置访问ak
    configProps.put(ConfigConstants.LOG_ACCESSSKEYID， "");
    configProps.put(ConfigConstants.LOG_ACCESSKEY， "");
    // 设置日志服务的project
    configProps.put(ConfigConstants.LOG_PROJECT， "ali-cn-hangzhou-sls-admin");
    // 设置日志服务的Logstore
    configProps.put(ConfigConstants.LOG_LOGSTORE， "sls_consumergroup_log");
    // 设置消费日志服务起始位置
    configProps.put(ConfigConstants.LOG_CONSUMER_BEGIN_POSITION， Consts.LOG_END_CURSOR);
    // 设置日志服务的消息反序列化方法
    RawLogGroupListDeserializer deserializer = new RawLogGroupListDeserializer();
    final StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
    DataStream<RawLogGroupList> logTestStream = env.addSource(
            new FlinkLogConsumer<RawLogGroupList>(deserializer， configProps));
    ```

    **说明：** Flink Stream的子任务数量和日志服务Logstore中的Shard数量是独立的，如果Shard数量多于子任务数量，每个子任务不重复的消费多个Shard，如果少于子任务数量，那么部分子任务就会空闲，等到新的Shard产生。

2.  设置消费起始位置。 Flink Log Consumer支持设置Shard的消费起始位置，通过设置属性ConfigConstants.LOG\_CONSUMER\_BEGIN\_POSITION，就可以定制消费从Shard的头尾或者某个特定时间开始消费，另外，Connector也支持从某个具体的ConsumerGroup中恢复消费。具体取值如下：

    -   Consts.LOG\_BEGIN\_CURSOR： 表示从Shard的头开始消费，也就是从Shard中最旧的数据开始消费。
    -   Consts.LOG\_END\_CURSOR： 表示从Shard的尾开始，也就是从Shard中最新的数据开始消费。
    -   Consts.LOG\_FROM\_CHECKPOINT：表示从某个特定的ConsumerGroup中保存的Checkpoint开始消费，通过ConfigConstants.LOG\_CONSUMERGROUP指定具体的ConsumerGroup。
    -   UnixTimestamp： 一个整型数值的字符串，用1970-01-01到现在的秒数表示， 含义是消费Shard中这个时间点之后的数据。
    四种取值示例如下：

    ``` {#codeblock_soi_7bn_zc4}
    configProps.put(ConfigConstants.LOG_CONSUMER_BEGIN_POSITION, Consts.LOG_BEGIN_CURSOR);
    configProps.put(ConfigConstants.LOG_CONSUMER_BEGIN_POSITION, Consts.LOG_END_CURSOR);
    configProps.put(ConfigConstants.LOG_CONSUMER_BEGIN_POSITION, "1512439000");
    configProps.put(ConfigConstants.LOG_CONSUMER_BEGIN_POSITION, Consts.LOG_FROM_CHECKPOINT);
    ```

    **说明：** 如果在启动Flink任务时，设置了从Flink自身的StateBackend中恢复，那么Connector会忽略上面的配置，使用StateBackend中保存的Checkpoint。

3.  设置消费进度监控。 Flink Log Consumer支持设置消费进度监控，所谓消费进度就是获取每一个Shard实时的消费位置，这个位置使用时间戳表示，详细概念可以参考文档[消费组状态](../../../../cn.zh-CN/实时消费/消费组消费/消费组状态.md#)，[消费组监控与报警](../../../../cn.zh-CN/实时消费/消费组消费/消费组监控与报警.md#)。

    ``` {#codeblock_rdi_4rn_25y}
    configProps.put(ConfigConstants.LOG_CONSUMERGROUP, "your consumer group name");
    ```

    **说明：** 该设置为可选配置项，如果设置，Consumer会首先创建ConsumerGroup，如果ConsumerGroup已经存在，则不执行任何操作，Consumer中的snapshot会自动同步到日志服务的ConsumerGroup中，用户可以在日志服务的控制台查看Consumer的消费进度。

4.  设置容灾和exactly once语义支持。 当打开Flink的Checkpointing功能时，Flink Log Consumer会周期性的将每个Shard的消费进度保存起来，当作业失败时，Flink会恢复Log Consumer，并从保存的最新的Checkpoint开始消费。

    写Checkpoint的周期定义了当发生失败时，最多多少的数据会被回溯，即重新消费，使用代码如下：

    ``` {#codeblock_pyz_swc_535}
    final StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
    // 开启flink exactly once语义
    env.getCheckpointConfig().setCheckpointingMode(CheckpointingMode.EXACTLY_ONCE);
    // 每5s保存一次checkpoint
    env.enableCheckpointing(5000);
    ```

    更多Flink Checkpoint的细节请参考Flink官方文档[Checkpoints](https://ci.apache.org/projects/flink/flink-docs-release-1.3/setup/checkpoints.html)。


## Log Producer {#section_ovw_f4f_q9h .section}

Flink Log Producer 用于将数据写到阿里云日志服务中。

**说明：** Producer只支持Flink at-least-once语义，在发生作业失败的情况下，写入日志服务中的数据有可能会重复，但是不会丢失。

Producer会用到的日志服务API接口如下：

-   PostLogStoreLogs
-   ListShards

当RAM子账号使用Producer时，需要对以上两个API进行授权：

|接口|资源|
|:-|:-|
|PostLogStoreLogs|`acs:log:${regionName}:${projectOwnerAliUid}:project/${projectName}/logstore/${logstoreName}`|
|ListShards|`acs:log:${regionName}:${projectOwnerAliUid}:project/${projectName}/logstore/${logstoreName}`|

关于子账号及其授权操作请参见[简介](../../../../cn.zh-CN/访问控制RAM/简介.md#)。

1.  初始化Producer。 
    1.  初始化配置参数Properties。 Producer初始化步骤与Consumer类似。 Producer包含以下参数，一般情况下使用默认值即可，如有需要，可以自定义配置。

        ``` {#codeblock_2sc_lm6_0n4}
        // 用于发送数据的io线程的数量，默认是8
        ConfigConstants.LOG_SENDER_IO_THREAD_COUNT
        // 该值定义日志数据被缓存发送的时间，默认是3000
        ConfigConstants.LOG_PACKAGE_TIMEOUT_MILLIS
        // 缓存发送的包中日志的数量，默认是4096
        ConfigConstants.LOG_LOGS_COUNT_PER_PACKAGE
        // 缓存发送的包的大小，默认是3Mb
        ConfigConstants.LOG_LOGS_BYTES_PER_PACKAGE
        // 作业可以使用的内存总的大小，默认是100Mb
        ConfigConstants.LOG_MEM_POOL_BYTES
        ```

        **说明：** 上述参数不是必选参数，用户可以不设置，直接使用默认值。

    2.  重载LogSerializationSchema，定义将数据序列化成RawLogGroup的方法。 RawLogGroup是log的集合，每个字段的含义可以参考文档[数据模型](../../../../cn.zh-CN/API 参考/公共资源说明/数据模型.md)。

        如果用户需要使用日志服务的Shard HashKey功能，指定数据写到某一个shard中，可以使用LogPartitioner产生数据的HashKey。

        示例：

        ``` {#codeblock_zm6_o7b_zef}
        FlinkLogProducer<String> logProducer = new FlinkLogProducer<String>(new SimpleLogSerializer()， configProps);
        logProducer.setCustomPartitioner(new LogPartitioner<String>() {
              // 生成32位hash值
              public String getHashKey(String element) {
                  try {
                      MessageDigest md = MessageDigest.getInstance("MD5");
                      md.update(element.getBytes());
                      String hash = new BigInteger(1， md.digest()).toString(16);
                      while(hash.length() < 32) hash = "0" + hash;
                      return hash;
                  } catch (NoSuchAlgorithmException e) {
                  }
                  return  "0000000000000000000000000000000000000000000000000000000000000000";
              }
          });
        ```

        **说明：** LogPartitioner为可选项，如您没有配置，数据会随机写入某一个Shard。

2.  执行以下示例语句，将模拟产生的字符串写入日志服务。 

    ``` {#codeblock_p46_o2t_fbj}
    // 将数据序列化成日志服务的数据格式
    class SimpleLogSerializer implements LogSerializationSchema<String> {
        public RawLogGroup serialize(String element) {
            RawLogGroup rlg = new RawLogGroup();
            RawLog rl = new RawLog();
            rl.setTime((int)(System.currentTimeMillis() / 1000));
            rl.addContent("message"， element);
            rlg.addLog(rl);
            return rlg;
        }
    }
    public class ProducerSample {
        public static String sEndpoint = "cn-hangzhou.log.aliyuncs.com";
        public static String sAccessKeyId = "";
        public static String sAccessKey = "";
        public static String sProject = "ali-cn-hangzhou-sls-admin";
        public static String sLogstore = "test-flink-producer";
        private static final Logger LOG = LoggerFactory.getLogger(ConsumerSample.class);
        public static void main(String[] args) throws Exception {
            final ParameterTool params = ParameterTool.fromArgs(args);
            final StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
            env.getConfig().setGlobalJobParameters(params);
            env.setParallelism(3);
            DataStream<String> simpleStringStream = env.addSource(new EventsGenerator());
            Properties configProps = new Properties();
            // 设置访问日志服务的域名
            configProps.put(ConfigConstants.LOG_ENDPOINT， sEndpoint);
            // 设置访问日志服务的ak
            configProps.put(ConfigConstants.LOG_ACCESSSKEYID， sAccessKeyId);
            configProps.put(ConfigConstants.LOG_ACCESSKEY， sAccessKey);
            // 设置日志写入的日志服务project
            configProps.put(ConfigConstants.LOG_PROJECT， sProject);
            // 设置日志写入的日志服务Logstore
            configProps.put(ConfigConstants.LOG_LOGSTORE， sLogstore);
            FlinkLogProducer<String> logProducer = new FlinkLogProducer<String>(new SimpleLogSerializer()， configProps);
            simpleStringStream.addSink(logProducer);
            env.execute("flink log producer");
        }
        // 模拟产生日志
        public static class EventsGenerator implements SourceFunction<String> {
            private boolean running = true;
            @Override
            public void run(SourceContext<String> ctx) throws Exception {
                long seq = 0;
                while (running) {
                    Thread.sleep(10);
                    ctx.collect((seq++) + "-" + RandomStringUtils.randomAlphabetic(12));
                }
            }
            @Override
            public void cancel() {
                running = false;
            }
        }
    }
    ```


