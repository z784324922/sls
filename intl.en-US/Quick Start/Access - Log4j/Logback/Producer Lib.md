# Access - Log4j/Logback/Producer Lib {#concept_jml_d3q_zdb .concept}

In recent years, the advent of stateless programming, containers, and serverless programming greatly increased the efficiency of software delivery and deployment. In the evolution of the architecture, you can see the following two changes:

-   The application architecture is changing from a single system to microservices. Then, the business logic changes to the call and request between microservices.
-   In terms of resources, traditional physical servers are fading out and changing to the invisible virtual resources.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13020/5904_en-US.png "Architectural Evolution")

The preceding two changes show that behind the elastic and standardized architecture, the Operation & Maintenance \(O&M\) and diagnosis requirements are becoming more and more complex. Ten years ago, you could log on to a server and fetch logs quickly. However, the attach process mode no longer exists. Currently, we are facing with a standardized black box.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13020/5905_en-US.png "Changing Trends")

To respond to these changes, a series of DevOps-oriented diagnosis and analysis tools have emerged. These include centralized monitors, centralized log systems, and various SaaS deployment, monitoring, and other services.

Centralizing logs solves the preceding issues. To do this, after applications produce logs, the logs are transmitted to a central node server in real time \(or quasi-real time\). Often, Syslog, Kafka, ELK, and HBase are used to perform centralized storage.

## Advantages of centralisation {#section_xbz_dqq_12b .section}

-   Ease of use: Using Grep to query stateless application logs is troublesome. In the centralized storage, the previous long process is replaced by running a search command.
-   Separated storage and computing: When customizing machine hardware, you do not have to consider the storage space for logs.
-   Lower costs: Centralized log storage can perform load shifting to reserve more resources.
-   Security: In case of hacker intrusion or a disaster, critical data is retained as the evidence.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13020/5907_en-US.png "Advantages of centralisation")

## Collector \(Java series\) {#section_l1k_fqq_12b .section}

Log Service provides more than 30 data collection methods and comprehensive access solutions for servers, mobile terminals, embedded devices, and various development languages. Java developers need the familiar log frameworks:  Log4j, Log4j2, and Logback Appender.

Java applications currently have two mainstream log collection solutions:

-   Java programs flush logs to disks and use Logtail for real-time collection.
-   Java programs directly configure the Appender provided by Log Service. When the program is running, logs are sent to Log Service in real time.

Differences between the two:

| |Flush logs to disks + Use Logtail to collect logs|Use Appender for direct transmission|
|:-|:------------------------------------------------|:-----------------------------------|
|Timeliness|Logs are flushed to files and collected by using Logtail|Logs are directly sent to Log Service|
|Throughput|Big|Big|
|Resumable upload|Supported. Depends on the Logtail configuration|Supported. Depends on the memory size|
|Sensitive to application location|Required when configuring the collection machine group|Not required. Logs are initiatively sent|
|Local log|Supported|Supported|
|Disable collection|Delete Logtail configuration|Modify Appender configuration and restart the application|

By using Appender, you can use Config to complete real-time log collection easily without changing any code. The Java-series Appender provided by Log Service has the following advantages:

-   Configuration modifications take effect without modifying the program.
-   Asynchrony + breakpoint transmission: I/O does not affect main threads and can tolerate certain network and service faults.
-   High-concurrency design: Meets the writing requirements for massive logs.
-   Supports context query: Supports precisely restoring the context of a log \(N logs before and after the log\) in the original process in Log Service. 

## Overview and usage of Appender {#section_pbr_3qq_12b .section}

The provided Appenders are as follows. The underlying layers all use aliyun-log-producer-java to write data.

