# Comparison with the ELK Stack in log query and analysis {#concept_wh3_tmq_zdb .concept}

## Background {#section_jvw_rwy_zdb .section}

To realize real-time log analysis, many people may think of using the popular Elasticsearch, Logstash, and Kibana \(known as the ELK Stack\) to build a project. The ELK Stack is an open-source solution that has accumulated a large number of topics and use cases in the community.

Alibaba Group provides Alibaba Cloud [Log Service](https://www.alibabacloud.com/zh/product/log-service?spm) as a solution to log scenarios. Its predecessor was a monitoring and diagnosis solution that came into shape in early 2012 when Alibaba Cloud was researching and developing the Apsara system. As the number of users grows and the business evolves, Alibaba Cloud has applied Log Service to log analysis in Ops scenarios, such as DevOps, Market Ops, and DevSecOps. During its development, Log Service has taken on challenges in scenarios such as the Double 11 Shopping Festival, Ant Financial Double 12 Shopping Festival, Spring Festival red envelopes, and international business. It is now a product that serves both Alibaba Group and external users.

## Orientation for log query and analysis {#section_url_qkj_ngb .section}

As an open-source search engine software library supported by the Apache Software Foundation, Apache Lucene provides full-text searching and indexing and text analysis capabilities. Lucene was originally written by Doug Cutting, who is also a founder of Apache Hadoop. Lucene joined the Apache Software Foundation in 2001. Founded in 2012, Elastic developed Elasticsearch based on the Lucene library and launched the ELK Stack in 2015 as an integrated solution to log collection, storage, and query. Lucene was designed to retrieve information based on documents. It has only limited log processing capabilities. For example, Lucene provides limited log processing scale, query performance, and support for custom functions such as LogReduce. Alibaba Cloud Log Service uses a self-developed log storage engine. In the past three years, Alibaba Cloud has applied Log Service to tens of thousands of applications. Log Service supports indexing for data in units of PB per day and serves tens of thousands of developers to query and analyze data hundreds of millions of times per day. In Alibaba Group, Log Service serves all Alibaba Cloud products, such as SQL audit, EagleEye, Cloud Map, sTrace, and Ditecting.

Log query is the most basic requirement of DevOps. According to the industry research report [50 Most Frequently Used UNIX/Linux Commands](https://www.thegeekstuff.com/2010/11/50-linux-commands/), the tar and grep commands rank the first and second, respectively. This shows the importance of log query to programmers.

Here, we make a comprehensive comparison of the application of Log Service and the ELK Stack in log query and analysis scenarios from the following aspects:

-   Ease of use: the cost when you get started and use the solution.
-   Features \(key point\): the query and analysis capabilities.
-   Performance \(key point\): the query and analysis requirements for unit data volume and how is the latency.
-   Scale \(key point\): the data volume that can be processed and the scalability.
-   Cost \(key point\): the cost for using the same feature and performance.

## Ease of use {#section_hqd_twy_zdb .section}

The use of a log analysis system involves the following phases:

1.  Collection: writes data in a stable manner.
2.  Configuration: configures the data source.
3.  Resizing: uses more data sources and machines to extend the storage space and scale out machines.
4.  Usage: provides query and analysis features, which are described in the Features section of this topic.
5.  Export: exports data to other systems for further processing, such as stream computing in StreamCompute and backup in Object Storage Service \(OSS\).
6.  Multi-tenancy: shares data with other users and uses data securely.

The following table lists the comparison results between Log Service and the ELK Stack in terms of ease of use.

|Item|Sub-item|On-premises ELK Stack|Log Service|
|:---|:-------|:--------------------|:----------|
|Collection|Protocol|RESTful API| -   RESTful API
-   Java Database Connectivity \(JDBC\)

 |
|Client|Various products in the ecosystem, including Logstash, Beats, and Fluentd| -   Logtail, which is the major component
-   Others, such as Logstash

 |
|Configuration|Unit|Uses indexes to differentiate logs.| -   Project
-   Logstore

 Uses a project to function as a namespace and contain multiple Logstores.

 |
|Attribute|API + Kibana| -   API + SDK
-   Console

 |
|Resizing|Storage| -   Adds machines.
-   Purchases cloud disks.

 |No operation is needed.|
|Computing|Adds machines.|No operation is needed.|
|Configuration| -   Configures machines in the configuration management system.
-   Logstash has supported centralized configuration in a beta version.

 |Uses the console or API, without the need for a configuration management system.|
|Collection point|Installs configuration and Logstash on machine groups in the configuration management system.|Uses the console or API, without the need for a configuration management system.|
|Capacity|Does not support dynamic resizing.|Supports dynamic resizing and auto scaling.|
|Export|Method| -   API
-   SDK

 | -   API
-   SDK
-   Kafka consumer API
-   Stream computing engines, such as Spark, Storm, and Flink
-   Stream computing class libraries, such as Python and Java

 |
|Multi-tenancy|Security|Commercial version| -   HTTPS
-   Transmission with a signature
-   Multi-tenant data segregation
-   Access control

 |
|Throttling|No throttling| -   Project-level throttling
-   Shard-level throttling

 |
|Multi-tenancy|Kibana support|Native account and permission management|

Conclusion:

-   The ELK Stack has extensive support from various products in the ecosystem and supports many write, installation, and configuration tools.
-   Log Service is a highly integrated hosting service that is easy to access, configure, and use. Common users can access Log Service within 5 minutes.
-   Log Service is a software as a service \(SaaS\)-based service. You do not need to worry about capacity or concurrency. It supports auto scaling and is O&M-free.

## Features \(query and analysis\) {#section_mf4_wwy_zdb .section}

The query feature quickly hits logs that meet query criteria. The analysis feature collects statistics and computes data.

For example, you have an analysis requirement that intends to collect statistics by IP address on the number and traffic of all read requests with an HTTP status code greater than 200. This analysis requirement can be converted to two operations: querying specified results and conducting statistical analysis for the results. In some cases, you can directly analyze all logs without a prior query.

``` {#codeblock_08d_sss_c6a}
1. Status in (200,500] and Method:Get*
2. select count(1) as c, sum(inflow) as sum_inflow, ip group by Ip
```

-   Basic query capability

    The following table lists the comparison results based on [Elasticsearch 6.5 Indices](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices.html).

    |Type|Sub-type|On-premises ELK Stack|Log Service|
    |:---|:-------|:--------------------|:----------|
    |Text|Query by index|Supported|Supported|
    |Word-breaking|Supported|Supported|
    |Chinese word-breaking|Supported|Supported|
    |Prefix|Supported|Supported|
    |Suffix|Supported|Not supported|
    |Fuzzy|Supported|Supported by SQL|
    |Wildcast|Supported|Supported by SQL|
    |Numeric|Long|Supported|Supported|
    |Double|Supported|Supported|
    |Nested|JSON|Supported|Not supported|
    |Geo|Geo|Supported|Supported by SQL|
    |IP address|Query by IP address|Supported|Supported by SQL|

    Conclusion:

    -   The ELK Stack supports more data types and provides a stronger native query capability than Log Service.
    -   Log Service can use SQL statements in replacement of fuzzy string matching and comparison functions such as Geo. However, the query performance is slightly worse than the native query performance. The following examples show how to use SQL statements to query data:
    ``` {#codeblock_uzj_6pv_th7}
    Queries data that hits the specified substring:
    * | select content where content like '%substring%' limit 100
    
    Queries data that matches the specified regular expression:
    * | select content where regexp_like(content, '\d+m') limit 100
    
    Parses JSON-formatted data and queries data that matches the specified query criterion:
    * | select content where json_extract(content, '$.store.book')='mybook' limit 100
    
    If a JSON-type index is created, you can also specify the query criterion as follows:
    field.store.book='mybook'
    ```

-   Extended query capability

    In log analysis scenarios, you may need to perform follow-up operations on queried data. For example:

    1.  After finding an error log, check the context to find out the parameter that causes the error.
    2.  After identifying an error, check for similar errors. For example, run the tail -f command to view the content of raw log files and run the grep command to query similar data.
    3.  After obtaining millions of log entries from a query by keyword, filter out 90% of known issues that distract you.
    To resolve the preceding issues, Log Service provides the following closed-loop solutions:

    -   Context Lookup: allows you to look up the context of a log entry in raw log files by page, without the need to log on to the server.
    -   LiveTail: runs the tail -f command in the cloud and allows you to view the content of raw log files in real time.
    -   LogReduce: dynamically classifies logs based on their similar patterns to detect anomalies.
    1.  [LiveTail](../../../../reseller.en-US/Index and query/Query/LiveTail.md) \(tail -f command in the cloud\)

        According to the traditional O&M method, you must run the `tail -f` command on log files on the server to monitor the log files in real time. If a large amount of log data is obtained in real time, you can run the `grep` or `grep -v` command to filter data by keyword. Log Service provides LiveTail in the console for you to monitor and analyze online log data in real time. This makes O&M easier.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13138/156584022537745_en-US.png)

        LiveTail has the following features:

        -   Supports various data sources, including Docker, Kubernetes, servers, and Log4j Appender.
        -   Monitors log data in real time, and allows you to specify keywords and filter data by keyword.
        -   Performs word-breaking for log fields to query the context of log entries that contain the specified field after word-breaking.
    2.  [LogReduce](../../../../reseller.en-US/Index and query/Query/Use LogReduce to group log data.md) 

        With the rapid development of business, you need more stable systems. Considering that each system generates a large number of logs every day, you may have the following concerns:

        -   Your system has potential anomalies, which, however, cannot be effectively detected in large amounts of log data.
        -   Intruders have logged on to your machine without permission, but you have no idea until it is too late.
        -   After a new version is published, the system behavior changes, but you are unaware of the changes. The root cause is that the system records too much information but fails to classify it. Logs that record the information have no schema and vary in format, and therefore cannot be well classified. Log Service provides LogReduce to classify logs based on their similarity to help you quickly have a full view of log data. LogReduce has the following features:
            -   Supports logs in all formats, such as Log4j, JSON, and syslog.
            -   Filters logs by any criterion, uses the Reduce and Pattern functions to classify the filtered log data, and then queries the raw data based on a signature.
            -   Compares the patterns of different time periods.
            -   Dynamically adjusts the precision of the Reduce function.
            -   Obtains the query result from hundreds of millions of data entries in seconds.

## Analysis capability {#section_apk_cxy_zdb .section}

Elasticsearch provides the aggregation syntax based on Lucene DocValues and supports the SQL syntax in version 6.x to compute data by group and aggregation. Log Service supports the complete SQL-92 standard syntax in compliance with RESTful API and JDBC. In addition to the basic aggregate functions, Log Service also supports the complete SQL computing functions and provides self-developed functions, such as those for union query that joins internal and external data sources, machine learning, and pattern analysis.

**Note:** The following figure shows the comparison results based on [Elasticsearch 6.5 Aggregations](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations.html) and [Log Service analysis syntax](../../../../reseller.en-US/Index and query/Syntax description.md#).

In addition to the SQL-92 standard syntax, Log Service also develops a series of useful functions based on the actual log analysis requirements.

1.  [Interval-valued comparison and periodicity-valued comparison functions](../../../../reseller.en-US/Index and query/Analysis grammar/Interval-valued comparison and periodicity-valued comparison functions.md) 

    You can use interval-valued comparison and periodicity-valued comparison functions in SQL statements to compare calculations of different time windows to gain insight into the growth trend. The result of each calculation can be a single value, multiple values, or a curve.

    ``` {#codeblock_lq6_568_vay}
    * | select compare( pv , 86400) from (select count(1) as pv from log)
    ```

    ``` {#codeblock_1db_d79_7q2}
    *|select t, diff[1] as current, diff[2] as yesterday, diff[3] as percentage from(select t, compare( pv , 86400) as diff from (select count(1) as pv, date_format(from_unixtime(__time__), '%H:%i') as t from log group by t) group by t order by t) s 
    ```

2.  [Union query that joins internal and external data sources](../../../../reseller.en-US/Index and query/Analysis grammar/Logstore and RDS join query.md) 

    You can join internal and external data sources in a union query, which has the following features:

    -   Supports data sources such as Logstore, MySQL, and OSS \(CSV files\).
    -   Supports LEFT JOIN, RIGHT JOIN, FULL JOIN, and INNER JOIN.
    -   Uses SQL statements to query data in external tables and join internal and external tables.
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13138/156584022637750_en-US.png)

    The following example shows how to join internal and external tables to query data.

    ``` {#codeblock_wbu_ifh_qo4}
    SQL statements
    Creates an external table:
    * | create table user_meta ( userid bigint, nick varchar, gender varchar, province varchar, gender varchar,age bigint) with ( endpoint='oss-cn-hangzhou.aliyuncs.com',accessid='LTA288',accesskey ='EjsowA',bucket='testossconnector',objects=ARRAY['user.csv'],type='oss')
    
    Uses the external table:
    * | select u.gender, count(1) from chiji_accesslog l join user_meta1 u on l.userid = u.userid group by u.gender
    ```

3.  Geo location functions

    You can use geo location functions to analyze user sources based on IP addresses, mobile numbers, and geographic data. Geo location functions include:

    -   ip: obtains the country, province, city, longitude and latitude, and carrier of an IP address.
    -   mobile: obtains the carrier, province, and city of a mobile number.
    -   geohash: converts geographic data to coordinates.
    The following example shows how to analyze the query results:

    ``` {#codeblock_vwv_vqg_lri}
    SQL statements
    * | SELECT count(1) as pv, ip_to_province(ip) as province WHERE ip_to_domain(ip) ! = 'intranet' GROUP BY province ORDER BY pv desc limit 10
    
    * | SELECT mobile_city(try_cast("mobile" as bigint)) as "city", mobile_province(try_cast("mobile" as bigint)) as "province", count(1) as "number of requests" group by "province", "city" order by "number of requests" desc limit 100
    ```

    -   [Geographic functions](../../../../reseller.en-US/Index and query/Analysis grammar/Geo functions.md)
    -   [Phone number functions](../../../../reseller.en-US/Index and query/Analysis grammar/Phone number functions.md)
    -   [IP address functions](../../../../reseller.en-US/Index and query/Analysis grammar/IP functions.md)
4.  [Security detection functions](../../../../reseller.en-US/Index and query/Analysis grammar/Security detection functions.md) 

    Based on the globally shared white hat security asset library, Log Service provides security detection functions. You can use security detection functions to check whether an IP address, domain name, or URL in logs is secure.

    -   security\_check\_ip
    -   security\_check\_domain
    -   security\_check\_url
5.  [Machine learning](../../../../reseller.en-US/Index and query/Machine learning syntax and functions/Overview.md) and time series-based detection functions

    Log Service provides new machine learning and intelligent diagnostic functions to:

    -   Automatically learn the rules of historical data and predict the future trend.
    -   Detect imperceptible abnormal changes in real time, and analyze the characteristics that cause anomalies in combination with analysis functions.
    -   Intelligently detect exceptions and inspect the system based on the period-on-period comparison and alerting features. This applies to intelligent O&M, security, and operations to gain insight into data in a faster, more effective, and more intelligent manner.
    These functions have the following features:

    -   Prediction: fits the baseline based on historical data.
    -   Anomaly detection, change point detection, and inflexion point detection: finds anomalies.
    -   Time series forecasting: finds the periodic rules of data access.
    -   Time series clustering: finds time series data with different curve shapes.
6.  Pattern analysis functions

    You can use pattern analysis functions to detect the characteristics and rules in data to help you quickly and accurately identify problems. These functions have the following features:

    1.  Finds the frequent pattern in statistical patterns. For example, 90% of invalid requests are sent by a user ID.
    2.  Finds the pattern that causes differences between two collections under specified conditions. For example:
        -   In requests with a latency greater than 10 seconds, the ratio of combined dimensions that contain an ID is much higher than that of other combined dimensions.
        -   The ratio of this ID in collection B is lower than that in collection A.
        -   Collections A and B are significantly different.
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13138/156584022637754_en-US.png)


