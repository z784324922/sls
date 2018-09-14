# Consumer group - Usage {#concept_dv4_xnq_zdb .concept}

The consumer library is an advanced mode of log consumption in Log Service, and provides the consumer group concept to abstract and manage the consumption end. Compared with using SDKs directly to read data, you can only focus on the business logic by using the consumer library, without caring about the implementation details of Log Service, or the load balancing or failover between consumers.

[Spark Streaming](intl.en-US/User Guide/Real-time subscription and consumption/Use Spark Streaming to consume LogHub logs.md), [Storm](intl.en-US/User Guide/Real-time subscription and consumption/Use Storm to consume LogHub logs.md), and Flink connector use consumer library as the base implementation.

## Basic concepts {#section_sn3_52n_1bb .section}

You must understand two concepts before using the consumer library: consumer group and consumer.

-   Consumer group

    A consumer group is composed of multiple consumers. Consumers in the same consumer group consume the data in the same Logstore and the data consumed by each consumer is different.

-   Consumer

    Consumers, as a unit that composes the consumer group, must consume data. The names of consumers in the same consumer group must be unique.


In Log Service, a Logstore can have multiple shards. The consumer library is used to allocate a shard to the consumers in a consumer group. The allocation rules are as follows:

-   Each shard can only be allocated to one consumer.
-   One consumer can have multiple shards at the same time.

After a new consumer is added to a consumer group, the affiliations of the shards for this consumer group is adjusted to achieve the load balancing of consumption. However, the preceding allocation rules are not changed. The allocation process is transparent to users.

The consumer library can also save the checkpoint, which allows consumers to consume data starting from the breakpoint after the program fault is resolved and makes sure that the data is consumed only once.

## Usage {#section_hmf_xjn_1bb .section}

## Add maven dependency {#section_p3k_dgc_ry .section}

```
<dependency>
  <groupId>com.google.protobuf</groupId>
  <artifactId>protobuf-java</artifactId>
  <version>2.5.0</version>
</dependency>
<dependency>
<groupId>com.aliyun.openservices</groupId>
<artifactId>loghub-client-lib</artifactId>
<version>0.6.15</version>
</dependency>
```

**main .java file**

```
public class Main {
    // Enter the domain name of Log Service according to your actual situation.
  private static String sEndpoint = "cn-hangzhou.log.aliyuncs.com";
    // Enter the project name of Log Service according to your actual situation.
  private static String sProject = "ali-cn-hangzhou-sls-admin";
    // Enter the Logstore name of Log Service according to your actual situation.
  private static String sLogstore = "sls_operation_log";
    // Enter the consumer group name according to your actual situation.
  private static String sConsumerGroup = "consumerGroupX";
    // Enter the AccessKey of data consumption according to your actual situation.
  private static String sAccessKeyId = "";
  private static String sAccessKey = "";
  public static void main(String []args) throws LogHubClientWorkerException, InterruptedException
  {
              // The second parameter is the consumer name. The consumer names in the same consumer group must be unique. However, the consumer group names can be duplicate. Different consumer names start multiple processes on multiple machines to consume a Logstore in a load balancing way. In this case, the consumer group names can be classified by machine IP address.  The ninth parameter maxFetchLogGroupSize is the number of Logstores each time obtained from Log Service. Use the default value. If you must adjust the value, make sure the value range is (0,1000].
      LogHubConfig config = new LogHubConfig(sConsumerGroup, "consumer_1", sEndpoint, sProject, sLogstore, sAccessKeyId, sAccessKey, LogHubConfig.ConsumePosition.BEGIN_CURSOR);
      ClientWorker worker = new ClientWorker(new SampleLogHubProcessorFactory(), config);
        Thread thread = new Thread(worker);
        //The ClientWorker automatically runs after the thread is running and extends the Runnable API.
       thread.start();
       Thread.sleep(60 * 60 * 1000);
              //Call the Shutdown function of worker to exit the consumption instance. The associated thread is automatically stopped.
       worker.shutdown();
       //Multiple asynchronous tasks are generated when the ClientWorker is running. We recommend that you wait 30 seconds until the running tasks exit after the shutdown. 
       Thread.sleep(30 * 1000);
  }
}
```

SampleLogHubProcessor.java files

