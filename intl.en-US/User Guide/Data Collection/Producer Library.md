# Producer Library {#concept_ax1_n45_vdb .concept}

LogHub Producer Library is a LogHub class library written for high-concurrency Java applications. Producer Library and [Consumer Library](intl.en-US/User Guide/Real-time subscription and consumption/Consumer group - Usage.md) are the read and write packaging for LogHub to lower the threshold for data collection and consumption. 

## Function features {#section_g2t_fj4_y1b .section}

-   Provides an asynchronous send interface to guarantee the thread security.
-   Configurations of multiple projects can be added.
-   The number of network I/O threads used for sending logs can be configured.
-   The number and size of logs of a merged package can be configured.
-   The memory usage is controllable. When the memory usage reaches your configured threshold value, the send interface of producer is blocked until idle memory is available.

## Function advantages {#section_zbz_pqb_ry .section}

-   Logs collected from the client are not flushed into the disk. Data is directly sent to Log Service by using the network after being generated.
-   High concurrency write operations on the client. For example, more than one hundred write operations are performed in one second.
-   Client computing logically separated from I/O. Printing logs does not affect the computing time used.

In the preceding scenarios, Producer Library simplifies your program development steps, aggregates write requests in batches, and sends the requests to the LogHub  server asynchronously.  During the process, you can configure the parameters for aggregation in batches and the logic to process server exception.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13029/2612_en-US.png)

Compare the preceding access methods:

|Access method|Advantages/disadvantages|Scenario|
|:------------|:-----------------------|:-------|
|Log flushed into the disk + Logtail|Log collection decoupled from logging, no need to modify the code.|Common scenarios|
|Syslog + Logtail|Good performance \(80 MB/s\). Logs are not flushed into the disk. The syslog protocol must be supported.|Syslog scenarios.|
|SDK direct transmission|Not flushed into the disk, and directly sent to the server. Switching between the network I/O and program I/O must be properly processed.|Logs are not flushed into the disk.|
|Producer Library|Not flushed into the disk, asynchronously merged and sent to the server, with good throughput.|Logs are not flushed into the disk and the client QPS is high.|

## Procedure {#section_erg_rrj_pdb .section}

-   [Java Producer](https://github.com/aliyun/aliyun-log-producer-java?spm=a2c4g.11186623.2.6.plcQxS)
-   [Log4J1. Log4J1.XAppender \(based on Java  Producer\)](https://github.com/aliyun/aliyun-log-log4j-appender?spm=a2c4g.11186623.2.7.plcQxS)
-   [Log4J2. XAppender \(based on Java  Producer\)](https://github.com/aliyun/aliyun-log-log4j2-appender?spm=a2c4g.11186623.2.8.plcQxS)
-   [LogBack Appender \(based on Java  Producer\)](https://github.com/aliyun/aliyun-log-logback-appender?spm=a2c4g.11186623.2.9.plcQxS)
-   [C Producer](https://github.com/aliyun/aliyun-log-c-sdk?spm=a2c4g.11186623.2.10.plcQxS)
-   [C Producer Lite](https://github.com/aliyun/aliyun-log-c-sdk/tree/lite?spm=a2c4g.11186623.2.11.plcQxS)