## Performance {#section_xcv_hxy_zdb .section}

By using the same dataset, this section compares the performance of Log Service and the ELK Stack in terms of data write, data query, and aggregation.

-   Test environment
    1.  Test configuration

        |Item|On-premises ELK Stack|Log Service|
        |:---|:--------------------|:----------|
        |Runtime environment|Elastic Compute Service \(ECS\) instance \(4 vCPUs and 16 GB memory\) × 4 + Ultra disks or SSDs|N/A|
        |Shard|10|10|
        |Number of replicas|2|3 \(configured by default and invisible to users\)|

    2.  Test data

        -   Five columns of the double type, five columns of the long type, and five columns of the text type, with the values in alphabetical order based on the initial letter of field names as follows: 256, 512, 768, 1024, and 1280
        -   Random fields
        -   Size of raw data: 50 GB
        -   Number of log lines: 162,640,232 \(about 160 million log entries\)
        The sample test log is as follows:

        ``` {#codeblock_4oq_1wu_smx}
        timestamp:August 27th 2017, 21:50:19.000 
        long_1:756,444 double_1:0 text_1:value_136 
        long_2:-3,839,872,295 double_2:-11.13 text_2:value_475 
        long_3:-73,775,372,011,896 double_3:-70,220.163 text_3:value_3 
        long_4:173,468,492,344,196 double_4:35,123.978 text_4:value_124 
        long_5:389,467,512,234,496 double_5:-20,10.312 text_5:value_1125
        ```