```
public class SampleLogHubProcessor implements ILogHubProcessor 
{
  private int mShardId;
  // Records the last persistent checkpoint time.
  private long mLastCheckTime = 0; 
  public void initialize(int shardId) 
  {
      mShardId = shardId;
  }
  // The main logic of data consumption. Catch all the exceptions but the caught exceptions cannot be thrown. 
  public String process(List<LogGroupData> logGroups,
          ILogHubCheckPointTracker checkPointTracker) 
  {
          // Write checkpoint to Log Service every 30 seconds. If worker crashes within 30 seconds, the newly started worker consumes data starting from the last checkpoint. Slight duplicate data may exist.
      for(LogGroupData logGroup: logGroups){
          FastLogGroup flg = logGroup.GetFastLogGroup();
          System.out.println(String.format("\tcategory\t:\t%s\n\tsource\t:\t%s\n\ttopic\t:\t%s\n\tmachineUUID\t:\t%s",
                  flg.getCategory(), flg.getSource(), flg.getTopic(), flg.getMachineUUID()));
          System.out.println("Tags");
          for (int tagIdx = 0; tagIdx < flg.getLogTagsCount(); ++tagIdx) {
              FastLogTag logtag = flg.getLogTags(tagIdx);
              System.out.println(String.format("\t%s\t:\t%s", logtag.getKey(), logtag.getValue()));
          }
          for (int lIdx = 0; lIdx < flg.getLogsCount(); ++lIdx) {
              FastLog log = flg.getLogs(lIdx);
              System.out.println("--------\nLog: " + lIdx + ", time: " + log.getTime() + ", GetContentCount: " + log.getContentsCount());
              for (int cIdx = 0; cIdx < log.getContentsCount(); ++cIdx) {
                  FastLogContent content = log.getContents(cIdx);
                  System.out.println(content.getKey() + "\t:\t" + content.getValue());
              }
          }
      }
      long curTime = System.currentTimeMillis();
      // Write checkpoint to Log Service every 30 seconds. If worker crashes within 30 seconds, 
      // the newly started worker consumes data starting from the last checkpoint. Slight duplicate data may exist.
      if (curTime - mLastCheckTime > 30 * 1000) 
      {
          try  
          {
                          //The parameter true indicates to update the checkpoint to Log Service immediately. The parameter false indicates to cache the checkpoint to your local machine and refresh the cached checkpoint to Log Service every 60 seconds by default.
              checkPointTracker.saveCheckPoint(true);
          } 
          catch (LogHubCheckPointException e) 
          {
              e.printStackTrace();
          }
          mLastCheckTime = curTime;
      } 
      return null;  
  }
  // The worker calls this function upon exit. You can perform cleanup here.
  public void shutdown(ILogHubCheckPointTracker checkPointTracker) 
  {
      //Saves the consumption breakpoint to the Log Service.
      try {
          checkPointTracker.saveCheckPoint(true);
      } catch (LogHubCheckPointException e) {
          e.printStackTrace();
      }
  }
}
class SampleLogHubProcessorFactory implements ILogHubProcessorFactory 
{
  public ILogHubProcessor generatorProcessor()
  {   
      // Generates a consumption instance.
      return new SampleLogHubProcessor();
  }
}
```

Run the preceding codes to print all the data in a Logstore. To allow multiple consumers to consume one Logstore, follow the program annotations to modify the program, use the same consumer group name and different consumer names, and start other consumption processes.

## Limits and exception diagnosis {#section_i2r_qgc_ry .section}

Each Logstore can create at most 10 consumer groups.  The error `ConsumerGroupQuotaExceed` is reported when the number exceeds the limit.

We recommend that you configure Log4j for the consumer program, which is used to throw the errors occurred in the consumer group and locate the exceptions. Put the log4j.properties file to the resources directory and run the program, the following exception occurs:

```
[WARN ] 2018-03-14 12:01:52,747 method:com.aliyun.openservices.loghub.client.LogHubConsumer.sampleLogError(LogHubConsumer.java:159)
com.aliyun.openservices.log.exception.LogException: Invalid loggroup count, (0,1000]
```

See the following log4j.properties configuration for reference:

```
log4j.rootLogger = info,stdout
log4j.appender.stdout = org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target = System.out
log4j.appender.stdout.layout = org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern = [%-5p] %d{yyyy-MM-dd HH:mm:ss,SSS} method:%l%n%m%n
```

## Status and alarm {#section_psb_f4z_c2b .section}

