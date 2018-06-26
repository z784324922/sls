# Log topic {#concept_ukf_1rn_vdb .concept}

Logs in a Logstore can be classified by log topics. You can specify the topic when writing and querying logs. For example, as a platform user, you can use your user ID as the log topic when writing logs. In this way, you can select to only view your own logs based on the log topic when querying logs. If you do not need to classify the logs in a Logstore, use the same topic for all of the logs.

**Note:** A null string is a valid log topic and is the default log topic when writing and querying logs.  So if you do not need to use the log topic, the easiest way is to use the default log topic, the null string, when writing and querying logs.

The relationship among Logstores, log topics, and logs is as follows.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13012/2389_en-US.png)

