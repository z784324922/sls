# Query and analyze AppLogs {#concept_59355_zh .concept}

## Features of AppLogs {#section_xsp_pzp_5fb .section}

-   Complete information: AppLogs are provided by programmers, covering key locations, variables, and exceptions. Technically, over 90% of bugs in the production environment can be located based on AppLogs.

-   Arbitrary formats: One piece of code is often developed by multiple programs who have their preferred formats. Therefore, the formats are difficult to be unified. Style inconsistency is also seen in logs introduced from third-party databases.

-   Similarity among AppLogs: Despite of the arbitrary formats, different AppLogs share things in common. For example, the following information is required for Log4J logs:

    -   Time
    -   Level
    -   File or class
    -   Line number
    -   Thread ID

## Challenges in processing AppLogs {#section_3gl_wh4_slr .section}

-   **Large data volume** 

    The size of AppLogs is generally one order of magnitude larger than that of access logs. Assume that a website has one million independent PVs each day, each PV involves about 20 logic modules, and 10 major logic points in each logic module need to be logged.

    The total number of the logs is calculated by using the following formula:

    ``` {#codeblock_tow_er5_wmb}
    1,000,000 × 20 × 10 = 2 × 10^8 entries
    ```

    Assume that the length of each entry is 200 Bytes. The total storage size is calculated by using the following formula:

    ``` {#codeblock_9ey_0f8_1g6}
    2 × 10^8 × 200 = 4 × 10^10 Bytes = 40 GB
    ```

    The data size grows as the business system becomes increasingly complex. It is common for a medium-sized website to generate 100 to 200 GB of log data every day.

-   **Multiple distributed applications** 

    Most applications are stateless and run in different frameworks, such as:

    -   Server
    -   Docker \(container\)
    -   Function Compute \(Container Service\)
    The numbers of corresponding instances vary from a few to thousands. Multiple instances require a cross-server log collection scheme.

-   **Complex runtime environments** 

    Programs are running in different environments, such as:

    -   Application logs are stored in Docker containers.
    -   API logs are stored in Function Compute.
    -   Old system logs are stored in traditional IDCs.
    -   Mobile-side logs are stored in users' mobile devices.
    -   Mobile web logs are stored in browsers.
    To have a complete picture of the log data, you need to unify and store the logs in a single place.


## Schemes {#section_b1f_1xr_bgb .section}

## Unified data storage {#section_bbf_1xr_bgb .section}

Purpose: to collect data from different sources into a central place to facilitate subsequent operations.

You can create a project in [Log Service](https://www.aliyun.com/product/sls/) to store AppLogs. Log Service supports over 30 collection methods, such as tracking in physical severs, entering JavaScript code on the mobile web side, and exporting logs on servers.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13206/156894339932435_en-US.png)

Apart from writing logs by using methods like SDK, Log Service provides a convenient, stable, and high-performance agent, namely, Logtail, for server logs. Logtail provides two versions for Windows and Linux. Once you have defined the machine group and completed the log collection configuration, server logs can be collected in real time.

After the log collection configuration is complete, you can operate on logs in the project.

Compared with other log collection agents, such as Logstash, Flume, FluentD, and Beats, Logtail has following advantages:

-   Easy to use: provides APIs and remote management and monitoring capabilities. Logtail is designed with the rich experience of Alibaba Group in million-level server log collection and management. This enables you to configure a collection point for hundreds of thousands of devices in seconds.
-   Adaptive to different environments: Logtail supports the public network, VPCs, and user-created IDCs. The HTTPS and resumable upload features make it possible to integrate with data on the public network.
-   Great performance with a little consumption of resources: With years of refinement, Logtail is superior to its open-source competitors in performance and resource consumption.

## Quick fault locating {#section_c1f_1xr_bgb .section}

Purpose: to ensure that the time it takes to locate problems is constant, regardless of how the data volume increases and how servers are deployed.

For example, quickly locate an order error and a long latency issue out of TBs of data every week. The process also involves filtering and investigation based on multiple conditions.

1.  For example, you can investigate AppLogs with latency details for the requests with a latency of over 1 second and methods starting with Post.

    ``` {#codeblock_0lx_425_5we}
    Latency > 1000000 and Method=Post*
    ```

2.  Search for logs that contain the keyword "error" and do not contain the keyword "merge".
    -   Result of a day

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13206/156894339932437_en-US.png)

    -   Result of a week

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13206/156894339932438_en-US.png)

    -   Result a larger time span

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13206/156894339932439_en-US.png)

        The results of any time span can be returned in less than 1 second.


