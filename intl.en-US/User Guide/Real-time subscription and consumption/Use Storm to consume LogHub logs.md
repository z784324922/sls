# Use Storm to consume LogHub logs {#concept_crv_c4q_zdb .concept}

LogHub of Log Service provides an efficient and reliable log channel for collecting log data in real time by using Logtail and SDKs. After collecting logs, you can consume the data written to  LogHub by using real-time systems such as Spark Stream and Storm.

Log Service provides LogHub Storm spout to read data from LogHub in real time, reducing the cost of LogHub consumption for Storm users.

## Basic architecture and process {#section_gw4_2hm_12b .section}

![](images/5809_en-US.png "Basic architecture and process")

-   In the preceding figure, the LogHub Storm spout is enclosed in the red dotted box. Each Storm topology has a group of spouts to read all the data from a Logstore. The spouts in different topologies are independent of each other.
-   Each topology is identified by a unique LogHub consumer group name. Spouts in the same topology use the [Consumer Library](reseller.en-US/User Guide/Real-time subscription and consumption/Consumption by consumer groups/Consumer group - Usage.md) to achieve load balancing and automatic failover.
-   Spouts read data from LogHub in real time, send data to the bolt nodes of the topology, and periodically save consumption endpoint as checkpoint to LogHub.

## Limits {#section_q4m_3hm_12b .section}

-   To prevent misuse, each Logstore supports up to five consumer groups. You can use the DeleteConsumerGroup interface of the Java SDK to delete unused consumer groups.
-   We recommend that the number of spouts is the same as that of shards. Otherwise, a single spout may not process a large amount of data.
-   If a shard contains a large amount of data exceeding the processing capability of a single spout, you can use the shard split interface to split the shard and reduce the data volume of each shard.
-   Dependency on the Storm ACK is required in LogHub spouts to confirm that spouts correctly send messages to bolts. Therefore, bolts must call ACK for confirmation.

## Usage example {#section_ukx_43c_ry .section}

-   **Spout \(used to build  topology\)**

    ```
         public static void main( String[] args )
        {     
            String mode = "Local"; // Use the local test mode.
               String conumser_group_name = ""; // Specify a unique consumer group name for each topology. The value cannot be empty. The value can be 3–63 characters long, contain lowercase letters, numbers, hyphens (-), and underscores (_), and must begin and end with a lowercase letter or number.
            String project = ""; // The Log Service project. 
            String logstore = ""; // The Log Service Logstore.
            String endpoint = "";   // Domain of the Log Service
            String access_id = "";  // User's access key
             String access_key = "";
            // Configurations required for building a LogHub Storm spout.
            Loghubspoutconfig Config = new loghubspoutconfig (conumser_group_name,
                    endpoint, project, logstore, access_id,
                    access_key, LogHubCursorPosition.END_CURSOR);
            Topologybuilder builder = new topologybuilder ();
            // 构建 loghub storm spout
            Loghubspout spin = new (config );
             // The number of spouts can be the same as that of Logstore shards in actual scenarios.
            builder.setSpout("spout", spout, 1);
            builder.setBolt("exclaim", new SampleBolt()).shuffleGrouping("spout");
            Config conf = new Config();
            conf.setDebug(false);
            conf.setMaxSpoutPending(1); 
            // The serialization method LogGroupDataSerializSerializer of LogGroupData must be configured explicitly when Kryo is used for data serialization and deserialization.
            Config.registerSerialization(conf, LogGroupData.class, LogGroupDataSerializSerializer.class);
            if (mode.equals("Local")) {
                logger.info("Local mode...") ;
                LocalCluster cluster = new LocalCluster();
                cluster.submitTopology("test-jstorm-spout", conf, builder.createTopology());
                try {
                    Thread.sleep(6000 * 1000); //waiting for several minutes
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

-   **The following bolt code example consumes data and only prints the contents of each log.**

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
                // Each LogGroup consists of one or more logs.
                LogGroup logGroup = groupData.GetLogGroup();
                for (Log log : logGroup.getLogsList()) {
                    StringBuilder sb = new StringBuilder();
                    // Each log has a time field and multiple key: value pairs,
                    int log_time = log.getTime();
                    sb.append("LogTime:").append(log_time);
                    for (Content content : log.getContentsList()) {
                        sb.append("\t").append(content.getKey()).append(":")
                                .append(content.getValue());
                    }
                    logger.info(sb.toString());
                }
            }
            // The dependency on the Storm ACK mechanism is mandatory in LogHub spouts to confirm that spouts send messages correctly
            // to bolts. Therefore, bolts must call ACK for such confirmation.
            mCollector.ack(tuple);
        }
        @Override
        public void declareOutputFields(OutputFieldsDeclarer declarer) {
            //do nothing
        }
    }
    ```


## Maven {#section_khp_w3c_ry .section}

Use the following code for versions earlier than Storm 1.0 \(for example, 0.9.6\):

```
<dependency>
  <groupId>com.aliyun.openservices</groupId>
  <artifactId>loghub-storm-spout</artifactId>
  <version>0.6.5</version>
</dependency>
```

Use the following code for Storm 1.0 and later versions:

```
<dependency>
  <groupId>com.aliyun.openservices</groupId>
  <artifactId>loghub-storm-1.0-spout</artifactId>
  <version>0.1.2</version>
</dependency>
```