-   Write test results

    To write multiple data entries at a time, the ELK Stack uses a bulk API, whereas Log Service calls the PostLogstoreLogs operation. The following table lists the test results.

    |Item|Sub-item|On-premises ELK Stack|Log Service|
    |:---|:-------|:--------------------|:----------|
    |Latency|Average write latency|40 ms|14 ms|
    |Storage|Data volume of a replica|86 GB|58 GB|
    |Expansion rate: Data volume/Raw data size|172%|116%|

    **Note:** Log Service charges a total storage fee for 50 GB, where 23 GB is occupied by the written and compressed raw data and 27 GB is occupied by indexing.

    Conclusion:

    -   Log Service has a shorter data write latency than the ELK Stack.
    -   The data stored in Log Service occupies a smaller space than that in the on-premises ELK Stack. The size of raw data is 50 GB. The storage space expands because the test data is random. In most live-network scenarios, the storage space after compression is smaller than the size of raw data. The data stored in the on-premises ELK Stack expands to 86 GB, with an expansion rate of 172%, which is 58% higher than that in Log Service. For the ELK Stack, this expansion rate is approximate to the recommended value, where the storage space is 2.2 times the size of raw data.
-   Read \(query and analysis\) test
    -   Test scenarios

        This test uses two common scenarios as an example: log query and aggregation. It calculates the average latency in the two scenarios when the number of concurrent requests is 1, 5, and 10, respectively. The two scenarios are as follows:

        1.  Uses a GROUP BY clause on any text column of full data, calculates the AVG, MIN, MAX, SUM, and COUNT values of five numeric columns, and then sorts the calculated values by the COUNT value to obtain the first 1,000 results:

            ``` {#codeblock_zqf_esi_egr}
            select count(long_1) as pv,sum(long_2),min(long_3),max(long_4),sum(long_5) 
              group by text_1 order by pv desc limit 1000
            ```

        2.  Queries a random keyword, for example, value\_126, in logs of full data, and then obtains the first 100 lines of log entries that hit the keyword:

            ``` {#codeblock_bqz_vva_c5m}
            value_126
            ```

    -   Test results

        |Scenario|Number of concurrent requests|Latency of the on-premises ELK Stack \(Unit: seconds\)|Latency of Log Service \(Unit: seconds\)|
        |:-------|:----------------------------|:-----------------------------------------------------|:---------------------------------------|
        |Analysis|1|3.76|3.4|
        |5|3.9|4.7|
        |10|6.6|7.2|
        |Query|1|0.097|0.086|
        |5|0.171|0.083|
        |10|0.2|0.082|

    -   Result analysis
        -   According to the test results, both Log Service and the ELK Stack can query and analyze 150 million data entries within seconds.
        -   In the analysis scenario, the latency is similar between Log Service and the ELK Stack. The ELK Stack uses SSD and delivers better I/O performance than Log Service when a large amount of data is read.
        -   In the query scenario, Log Service has a much shorter latency than the ELK Stack. As the number of concurrent requests increases, the latency of the ELK Stack increases, whereas that of Log Service remains stable and even decreases.