-   [aliyun-log-log4j-appender](https://github.com/aliyun/aliyun-log-log4j-appender?spm=a2c4e.11153959.blogcont409045.20.510e49bdIKC4NA)
-   [aliyun-log-log4j2-appender](https://github.com/aliyun/aliyun-log-log4j2-appender?spm=a2c4e.11153959.blogcont409045.21.510e49bdIKC4NA)
-   [aliyun-log-logback-appender](https://github.com/aliyun/aliyun-log-logback-appender?spm=a2c4e.11153959.blogcont409045.22.510e49bdIKC4NA)

Differences between the four:

|Appender name|Description|
|:------------|:----------|
|aliyun-log-log4j-appender|Developed for Log4j 1.x. If your application uses the Log4j 1.x log framework, we recommend that you use this Appender.|
|aliyun-log-log4j2-appender|Developed for Log4j 2.x. If your application uses the Log4j 2.x log framework, we recommend that you use this Appender.|
|aliyun-log-logback-appender|Appender developed for logback, if your application is using logback Developed for Logback. If your application uses the Logback log framework, we recommend that you use this Appender.|
|aliyun-log-producer-java|The LogHub class library used for high-concurrency log writing, which is programmed for Java applications. All the provided Appender underlying layers use this Appender to write data. This highly flexible Appender allows you to specify the fields and formats of the data written to LogHub.  If the provided Appender cannot meet your business needs, you can develop a log collection program based on this Appender as per your needs.|

## Step 1 Access Appender {#section_g4r_kqq_12b .section}

Access the Appender by following the steps in [aliyun-log-log4j-appender](https://github.com/aliyun/aliyun-log-log4j-appender?spm=a2c4e.11153959.blogcont409045.23.510e49bdIKC4NA) .

The contents of the configuration file `log4j.properties` are as follows:

```
log4j.rootLogger=WARN,loghub
log4j.appender.loghub=com.aliyun.openservices.log.log4j.LoghubAppender
# Log Service project name (required parameter)
log4j.appender.loghub.projectName=[your project]
# Log Service LogStore name (required parameter)
log4j.appender.loghub.logstore=[your logstore]
#Log Service HTTP address (required parameter)
log4j.appender.loghub.endpoint=[your project endpoint]
#(Mandatory) User identity
log4j.appender.loghub.accessKeyId=[your accesskey id]
log4j.appender.loghub.accessKey=[your accesskey]
```

## Step 2 Query and analysis {#section_hkt_mqq_12b .section}

After configuring the Appender as described in the previous step, the logs produced by Java applications are automatically sent to Log Service. You can use  [LogSearch/Analytics](../../../../intl.en-US/User Guide/Index and query/Overview.md) to query and analyze these logs in real time. See the sample log format as follows. Log formats used in this example:

-   Logs that record your logon behavior:

    ```
    level: INFO  
    location: com.aliyun.log4jappendertest.Log4jAppenderBizDemo.login(Log4jAppenderBizDemo.java:38)
    message: User login successfully. requestID=id4 userID=user8  
    thread: main  
    time: 2018-01-26T15:31+0000
    ```

-   Logs that record your purchase behavior:

    ```
    level: INFO  
    location: com.aliyun.log4jappendertest.Log4jAppenderBizDemo.order(Log4jAppenderBizDemo.java:46)
    message: Place an order successfully. requestID=id44 userID=user8 itemID=item3 amount=9  
    thread: main  
    time: 2018-01-26T15:31+0000
    ```


## Step 3 Enable query and analysis {#section_ifp_pqq_12b .section}

You must enable the query and analysis function before querying and analyzing data. Follow these steps to enable the function:

1.  Log on to the Log Service console.
2.  Click the project name on the Project List page.
3.  Click **Search** at the right of the Logstore.
4.  Click Enable in the**upper-right** \> **Modify.**.
5.  If you have enabled the index before, click Index Attributes \> Modify. The Search & Analysis page appears.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13020/5909_en-US.png "Specify a Query field")

## Step 4 Analyze logs {#section_q41_vqq_12b .section}

[Video tutorial](http://cloud.video.taobao.com//play/u/2450842572/p/1/e/6/t/1/50073510333.mp4)

1.  Count the top three locations where errors occurred most commonly in the last hour.

    ```
    level: ERROR | select location ,count(*) as count GROUP BY location ORDER BY count DESC LIMIT 3
    ```

2.  Count the number of generated logs for each log level in the last 15 minutes.

    ```
    | select level ,count(*) as count GROUP BY level ORDER BY count DESC
    ```

3.  Query the log context.

    For any log, you can precisely reconstruct the log context information for the original log file. For more information, see [Context query](../../../../intl.en-US/User Guide/Index and query/Context query.md).

4.  Count the top three users who have logged on most frequently in the last hour.

    ```
    Login | select maid (message, 'userid = (? <userID>[a-zA-Z\d]+)', 1) AS userID, count(*) as count GROUP BY userID ORDER BY count DESC LIMIT 3
    ```

5.  Compile payment total statistics for the past 15 minutes for each user.

    ```
    order | SELECT regexp_extract(message, 'userID=(? <userID>[a-zA-Z\d]+)', 1) AS userID, sum(cast(regexp_extract(message, 'amount=(? <amount>[a-zA-Z\d]+)', 1) AS double)) AS amount GROUP BY userID
    ```


