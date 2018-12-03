# Cost advantages {#concept_ngk_v5n_vdb .concept}

## Cost advantages {#section_zbr_w5n_vdb .section}

Log Service has the following cost advantages in three log processing scenarios:

-   Loghub:
    -   A more cost-effective choice for users in 98% scenarios compared to building Kafka with purchased cloud hosts + cloud disks. At less than 30% of the Kafka cost for small websites. 
    -   Provides RESTful APIs and supports data collection on mobile devices, saving you the cost of the gateway servers for log collection.
    -   Operation & Maintenance \(O&M\) -free and auto scaling anytime and anywhere.
-   Logshipper:
    -   No code/machine resources required, flexible configuration, and rich monitoring data.
    -   Linear scalability \(PB grade/day\), available for free currently.
-   Logsearch/analytics:
    -   At less than 15% of the cost of purchasing cloud hosts + self-building ELK, and offers dramatic enhancement in query capability and data processing scale. See Comparison report. A better choice than the above-mentioned log management softwares for its ability to seamlessly integrate with various  popular stream computing + offline computing frameworks to allow for unobstructed flow of logs. 

## Cost Comparison {#section_yr3_cvn_vdb .section}

The following is the comparison of Log Service and self-built solutions in the billing model, for your reference only.

## LogHub \(LogHub vs Kafka\) {#section_gjs_dvn_vdb .section}

|-|Focus|LogHub|Self-built middleware \(such as Kafka\)|
|:-|:----|:-----|:--------------------------------------|
|Decompress the file by using|New|Imperceptible|O&M required|
|Expansion|Imperceptible|O&M required|
|Increase backups|Imperceptible|O&M required|
|Multitenancy|Quarantine|Might affect each other|
|Charge|Internet collection \(10 GB/day\)  | USD 2/day| USD 16.1/day|
|Internet collection \(1 TB/day\)| USD 162/day| USD 800/day|
|Intranet collection \(small data size\)|-|-|
|Intranet collection \(moderate data size\)|-|-|
|Intranet collection \(large data size\)|-|-|

## Log Storage and Query Engine {#section_ojs_dvn_vdb .section}

|Focus-|LogSearch|ES \(Lucene Based\)|NoSQL|Hive|
|:-----|:--------|:------------------|:----|:---|
|Scale|Scale|PB|TB|PB|PB|
|Cost| Store \(USD/GB per day\)|0.0115|3.6|0.02|0.035|
|Write \(USD/GB\) |0.35|5|0.4|0|
|Query \(US $/GB\) |0|0|0.2|0.3|
|Speed-query|Millisecond level-second level|Millisecond level-second level|Within milliseconds|In minutes|
|Speed-statistics  |Weak+|Relatively strong|Weak|Strong|
|Latency|Write-\>queryable  |Real time|In minutes|Real time|Ten-minute level|

**Note:** 

The price comparisons here are calculated basically based on the fact that softwares are deployed on Elastic Compute Service \(ECS\) and three copies have been configured.

For more information, see Comparisons of log query solutions.[Compare LogSearch/Analytics with ELK in log query and analysis](../../../../reseller.en-US/User Guide/Index and query/Advanced analysis/Compare LogSearch__Analytics with ELK in log query and analysis.md)

