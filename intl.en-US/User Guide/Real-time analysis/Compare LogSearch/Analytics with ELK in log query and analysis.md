# Compare LogSearch/Analytics with ELK in log query and analysis {#concept_wh3_tmq_zdb .concept}

When talking about real-time log analysis, people will think of using ELK \(Elastic, Logstash, and Kibana\) to implement it.  ELK Stack is an open-source solution that has accumulated many contents and use cases in the community.

The new version of Alibaba Cloud Log Service adds enhancements of LogSearch/LogAnalytics to support real-time indexing, query, and analysis of log data and optimize query performance and data volume computing in many aspects. This document conducts a comprehensive comparison and analyzes the aspects that you pay attention.

-   Ease of use: The cost when you get started and use the function.
-   Functions \(important\): Query and analysis.
-   Performance \(important\): The query and analysis requirements for unit data volume and how is the latency.
-   Scale \(important\): The data volumes that can be processed and the scalability.
-   Cost \(important\): The cost for using the same function and performance.

## Ease of use {#section_hqd_twy_zdb .section}

A log analysis system is used in the following procedures:

1.  Collection: Write data in a stable manner.
2.  Configuration: How to configure the data source.
3.  Expansion: Access more data sources and machines. Expand the storage space and machines.
4.  Usage: Described in the Functions section.
5.  Export: Whether data can be conveniently exported to other systems for operations such as streaming computing and storage in OSS for backup purposes
6.  Multi-tenant: The way data is shared to others and whether or not data can be used securely.

Comparison results:

|Item|Sub item|Self-built ELK|LogSearch/Analytics|
|:---|:-------|:-------------|:------------------|
|Collection|Protocol|Restful API|Restful API|
| |Agent|Logstash/Beats/Fluentd, with rich ecosystem|Logtail \(main\) + Others \(for example, Logstash\)|
|Configuration|Unit|Use index to differentiate logs|Project + Logstore. Provide a concept of two levels. A project is considered as a namespace, and multiple Logstores can be created in a project|
| |Attribute|API + kikana|API + SDK + Console|
|Expansion|Storage|Add machines and purchase cloud disks|No operation is needed|
| |machine |Add machines|No operation is needed|
| |Configuration|Configure Logstash and apply Logstash to machines by using the configuration management system|Perform operations in the console or by using APIs, without using the configuration management system|
| |Collection point|Install configuration and Logstash on a machine group by using the configuration management system|Perform operations in the console or by using APIs, without using the configuration management system|
|Export|Method|API/SDK|API/SDK +  Stream computing engines \(Spark, Storm, Flink, and CloudMonitor\) + Storage \(OSS\)|
|Multi-tenant|Safety|None \(non-commercial version\)|HTTPS + Transmission signature + Multi-tenant isolation + Access control|
| |Decompress the file by using|Same account|Sub-account, role, product, and temporary authorization|

Conclusion:

The ELK  has many ecosystem and write tools and supports many installation and configuration tools.  LogSearch/Analytics is a hosting service with a high degree of integration in terms of access, configuration, and usage. Normal users can access LogSearch/Analytics in five minutes,  without worrying about the capacity and concurrency issues. The billing method is Pay-As-You-Go and auto scaling is supported.

## Functions \(query and analysis\) {#section_mf4_wwy_zdb .section}

The query function enables quick hitting of logs that comply with search criteria. The analysis function performs statistics and computing of data.

For example, you have an analysis requirement that intends to collect statistics by IP address on the number and traffic of all read requests with a status code greater than 200. This analysis requirement can be converted to two operations: query specified results and perform statistical analysis of the results. In some cases, you can directly analyze all the logs without query.

```
1. Status in (200,500] and Method:Get* 
 2. select count(1) as c, sum(inflow) as sum_inflow, ip group by Ip
```

## Comparison of query capability {#section_hkc_lqc_ry .section}

|Type|Sub item|Self-built ELK|LogSearch/Analytics|
|:---|:-------|:-------------|:------------------|
|Text |Index query|Supported|Supported|
| |Word segmentation|Supported|Supported|
| |Chinese word segmentation|Supported|Not supported|
| |Prefix|Supported|Supported|
| |TLD|Supported| |
| |Fuzzy|Supported|Supported|
| |Wildcast|Supported|Not supported|
|Numeric value|long|Supported|Supported|
| |double|Supported|Supported|
|Nested|JSON query|Supported| |
|Geo|Geo query|Supported|Not directly supported. You can use the range query to have the same effect|
|Ip|IP address query|Supported|Not directly supported. You can use the string query to have the same effect|
|Context|Contextual Query| |Supported|
| |Context filter| |Supported|

Elasticsearch supports more data types and more advanced query methods.  LogSearch/Analytics supports most of the common queries with unique features \(for example, context query and expansion of program logs\).