1.  [View the consumer group status on the console](intl.en-US/User Guide/Real-time subscription and consumption/View consumer group status.md)
2.  [View the consumer group delay with CloudMonitor and configure the alarm](intl.en-US/User Guide/Real-time subscription and consumption/Consumer group - Monitoring alarm.md)

## Advanced Configuration {#section_crq_m4z_c2b .section}

For ordinary users, the data can be consumed using the program above, advanced configurations will be discussed in the following.

-   Want to consume data that starts at a certain time

    The loghubconfig in the code above has two constructors:

    ```
    // The consumerstarttimeinseconds parameter represents the number of seconds after 1970, meaning that  the data after this is consumed.
    public LogHubConfig(String consumerGroupName, 
                          String consumerName, 
                          String loghubEndPoint,
                          String project, String logStore,
                          String accessId, String accessKey,
                          int consumerStartTimeInSeconds);
    // Position is an enumeration variable, loghubconfig. glaseposition. begin_cursor indicates that consumption starts with the oldest data, loghubconfig. glaseposition. end_cursor indicates that consumption starts with the latest data.
    public LogHubConfig(String consumerGroupName, 
                          String consumerName, 
                          String loghubEndPoint,
                          String project, String logStore,
                          String accessId, String accessKey,
                          ConsumePosition position);
    ```

    You can use different construction methods according to consumer needs, but note that if the server is saved with checkpoint, then the starting consumption position is based on the checkpoint saved by the server.

-   Use RAM user to access Log Service

    You need to set the ram permissions associated with the consumer group, and set the method to reference the documentation of the ram, the permissions that need to be set are as follows:


|Action|Resource|
|:-----|:-------|
|log:GetCursorOrData|acs:log:**$\{regionName\}**:**$\{projectOwnerAliUid\}**:project/**$\{projectName\}**/logstore/**$\{logstoreName\}**|
|log:CreateConsumerGroup|acs:log:**$\{regionName\}**:**$\{projectOwnerAliUid\}**:project/**$\{projectName\}**/logstore/**$\{logstoreName\}**/consumergroup/\*|
|log:ListConsumerGroup|acs:log:**$\{regionName\}**:**$\{projectOwnerAliUid\}**:project/**$\{projectName\}**/logstore/**$\{logstoreName\}**/consumergroup/\*|
|log:ConsumerGroupUpdateCheckPoint|acs:log:**$\{regionName\}**:**$\{projectOwnerAliUid\}**:project/**$\{projectName\}**/logstore/**$\{logstoreName\}**/consumergroup/**$\{consumerGroupName\}**|
|log:ConsumerGroupHeartBeat|acs:log:**$\{regionName\}**:**$\{projectOwnerAliUid\}**:project/**$\{projectName\}**/logstore/**$\{logstoreName\}**/consumergroup/**$\{consumerGroupName\}**|
|log:UpdateConsumerGroup|acs:log:**$\{regionName\}**:**$\{projectOwnerAliUid\}**:project/**$\{projectName\}**/logstore/**$\{logstoreName\}**/consumergroup/**$\{consumerGroupName\}**|
|log:GetConsumerGroupCheckPoint|acs:log:**$\{regionName\}**:**$\{projectOwnerAliUid\}**:project/**$\{projectName\}**/logstore/**$\{logstoreName\}**/consumergroup/**$\{consumerGroupName\}**|

-   Reset the consumption point

    In some scenarios \(fill data, repeat the calculation\), we need to set a ConsumerGroup point to a certain point in time, so that the current consumer groups can start to consume from the new point. There are two ways:

    1.  Delete consumer group
        -   Delete consumer group on the console, and restart consumer group program.
        -   consumer group program start to consume from default starting point \(configured by program\)
    2.  Reset the current consumer group to a certain point-in-time using SDK
        -   The program and Java code example are as follows
        -   Restart the consumer program by using the SDK to modify the site.
    ```
    Client client = new Client(host, accessId, accessKey);
    long time_stamp = Timestamp.valueOf("2017-11-15 00:00:00").getTime() / 1000;
    ListShardResponse shard_res = client.ListShard(new ListShardRequest(project, logStore));
    ArrayList<Shard> all_shards = shard_res.GetShards();
    for (Shard shard: all_shards)
    {
      shardId = shard.GetShardId();
      long cursor_time = time_stamp;
      String cursor = client.GetCursor(project, logStore, shardId, cursor_time). GetCursor();
      client.UpdateCheckPoint(project, logStore, consumerGroup, shardId, cursor);
    }
    ```