## Scale and cost {#section_pp7_2oc_z7l .section}

-   Processing scale

    1.  Log Service can perform indexing for data in units of PB per day and query data among dozens of TB within seconds at a time. It supports auto scaling and scale-out for the processing scale.
    2.  The ELK Stack is suitable for scenarios where data is written in units of GB to TB per day and stored in units of TB. The processing scale is constrained by the following factors:
        -   Single cluster scale: An ideal cluster consists of about 20 nodes. In the industry, a cluster can contain up to 100 nodes and is often split into multiple clusters to process business data in a convenient manner.
        -   Write capacity: After shards are created, the write capacity cannot be modified. As the throughput increases, more shards are required to dynamically scale up nodes. The maximum number of available nodes equals the number of shards.
        -   Storage capacity: When the primary shard reaches the maximum disk capacity, it must be migrated to another disk with larger capacity, or more shards must be allocated. The typical solution is to create an index, specify more shards, and rebuild existing data.
    Example about the processing scale

    Customer A is one of the major information websites in China. It has thousands of machines and hundreds of developers. The O&M team used to build a cluster based on the ELK Stack to process Nginx logs. However, the cluster always failed when processing a large scale of data. For example:

    1.  The cluster stops responding when processing a large amount of data for a user query. This makes the cluster unavailable to process data for other user queries.
    2.  During peak hours, the cluster is fully occupied and busy collecting and processing data. As a result, the cluster fails to guarantee data integrity and the accuracy of query results.
    3.  When the business grows to a certain scale, out of memory \(OOM\) often occurs due to memory settings and heartbeat synchronization. The cluster cannot guarantee its availability and accuracy. In this case, this cluster does not work properly and is abandoned by the development team.
    In June 2018, the O&M team began to use Log Service, which provides the following features:

    1.  Uses Logtail to collect online logs, and uses API operations to integrate collection configuration and machine management into the customer's own O&M and control systems.
    2.  Embeds the Log Service query page into the unified logon and O&M platform to separate business permissions from account permissions.
    3.  Embeds Log Service console pages into the customer's own platform to enable the development team to query logs. Uses Grafana plug-ins to interconnect with Log Service and monitor business. Uses DataV to interconnect with Log Service and build a dashboard.
    **Note:** For more information, see the following topics:

    -   [Console sharing embedment](../../../../reseller.en-US/Index and query/Query and visualization/Other visualization methods/Console sharing embedment.md)
    -   [Interconnection with Grafana](../../../../reseller.en-US/Index and query/Query and visualization/Other visualization methods/Interconnection with Grafana.md)
    -   [Interconnect with DataV big screen](../../../../reseller.en-US/Index and query/Query and visualization/Other visualization methods/Interconnect with DataV big screen.md)
    -   [OpenTracing implementation of Jaeger](../../../../reseller.en-US/Index and query/Query and visualization/Other visualization methods/OpenTracing implementation of Jaeger.md)
    After two months, the customer's O&M has been improved as follows:

    1.  The number of queries per day has increased significantly. The development team is gradually getting used to log query and analysis on the O&M platform, which improves their R&D efficiency. The O&M team also revokes online logon permissions.
    2.  In addition to Nginx logs, the O&M platform also imports app logs, mobile logs, and container logs. The processing scale is 10 times that before Log Service is used.
    3.  More applications are developed. For example, after Jaeger plug-ins are used to interconnect with Log Service, a tracing system is built based on logs to generate daily alerts and reports for online errors and accordingly carry out inspection.
    4.  Various platforms are interconnected with the unified O&M platform to collect data in a uniform manner and avoid repeated data collection. For example, the platforms of the big data department, such as Spark and Flink, can directly subscribe to and process real-time log data.
