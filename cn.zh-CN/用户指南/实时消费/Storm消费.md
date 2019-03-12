# Storm消费 {#concept_crv_c4q_zdb .concept}

日志服务的LogHub提供了高效、可靠的日志通道功能，您可以通过Logtail、SDK等多种方式来实时收集日志数据。收集日志之后，可以通过Spark Stream、Storm 等各实时系统来消费写入到LogHub中的数据。

为了降低Storm用户消费LogHub的代价，日志服务提供了LogHub Storm Spout来实时读取LogHub的数据。

## 基本结构和流程 {#section_gw4_2hm_12b .section}

![](images/5809_zh-CN.png "基本结构和流程")

-   上图中红色虚线框中就是LogHub Storm Spout，每个Storm Topology会有一组Spout，同组内的Spout共同负责读取Logstore中全部数据。不同Topology中的Spout相互不干扰。
-   每个Topology需要选择唯一的LogHub Consume Group名字来相互标识，同一 Topology内的Spout通过 [Consumer Library](cn.zh-CN/用户指南/实时消费/消费组消费/通过消费组消费日志.md) 来完成负载均衡和自动failover。
-   Spout从LogHub中实时读取数据之后，发送至Topology中的Bolt节点，定期保存消费完成位置作为checkpoint到LogHub服务端。

## 使用限制 {#section_q4m_3hm_12b .section}

-   为了防止滥用，每个Logstore最多支持 10 个Consumer Group，对于不再使用的 Consumer Group，可以使用Java SDK中的DeleteConsumerGroup接口进行删除。
-   Spout的个数最好和Shard个数相同，否则可能会导致单个Spout处理数据量过多而处理不过来。
-   如果单个Shard 的数据量太大，超过一个Spout处理极限，则可以使用Shard split接口分裂Shard，来降低每个Shard的数据量。
-   在Loghub Spout中，强制依赖Storm的ACK机制，用于确认Spout将消息正确发送至Bolt，所以在Bolt中一定要调用ACK进行确认。

## 使用样例 {#section_ukx_43c_ry .section}

-   **Spout 使用示例（用于构建 Topology）**

    ```
         public static void main( String[] args )
        {     
            String mode = "Local";  // 使用本地测试模式
               String conumser_group_name = "";   // 每个Topology 需要设定唯一的 consumer group 名字，不能为空，支持 [a-z][0-9] 和 '_'，'-'，长度在 [3-63] 字符，只能以小写字母和数字开头结尾
            String project = "";    // 日志服务的Project 
            String logstore = "";   // 日志服务的Logstore
            String endpoint = "";   // 日志服务访问域名
            String access_id = "";  // 用户 ak 信息
            String access_key = "";
            // 构建一个 Loghub Storm Spout 需要使用的配置
            LogHubSpoutConfig config = new LogHubSpoutConfig(conumser_group_name,
                    endpoint, project, logstore, access_id,
                    access_key, LogHubCursorPosition.END_CURSOR);
            TopologyBuilder builder = new TopologyBuilder();
            // 构建 loghub storm spout
            LogHubSpout spout = new LogHubSpout(config);
            // 在实际场景中，Spout的个数可以和Logstore Shard 个数相同
            builder.setSpout("spout", spout, 1);
            builder.setBolt("exclaim", new SampleBolt()).shuffleGrouping("spout");
            Config conf = new Config();
            conf.setDebug(false);
            conf.setMaxSpoutPending(1); 
            // 如果使用Kryo进行数据的序列化和反序列化，则需要显示设置 LogGroupData 的序列化方法 LogGroupDataSerializSerializer
            Config.registerSerialization(conf, LogGroupData.class, LogGroupDataSerializSerializer.class);
            if (mode.equals("Local")) {
                logger.info("Local mode...");
                LocalCluster cluster  = new LocalCluster();
                cluster.submitTopology("test-jstorm-spout", conf, builder.createTopology());
                try {
                    Thread.sleep(6000 * 1000);    //waiting for several minutes
                } catch (InterruptedException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }  
                cluster.killTopology("test-jstorm-spout");
                cluster.shutdown();  
            } else if (mode.equals("Remote")) {
                logger.info("Remote mode...");
                conf.setNumWorkers(2);
                try {
                    StormSubmitter.submitTopology("stt-jstorm-spout-4", conf, builder.createTopology());
                } catch (AlreadyAliveException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                } catch (InvalidTopologyException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
            } else {
                logger.error("invalid mode: " + mode);
            }
        }
    }
    ```

-   **消费数据的 bolt 代码样例，只打印每条日志的内容**

    ```
    public class SampleBolt extends BaseRichBolt {
        private static final long serialVersionUID = 4752656887774402264L;
        private static final Logger logger = Logger.getLogger(BaseBasicBolt.class);
        private OutputCollector mCollector;
        @Override
        public void prepare(@SuppressWarnings("rawtypes") Map stormConf, TopologyContext context,
                OutputCollector collector) {
            mCollector = collector;
        }
        @Override
        public void execute(Tuple tuple) {
            String shardId = (String) tuple
                    .getValueByField(LogHubSpout.FIELD_SHARD_ID);
            @SuppressWarnings("unchecked")
            List<LogGroupData> logGroupDatas = (ArrayList<LogGroupData>) tuple.getValueByField(LogHubSpout.FIELD_LOGGROUPS);
            for (LogGroupData groupData : logGroupDatas) {
                // 每个 logGroup 由一条或多条日志组成
                LogGroup logGroup = groupData.GetLogGroup();
                for (Log log : logGroup.getLogsList()) {
                    StringBuilder sb = new StringBuilder();
                    // 每条日志，有一个时间字段，以及多个 Key:Value 对，
                    int log_time = log.getTime();
                    sb.append("LogTime:").append(log_time);
                    for (Content content : log.getContentsList()) {
                        sb.append("\t").append(content.getKey()).append(":")
                                .append(content.getValue());
                    }
                    logger.info(sb.toString());
                }
            }
            // 在 loghub spout 中，强制依赖 storm 的 ack 机制，用于确认 spout 将消息正确
            // 发送至 bolt，所以在 bolt 中一定要调用 ack
            mCollector.ack(tuple);
        }
        @Override
        public void declareOutputFields(OutputFieldsDeclarer declarer) {
            //do nothing
        }
    }
    ```


## Maven {#section_khp_w3c_ry .section}

storm 1.0 之前版本（如 0.9.6），请使用：

```
<dependency>
  <groupId>com.aliyun.openservices</groupId>
  <artifactId>loghub-storm-spout</artifactId>
  <version>0.6.5</version>
</dependency>
```

storm 1.0 版本及以后，请使用：

```
<dependency>
  <groupId>com.aliyun.openservices</groupId>
  <artifactId>loghub-storm-1.0-spout</artifactId>
  <version>0.1.2</version>
</dependency>
```

