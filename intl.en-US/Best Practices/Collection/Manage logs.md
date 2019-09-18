# Manage logs {#concept_44164_zh .concept}

## Log management {#section_g5z_t4k_5fb .section}

Here is a typical scenario: A server or container stores a large amount of application log data generated in different directories.

-   Developers deploy new applications and take them offline.
-   The server can scale in or out as needed. Specifically, the servers scale out during the peak periods and scale in during the slack periods.
-   The log data is to be queried, monitored, and warehoused depending on the different and ever-changing requirements.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13198/156877572232391_en-US.png)

## Challenges {#section_ate_uje_mih .section}

## 1. Fast application deployment and go-live, and a growing number of log types {#section_hpy_spk_5fb .section}

Each application can generate access, operations, logic, and error logs. When more applications are added and the dependence exists between applications, the volume of logs explodes.

The following table lists the different types of logs collected for a takeaway website.

|Category|Application|Log name|
|:-------|:----------|:-------|
|Web|NGINX|wechat-nginx \(for storing WeChat server NGINX logs\)|
|Web error|NGINX|alipay-nginx \(for storing Alipay server NGINX logs\)|
|NGINX|server-access \(for storing server-side access logs\)|
|NGINX error|alipay-nginx \(for storing NGINX error logs\)|
|NGINX error|...|
|Web app|Tomcat|alipay-app \(for storing Alipay server application logic\)|
|Tomcat|...|
|App|Mobile app|deliver-app \(for storing logs related to the courier app status\)|
|App error|Mobile app|deliver-error \(for storing delivery error logs\)|
|Web|HTML5|web-click \(for storing HTML5 webpage click logs\)|
|Server|Server|Server-side internal logic logs|
|Syslog|Server|Server system logs|

## 2. Log consumption for different purposes {#section_tqy_spk_5fb .section}

For example, access logs can be used for billing, and for users to download. Operations logs can be queried by a database administrator \(DBA\). These logs require Business Intelligence \(BI\) analysis and full-link monitoring.

## 3. Environment changes {#section_vej_ili_tzi .section}

With the incredibly fast evolution of the Internet, you need to adapt to the ever-changing business and environment in the real world.

-   Application server scale-out
-   Servers as machines
-   New application deployment
-   New log consumers

## A perfect management architecture requires: {#section_6zw_0h7_4yt .section}

-   A well-defined architecture with low cost
-   A stable and highly reliable, preferably unattended mechanism \(which, for example, allows for automatically scaling in or out servers as needed\)
-   Standardized application deployment without complicated configuration
-   Easy compliance with log processing requirements

## Log Service solution {#section_lkc_jpl_ynv .section}

Log Service LogHub uses Logtail to collect logs, involving the following concepts:

-   Project: a management container.
-   Logstore: indicates a log source.
-   Machine group: indicates the directory and format of logs.
-   Config: indicates the path to logs.

The relationships between these concepts are as follows:

-   A project includes multiple Logstores, machine groups, and Configs. Different projects can be used for different business purposes.
-   An application can have multiple types of logs. Each type of log has a Logstore and a fixed directory \(with the same Config\).

    ``` {#codeblock_zsx_yun_gc2}
        app --> logstore1, logstore2, logstore3
        app --> config1, config2, config3 
    					
    ```

-   A single application can be deployed on multiple machine groups, and multiple applications can be deployed on a single machine group.

    ``` {#codeblock_6pu_jn3_2ho}
        app --> machineGroup1, machineGroup2
        machineGroup1 --> app1, app2, app3
    					
    ```

-   The collection directories defined in the Logtail Configs are applied to the corresponding machine groups, and collected into any Logstore.

    ``` {#codeblock_h2y_pdb_ek9}
        config1 * machineGroup1 --> Logstore1
        config1 * machineGroup2 --> logstore1
        config2 * machineGroup1 --> logstore2
    					
    ```


## Advantages {#section_uvg_kfk_in0 .section}

-   Convenient: The web console or SDK is provided for batch management.

-   Large-scale: Millions of machines and millions of applications can be managed.

-   Real-time: The collection configuration takes effect within minutes.

-   Elastic:

    -   The machine ID feature is used for auto scaling of servers.
    -   LogHub supports auto scaling.
-   Stable and reliable: No human intervention is required.

-   Abundant query capabilities such as real-time computing, offline analysis, and indexing in log processing:

    -   LogHub: real-time collection and consumption. LogHub uses more than 30 approaches to collect large amounts of data for real-time downstream consumption.
    -   LogShipper: stable and reliable log shipping. It ships data from LogHub to storage services such as Object Storage Service \(OSS\), MaxCompute, and Table Store, for storage and big data analysis.
    -   LogSearch: real-time data indexing and querying. It allows for centralized log query without caring about where active server logs are located.