-   Cost

    Based on the preceding test data, this section calculates the average monthly cost in the scenario where 50 GB of data is written on a daily basis and stored for 90 days. The actual data size is 23 GB.

    -   The billing items of Log Service include read and write operations, indexing, and storage space. The query feature is free of charge. For more information about the billing standards, see [Billing method](../../../../reseller.en-US/Pricing/Billing method.md).

        |Billing item|Quantity|Unit price|Cost \(Unit: RMB\)|
        |:-----------|:-------|:---------|:-----------------|
        |Read and write operations|23 GB × 30 days|RMB 0.2/GB|138|
        |Storage space \(data stored for 90 days\)|50 GB × 90 days|RMB 0.3/GB per month|1,350|
        |Indexing|27 GB × 30 days|RMB 0.35/GB|283|
        |Total|N/A|N/A|1,771|

    -   The billing items of the ELK Stack include machines and data storage on SSD.

        -   Generally, cloud disks provide high reliability. Therefore, the storage of replicas is not billed in this topic.
        -   Generally, 15% of the available space must be reserved on a storage disk to prevent the space from being fully occupied. Therefore, a coefficient of 1.15 is multiplied.
        |Billing item|Quantity|Unit price|Cost \(Unit: RMB\)|
        |:-----------|:-------|:---------|:-----------------|
        |Server|Server with 4 vCPUs and 16 GB memory × 4 × Three months \(ecs.mn4.xlarge\)|Subscription: RMB 675/month|2,021|
        |Storage|86 GB × 1.15 × 90 days \(only one replica is calculated\)|SSD: RMB 1/GB per month|8,901|
        |N/A|SATA: RMB 0.35/GB per month|3,115|
        |Total|SSD: 12,943|
        |SATA: 5,135|

        With the same performance delivered, the cost ratio of Log Service to the ELK Stack \(SSD\) is 13.6%. SSD can be replaced with SATA to lower the cost of the on-premises ELK Stack. In this case, the cost ratio of Log Service to the ELK Stack \(SATA\) is 34%. However, the latency increases from 40 ms \(SSD\) to 150 ms \(SATA\). After data is read and written for a long time, the query and read/write latency greatly increases and the on-premises ELK Stack cannot work properly.

-   Time to value

    In addition to hardware costs, Log Service is basically free of charge for new data import, new services, maintenance, and resource resizing. It also provides the following features:

    -   Seamless interconnection with various log processing systems in the ecosystem, such as Spark, Hadoop, Flink, and Grafana
    -   Global deployment in more than 20 regions, which helps you expand business on a global scale
    -   More than 30 SDKs that can seamlessly interconnect and integrate Log Service with other Alibaba Cloud products

## Summary {#section_7ka_cty_7xa .section}

Elasticsearch supports more common scenarios such as update, query, and delete. It is widely used in fields such as search, data analysis, and application development. The ELK Stack maximizes the flexibility and performance of Elasticsearch in log analysis scenarios. Log Service is designed for log data analysis scenarios and has customized and developed many applications in this field. The former has a wide service range, whereas the latter is more targeted. Of course, the comparison of data is meaningless if scenarios are not considered. Both of them are useful in suitable scenarios.

