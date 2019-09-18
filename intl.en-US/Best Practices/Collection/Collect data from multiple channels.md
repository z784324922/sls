# Collect data from multiple channels {#concept_43816_zh .concept}

Log Service LogHub supports real-time data collection and consumption. It supports more than 30 real-time collection approaches.

Data is usually collected in two different modes as described in the following table. This topic describes how to use the streaming import feature of LogHub to collect data in real time.

|Mode|Advantage|Disadvantage|Example|
|:---|:--------|:-----------|:------|
|Batch import|High throughput, focusing on historical data|Poor real-time performance|FTP-based import, uploading from OSS, mailing hard drives, and SQL-based data export to LogHub|
|Streaming import|Real-time, what you see is what you get \(WYSIWYG\), focusing on real-time data|High requirements on the collection terminals|LogHub-based data collection, HTTP-based uploading, Internet of Things \(IoT\) data collection, and Queue|

## Background {#section_flt_kkk_5fb .section}

"I Want Take-away" is an e-commerce website with a platform involving users, restaurants, and couriers. Users can place their take-away orders on the website, app, WeChat, or Alipay. Once receiving an order from a user, a merchant starts preparing the ordered dishes. At the same time, the system automatically notifies the nearest couriers. Then, one of the couriers accepts the order and delivers the dishes to the user.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13197/156877446431813_en-US.png)

## Operation requirements {#section_980_2p1_wus .section}

During operations, the following issues are identified:

-   It is difficult to attract new users. In some other cases, a hefty advertising investment in various marketing channels, such as webpage ads and WeChat push messages, attracts some new users. However, it is difficult to evaluate the effectiveness of each channel.
-   Users often complain about the slow delivery. However, it is difficult to locate the delayed phase, for example, order taking, delivery, or preparing the order. In addition, "I Want Take-away" has no clue how to improve the situation.
-   User operations teams often organize promotions, such as giving away coupons, but cannot get expected results.
-   In terms of scheduling, "I Want Take-away" wants to know how to facilitate merchants to stock up food for peak hours and dispatch more couriers to a specific area.
-   As for the customer service, "I Want Take-away" wants to analyze the reason why some users fail to place orders, for example, whether any inappropriate actions were performed by the users before they report the failure or whether any system errors occur.

## Data collection challenges {#section_0oe_j7g_y14 .section}

In digital operations, the first step is to figure out how to centrally collect the distributed log data. The following challenges are encountered during the process:

-   Multiple channels: advertisers and street promotions \(leaflets\)
-   Multiple terminals: webpages, official social media accounts, mobile phones, desktop browsers, and mobile browsers
-   Heterogeneous networks: Virtual Private Cloud \(VPC\), user-created Internet data center \(IDC\), and Alibaba Cloud Elastic Compute Service \(ECS\)
-   Various development languages: Java for the core system, NGINX for front-end servers, and C++ for the back-end payment system
-   Devices: merchants' devices adopting different architectures, such as x86 and ARM

In the past, you need to complete a large amount of diversified work to collect the logs distributed externally and internally for unified management. But now, you can use the collection feature of LogHub for unified access.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13197/156877446731889_en-US.png)

## Unified log management and configuration {#section_m0q_ute_s6m .section}

1.  Create a log management project, for example, myorder.
2.  Create Logstores for logs generated from different data sources, for example:
    -   wechat-server \(for storing WeChat server access logs\)
    -   wechat-app \(for storing WeChat server application logs\)
    -   wechat-error \(for storing WeChat error logs\)
    -   alipay-server
    -   alipay-app
    -   deliver-app \(for storing logs related to the courier app status\)
    -   deliver-error \(for storing delivery error logs\)
    -   web-click \(for storing HTML5 webpage click logs\)
    -   server-access \(for storing server-side access logs\)
    -   server-app \(for storing application logs\)
    -   coupon \(for storing application coupon logs\)
    -   pay \(for storing payment logs\)
    -   order \(for storing order logs\)
3.  To cleanse raw data and run Extract-Transform-Load \(ETL\) jobs on the raw data, create intermediate Logstores.

## User promotion log collection {#section_mxn_s33_ge3 .section}

The following two approaches are commonly used to attract new users:

-   Offer coupons upon website registration.
-   Offer coupons for scanning QR codes on other channels, such as:
    -   QR codes on leaflets
    -   QR codes on webpages

## Implementation {#section_08e_m36_umv .section}

Define the following registration server link and generate QR codes for leaflets and webpages. When a user scans the QR code on a webpage for registration, you can identify the user as being from a specific source, and create logs accordingly.

``` {#codeblock_5xo_clv_cre}
http://examplewebsite/login?source=10012&ref=kd4b
			
```

When receiving a request, the server generates the following logs:

``` {#codeblock_tap_puw_6nd}
2016-06-20 19:00:00 e41234ab342ef034,102345,5k4d,467890
			
```

The logs contain the following parameters:

-   time: the time of registration.
-   session: the current session of the browser. It is used for behavior tracking.
-   source: the source channels. For example, Campaign A is labeled as 10001, leaflets 10002, and elevator advertisements 10003.
-   ref: the account of someone who recommends the user to sign up. If no one recommends the user to sign up, leave this parameter blank.
-   params: other parameters.

Collection methods:

-   The application generates logs to hard disks by using [Logtail]().
-   The application writes data to Log Service by using the [SDK]().