## Comparison of analysis capability {#section_apk_cxy_zdb .section}

-   [ES 5.5 aggregation](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations.html)
-   [Syntax description](intl.en-US/User Guide/Real-time analysis/Syntax description.md)

|Type|Sub item|Self-built ELK|LogSearch/Analytics|
|:---|:-------|:-------------|:------------------|
|Interface|Method|API/SDK|API/SDK + SQL92|
| |Other protocols| |JDBC|
|Agg|Bucketing|Supported|Supported|
| |Metric|Supported|Supported|
| |Matrix|Supported|Supported|
| |Pipeline |Limited support|Full support|
|Arithmetic operation|Numeric value| |Supported|
| |String| |Supported|
| |Estimation| |Supported|
| |Mathematical statistics| |Supported|
| |Date Conversion| |Supported|
|GroupBy|Agg|Supported|Supported|
| | Having condition | |Supported|
|Sort|Sort| |Supported|
|Join|Join of multiple tables| |Supported|

LogSearch/LogAnalytics provides a superset of functions compared with Elasticsearch and fully supports SQL-92. LogSearch/LogAnalytics can be directly used in SQL writing scenarios.

## Performance {#section_xcv_hxy_zdb .section}

By using the same data set, compare the self-built ELK and LogSearch/Analytics in terms of data writing, data query, and aggregation.

**Hands-on Environment**

1.  Test configuration

    |Type|Self-built ELK|LogSearch/Analytics|
    |:---|:-------------|:------------------|
    |Environment |Elastic Compute Service \(ECS\) instance \(4 core and 16 GB\) x 4 + Efficient SSD cloud disk|-|
    |Shard|10|10|
    |Number of copies|2|3 \(configured by default and invisible to users\) |

2.  Test data

    -   Five columns of double-type data.
    -   Five columns of long-type data.
    -   Five columns of text-type data, with the dictionary sizes 256, 512, 768, 1024, and 1280, respectively.
    The preceding fields are random. The following is a sample test log:

    ```
    timestamp:August 27th 2017, 21:50:19.000 
      long_1:756,444 double_1:0 text_1:value_136
      long_2:-3,839,872,295 double_2:-11.13 text_2:value_475 
     long_3:-73,775,372,011,896 double_3:-70,220.163 text_3:value_3
     long_4:173,468,492,344,196 double_4:35,123.978 text_4:value_124 
     long_5:389,467,512,234,496 double_5:-20,10.312 text_5:value_1125
    ```

3.  Size of the data set
    -   Size of raw data: 50 GB
    -   Size of raw data with the key removed: 27 GB \(LogSearch/Analytics uses this size as the unit of storage and billing.\)
    -   Number of log lines: 162,640,232 \(about 160 million logs\)

## Write test results {#section_uxt_rxy_zdb .section}

Elasticsearch writes data in batches using the Bulk API, whereas LogSearch/LogAnalytics performs batch write using the PostLogstoreLogs API. The results are shown as follows:

|Type|Item|Self-built ELK|LogSearch/Analytics|
|:---|:---|:-------------|:------------------|
|Latency|Average write latency|40 ms|14 ms|
|Storage|Data volume copied at a time| 86 GB |58 GB|
| |Expansion rate: Data volume/Raw data size|172%|121%|

**Note:** The storage size of LogSearch/Analytics that generates bills includes the volume of compressed raw data that has been written \(23 GB\) and the indexing traffic \(27 GB\), amounting to 50 GB in total.

According to the test results:

-   LogSearch/Analytics has a lower write latency \(14 ms\) than Elasticsearch \(40 ms\).
-   Space: The size of raw data is 50 GB. The storage space expands because the test data is random. \(In most real scenarios, the storage space after the compression is smaller than the size of raw data.\)  The storage space occupied by Elasticsearch expands to 86 GB, with an expansion rate of 172%, which is  58% more than the storage space occupied by LogSearch/Analytics.

## Read \(query + analysis\) test {#section_zlh_vxy_zdb .section}

**Test scenario**

Use two common scenarios as an example: log query and aggregation.  The average latency in the two cases is counted when concurrency is 1, 5, and 10, respectively.

1.  Perform GROUP BY calculation on any text column of full data. Calculate the avg, min, max, sum, and count values of five columns and sort the values by count. The first 1,000 results are obtained. Example:

    ```
    select count(long_1) as pv,sum(long_2),min(long_3),max(long_4),sum(long_5) 
    group by text_1 order by pv desc limit 1000
    ```

2.  For full data, randomly query logs by using a keyword, such as  value\_126. Obtain the number of logs that meet the query condition and the first 100 log lines. Example:

    ```
    value_126
    ```


**Test results**