## Association analysis {#section_bl3_cxr_bgb .section}

There are two types of association: intra-process association and inter-process association. The difference between the two types is as follows:

-   Intra-process association: This type of association is simple because the old and new logs of a function are stored in the same file. In a multi-thread process, you can filter logs by thread ID.
-   Cross-process association: Usually, it is hard to find clear clues when dealing with logs from different processes. The association is generally performed by passing the TracerId parameter to Remote Procedure Call \(RPC\).

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13206/156894339932440_en-US.png)

-   **Context association** 

    Locate an error log by running a keyword-based query in the Log Service console.

    Click Context View, and see the preceding and following results.

    -   You can click **Old** and **New** for more results.
    -   You can also highlight the results by entering keywords.
-   **Cross-process association** 

    The concept of cross-process association, also known as tracing, can be dated back to the famous article [Dapper, a Large-Scale Distributed Systems Tracing Infrastructure](http://research.google.com/pubs/pub36356.html) by Google in 2010. Inspired by the article, developers from the open source sector created many affordable versions of tracers. Currently, the following versions are well-known:

    -   Dapper \(Google\): the foundation for all tracers.
    -   StackDriver Trace \(Google\): compatible with ZipKin.
    -   Zipkin: an open-source tracing system by Twitter.
    -   Appdash: a tracer of the Golang version.
    -   Hawkeye: developed by the Middleware Technology Department of Alibaba Group.
    -   X-ray: introduced at AWS re:Invent 2016.
    Applying a tracer from scratch is easier than in an existing system because of the costs and challenges in adapting it to the system.

    Based on Log Service, you can implement a basic tracing feature. To obtain all required logs, you can record associative fields such as Request\_id and OrderId in logs of different modules and search for the fields in various Logstores.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13206/156894339932443_en-US.png)

    For example, you can search logs of front-end servers, back-end servers, payment systems, and order systems by using SDKs. After the results are returned, you can create a page on the front end to associate the cross-process calls. The following figure shows the tracing system built based on Log Service.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13206/156894339932444_en-US.png)


## Statistical analysis {#section_d2b_s9o_9bq .section}

After the specific logs are located, you can analyze the logs, for example, calculating the types of online error logs.

1.  You can query logs by `__level__`. For example, 20 errors are found within one day.

    ``` {#codeblock_yj5_hh4_c02}
    __level__:error
    ```

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13206/156894339932445_en-US.png)

2.  To determine the unique log type, you can analyze and aggregate data based on the file and line fields.

    ``` {#codeblock_jlg_bpb_seq}
    __level__:error | select __file__, __line__, count(*) as c group by __file__, __line__ order by c desc
    ```

    Then, the results of all types and locations of errors can be returned.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13206/156894339932446_en-US.png)


Besides, you can locate and analyze IP addresses with certain error codes or high latency.

## Other features {#section_yo0_ep2_nya .section}

-   Log backup for auditing

    You can back up the logs to OSS, Infrequent Access \(IA\) storage which has a lower storage cost, or MaxCompute.

-   Keyword alerting

    Alerts can be reported in the following ways:

    -   [Enable alerting by Log Service.](../../../../reseller.en-US/Alarm/Configure an alarm/Configure an alert.md)
    -   [Enable alerting by CloudMonitor.](../../../../reseller.en-US/Monitor Log Servic/Monitor Log Service.md)
-   Log query permission management

    You can grant permissions to RAM users or a user group to isolate the development environment and the production environment.

    Price and cost: In this scheme, AppLogs mainly use the LogHub and LogSearch features of Log Service. Compared with an open source scheme, this scheme is an easy-to-use scheme with only 25% of the cost of an open source scheme. This improves the development efficiency.