## Server-side data collection {#section_gln_gxv_68e .section}

Alipay or WeChat official account programming is a typical web-side model that generally utilizes the following four types of logs:

-   NGINX or Apache access logs: used for monitoring and real-time statistics.

    ``` {#codeblock_wd1_em1_ac9}
    10.1.168.193 - - [01/Mar/2012:16:12:07 +0800] "GET /Send? AccessKeyId=8225105404 HTTP/1.1" 200 5 "-" "Mozilla/5.0 (X11; Linux i686 on x86_64; rv:10.0.2) Gecko/20100101 Firefox/10.0.2"
    					
    ```

-   NGINX or Apache error logs

    ``` {#codeblock_9t8_1ia_r4h}
    2016/04/18 18:59:01 [error] 26671#0: *20949999 connect() to unix:/tmp/fastcgi.socket failed (111: Connection refused) while connecting to upstream, client: 10.101.1.1, server: , request: "POST /logstores/test_log HTTP/1.1", upstream: "fastcgi://unix:/tmp/fastcgi.socket:", host: "ali-tianchi-log.cn-hangzhou-devcommon-intranet.   sls.aliyuncs.com"
    					
    ```

-   Application-layer logs: capture the event details, such as the time, location, result, delay, method, and parameters. This type of log usually ends with extended parameters.

    ``` {#codeblock_z4r_6lq_ubf}
    {
        "time":"2016-08-31 14:00:04",
        "localAddress":"10.178.93.88:0",
        "methodName":"load",
        "param":["31851502"],
        "result":....
        "serviceName":"com.example",
        "startTime":1472623203994,
        "success":true,
        "traceInfo":"88_1472621445126_1092"
    }
    					
    ```

-   Application-layer error logs: record the error details, such as the time, code line, error code, and reason.

    ``` {#codeblock_0yn_57b_9fv}
    2016/04/18 18:59:01 :/var/www/html/SCMC/routes/example.php:329 [thread:1] errorcode:20045 message:extractFuncDetail failed: account_hsf_service_log
    					
    ```


## Implementation {#section_x38_7te_fqd .section}

-   To write logs to a local file, specify regular expressions in the Logtail Config to write the logs to the specified Logstore.
-   To collect logs generated in a Docker container, integrate Container Service with Log Service.
-   To collect logs for Java programs, use Log4J Appender \(without saving logs to hard disks\) or LogHub Producer Library \(for high-concurrency client-side write\).
-   To collect logs for C\#, Python, Java, PHP, and C programs, use the corresponding SDKs.

## Access to end user logs {#section_lcp_ccn_o36 .section}

-   Mobile clients: Use the SDK for iOS or Android, or Mobile Analytics \(MAN\) for log access.
-   ARM devices: Use the native C code for cross-compiling.
-   Merchant platform devices: Use the corresponding SDK on x86 servers for log access. Use the native C code on ARM devices for cross-compiling.

## Collection of user behavior data for desktop or mobile websites {#section_gst_3z0_kl7 .section}

The user behavior can be divided into the following two types:

-   User behavior that has interaction with the back-end server, such as placing an order, logging on, and logging out.
-   User behavior that has no interaction with the back-end server, including sending requests that are processed directly at the front end, such as scrolling or closing a page.

## Implementation {#section_mbu_3fk_rkn .section}

-   To collect data of user behavior that has interaction with the back-end server, refer to the implementation approaches for server-side data collection.
-   To collect data of user behavior that has no interaction with the back-end server, use Tracking Pixel or JS Library.

## Server log O&M {#section_end_odu_hbs .section}

Example:

-   System logs

    ``` {#codeblock_8j1_h8n_6wl}
    Aug 31 11:07:24 zhouqi-mac WeChat[9676]: setupHotkeyListenning event NSEvent: type=KeyDown loc=(0,703) time=115959.8 flags=0 win=0x0 winNum=7041 ctxt=0x0 chars="u" unmodchars="u" repeat=0 keyCode=32
    					
    ```

-   Application debug logs

    ``` {#codeblock_4u2_4ph_4vm}
    __FILE__:build/release64/sls/shennong_worker/ShardDataIndexManager.cpp
    __LEVEL__:WARNING
    __LINE__:238
    __THREAD__:31502
    offset:816103453552
    saved_cursor:1469780553885742676
    seek count:62900
    seek data redo
    log:pangu://localcluster/redo_data/41/example/2016_08_30/250_1472555483
    user_cursor:1469780553885689973
    					
    ```

-   Trace logs

    ``` {#codeblock_sgb_kaa_hyr}
    [2013-07-13 10:28:12.772518]    [DEBUG] [26064]  __TRACE_ID__:661353951201    __item__:[Class:Function]_end__  request_id:1734117   user_id:124 context:.....
    					
    ```


## Implementation {#section_jz4_76g_da1 .section}

For more information, see the implementation approaches for server-side data collection.

## Data collection in different network environments {#section_3fq_ot8_gae .section}

LogHub provides access points in each region with the following three access approaches:

-   Internal network \(classic network\): accessible to services within the current region. We recommend that you use this access approach for its optimal link quality.
-   Public network \(classic network\): accessible without any limits. The transmission speed varies depending on the link quality. We recommend that you use HTTPS to ensure secure transmission of data.
-   Private network \(VPC\): accessible to services in the current region through VPCs.