|Type|Number of concurrencies|Latency \(unit: seconds\) of Elasticsearch|Latency \(unit: seconds\) of LogSearch/Analytics|
|:---|:----------------------|:-----------------------------------------|:-----------------------------------------------|
|Case1: Analysis class|1|3.76|3.4|
| |5|3.9|4.7|
| |10|6.6|7.2|
|Case 2: Query|1|0.097|0.086|
| |5|0.171|0.083|
| |10|0.2|0.082|

**Results Analysis**

-   According to the test results, for the scale of 150 million data, both Elasticsearch and LogSearch/Analytics can query and analyze data within seconds.
-   In Case 1 \(statistics\), Elasticsearch and Log Service are at the same performance level in terms of latency.  Elasticsearch with SSD cloud disks has I/O advantage over Log Service when reading large amounts of data.
-   In Case 2 \(query\),   LogSearch/Analytics has much lower latency than Elasticsearch.  As concurrency increases, the latency of the ELK increases, while that of LogSearch/Analytics remains stable and even decreases.

## Scale { .section}

1.  LogSearch/Analytics can index petabytes of data in one day and query dozens of terabytes of data within seconds at a time, and supports auto scaling and horizontal scaling of data volume.
2.  Elasticsearch is applicable to writing gigabytes to terabytes of data in one day and storing terabytes of data.  The main limits are as follows:
    -   Single cluster scale: The ideal condition is that one cluster contains about 20 machines. In the industry, one cluster can contain up to 100 nodes.
    -   Expansion of write capability: The write capability cannot be modified after shards are created. Nodes are dynamically expanded when the throughput rate is increased. The maximum number of nodes that can be used is the number of shards.
    -   Storage expansion: When the primary shard reaches the upper limit of disk capacity, it must be migrated to another disk with larger capacity, or more shards must be allocated.  Generally, you can create an index, specify more shards, and rebuild existing data.

        LogSearch/Analytics does not have expansion issues because each shard is in distributed storage.  When the throughput rate is increased, shards can be dynamically split for horizontal scaling of the processing capability.


## Cost {#section_t5s_rzy_zdb .section}

Based on the preceding test data, this section calculates the average monthly cost in the case that 50 GB data is written on a daily basis and stored for 90 days \(the actual data size is 27 GB\).

1.  1.The billing method of Log Service LogSearch/Analytics includes read and write traffic, indexing traffic, and storage space. The query function is free of charge. For more information, see [Billing method](../../../../intl.en-US/Pricing/Billing method.md)

    |Billing item|Value|Unit price|Cost \(USD\)|
    |:-----------|:----|:---------|:-----------|
    |Read and write traffic|23 GB x 30|USD 0.2/GB|138|
    |Storage space \(data stored for 90 days\)|50 GB x 90|USD 0.3/GB x Month|1350|
    |Indexing traffic|27 GB x 30|USD 0.0875/GB|283|
    |In total|-|-|1771|

2.  The Elasticsearch costs include the machine costs and the costs of SSD cloud disks used for data storage

    -   Generally, cloud disks provide high reliability. Therefore, the storage of copies is not billed.
    -   Generally, for storage disks, 15% available space must be reserved to avoid full space occupation by written data. Therefore, a factor of 1.15 is multiplied.
    |Billing item|Value|Unit price|Cost \(USD\)|
    |:-----------|:----|:---------|:-----------|
    |Server|Server of 4 cores and 16 GB x 4 \(three months\) \(ecs.mn4.xlarge\)|Cost of monthly or yearly subscription: USD 675/month|2021|
    |Storage|86\*1.15\*90 \(only one copy is calculated here\)|SSD: USD 1/GB x Month|8901|
    | |-|SATA: USD 0.35/GB x Month|3115|
    |In total| | |12943 \(SSD\)|
    | | | |5135 \(SATA\)|


With the same performance, the cost ratio of LogSearch/Analytics to the ELK \(SSD\) is 13.6%.  During the test process, SSD is replaced with SATA to lower costs \(the cost ratio of LogSearch/Analytics to the ELK with SATA is  34%\). However, latency increases from 40 ms to 150 ms. After a long period of reading and writing, the query and read/write latency increases greatly and the query and analysis functions become abnormal.

## concluding remarks { .section}

Compared with the open-source ELK, LogSearch/Analytics provides the same query speed but higher throughput, more robust analysis capability, and a 87% cost reduction, with a support for the Pay-As-You-Go billing method and zero O&M, allowing you to focus on business analysis.

In addition to LogSearch/Analytics, Log Service also provides the LogHub and LogShipper functions and supports real-time data collection and interconnection with stream computing systems \(Spark, Storm, and Flink\) and offline analysis systems \(E-MapReduce, Presto, and Hive\) to provide a one-stop real-time data solution.

