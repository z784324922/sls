# What is Log Service {#concept_mt2_ykn_vdb .concept}

As a one-stop service for log data, Log Service \(Log for short\) experiences massive big data scenarios of Alibaba Group. Log Service allows you to quickly complete the collection, consumption, shipping, query, and analysis of log data without the need for development, which improves the Operation & Maintenance \(O&M\) efficiency and the operational efficiency, and builds the processing capabilities to handle massive logs in the DT \(data technology\) era.

Log Service has the following core functions.

## Real-time log collection and consumption \(LogHub\) {#section_ox2_dcd_jbb .section}

Functions:

-   Use Elastic Compute Service \(ECS\), containers, mobile terminals, open-source softwares, and JS to access real-time log data \(such as Metric, Event, BinLog, TextLog, and Click data\).
-   A real-time consumption interface is provided to interconnect with real-time computing and service.

Purposes: ETL, Stream Compute, monitoring and alarm, machine learning, and iterative computing.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13002/2357_en-US.png)

## LogShipper {#section_prt_5hd_jbb .section}

Stable and reliable log shipping ships LogHub data to storage services for storage and big data analysis. Supports various storage methods such as compression, user-defined partitions, row storage, and column storage.

Purposes: Data warehouse + data analysis, audit, recommendation system, and user profiling.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13002/2363_en-US.png)

## Query and real-time analysis \(Search/Analytics\) {#section_z3x_mcd_jbb .section}

Index, query, and analyze data in real time.

-   Query: Keyword, fuzzy match, context, and range.
-   Statistics: Rich query methods such as SQL aggregation.
-   Visualization: Dashboard and report functions.
-   Interconnection: Grafana and JDBC/SQL92.

Purposes: DevOps/online O&M, real-time log data analysis, security diagnosis and analysis, and operation and customer service systems.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13002/2364_en-US.png)

