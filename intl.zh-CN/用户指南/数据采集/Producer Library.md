# Producer Library {#concept_ax1_n45_vdb .concept}

LogHub Producer Library 是针对应用程序高并发写LogHub类库，Producer Library 和[Consumer Library](intl.zh-CN/用户指南/实时订阅与消费/消费组消费.md)是对LogHub的读写包装，降低数据收集与消费的门槛。

## 功能特点 {#section_g2t_fj4_y1b .section}

-   提供异步的发送接口，线程安全。
-   可以添加多个Project的配置。
-   可以配置用于发送的网络 I/O 线程数量。
-   可以配置merge成的包的日志数量以及大小。
-   内存使用可控，当内存使用达到用户配置的阈值时，Producer 的 send 接口会阻塞，直到有空闲的内存可用。

## 功能优势 {#section_zbz_pqb_ry .section}

-   客户端日志不落盘：既数据产生后直接通过网络发往服务端。
-   客户端高并发写入：例如一秒钟会有百次以上写操作。
-   客户端计算与 IO 逻辑分离：打日志不影响计算耗时。

在以上场景中，Producer Library 会简化您程序开发的代价，帮助您批量聚合写请求，通过异步的方式发往 LogHub 服务端。在整个过程中，您可以配置批量聚合的参数，服务端异常处理的逻辑等。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13029/2612_zh-CN.png)

以上各种接入方式的对比：

|接入方式|优点/缺点|针对场景|
|:---|:----|:---|
|日志落盘 + logtail|日志收集与打印日志解耦，无需修改代码。|常用场景|
|syslog + logtail|性能较好（80MB/S），日志不落盘，需支持 syslog 协议。|syslog 场景|
|SDK 直发|不落盘，直接发往服务端，需要处理好网络 IO 与程序 IO 之间的切换。|日志不落盘|
|Producer Library|不落盘，异步合并发送服务端，吞吐量较大。|日志不落盘，客户端 QPS 高|

## 配置步骤 {#section_erg_rrj_pdb .section}

-   [Java Producer](https://github.com/aliyun/aliyun-log-producer-java?spm=a2c4g.11186623.2.6.plcQxS)
-   [Log4J1.XAppender\(基于Java Producer\)](https://github.com/aliyun/aliyun-log-log4j-appender?spm=a2c4g.11186623.2.7.plcQxS)
-   [Log4J2.XAppender\(基于Java Producer\)](https://github.com/aliyun/aliyun-log-log4j2-appender?spm=a2c4g.11186623.2.8.plcQxS)
-   [LogBack Appender\(基于Java Producer\)](https://github.com/aliyun/aliyun-log-logback-appender?spm=a2c4g.11186623.2.9.plcQxS)
-   [C Producer](https://github.com/aliyun/aliyun-log-c-sdk?spm=a2c4g.11186623.2.10.plcQxS)
-   [C Producer Lite](https://github.com/aliyun/aliyun-log-c-sdk/tree/lite?spm=a2c4g.11186623.2.11.plcQxS)

