# 查询分析全方位对比（ELK） {#concept_wh3_tmq_zdb .concept}

提到日志实时分析，很多人都会想到很火的ELK Stack（Elastic/Logstash/Kibana）来搭建。ELK方案开源，在社区中有大量的内容和使用案例。

阿里云日志服务产品在新版中增强查询分析功能（LogSearch/Analytics），支持对日志数据实时索引与查询分析，并且对查询性能和计算数据量做了大量优化。在这里我们做一个全方位的比较，对于用户关心的点，我们依次展开分析：

-   易用：上手及使用过程中的代价
-   功能（重点）：主要针对查询与分析两块
-   性能（重点）：对于单位大小数据量查询与分析需求，延时如何
-   规模（重点）：能够承担的数据量，扩展性等
-   成本（重点）：同样功能和性能，使用分别花多少钱

## 易用 {#section_hqd_twy_zdb .section}

对日志分析系统而言，有如下使用过程：

1.  采集：将数据稳定写入
2.  配置：如何配置数据源
3.  扩容：接入更多数据源，更多机器，对存储空间，机器进行扩容
4.  使用：这部分在功能这一节介绍
5.  导出：数据能否方便导出到其他系统，例如做流计算、放到对象存储中进行备份
6.  多租户：如何将数据分享给他人使用，使用是否安全等

以下是比较结果：

|项目|小项|自建ELK|Log Analytics|
|:-|:-|:----|:------------|
|采集|协议|Restful API|Restful API|
| |Agent|Logstash/Beats/FluentD，生态十分丰富|Logtail（为主）+其他（例如Logstash）|
|配置|单元|提供Index概念用以区分不同日志|Project + Logstore。提供两层概念，Project相当于命名空间，可以在Project下建立多个Logstore|
| |属性|API + Kibana|API + SDK + 控制台|
|扩容|存储|增加机器，购买云盘|无需操作|
| |机器|新增机器|无需操作|
| |配置|配置Logstash并通过配管系统应用机器|控制台/API 操作，无需配管系统|
| |采集点|通过配管系统控制，将配置和Logstash安装到机器组|控制台/API 操作，无需配管系统|
|导出|方式|API/SDK|API/SDK + 各流计算引擎（Spark，Storm，Flink，StreamCompute，CloudMonitor，ARMS） + 存储（OSS + MaxCompute）|
|多租户|安全|无|https + 传输签名 + 多租户隔离 + 访问控制|
| |使用|同一账号|子账号，角色，产品，临时授权等|

总体而言：

ELK 有非常多的生态和写入工具，使用中的安装、配置等都有较多工具可以参考。LogSearch/Analytics是一个托管服务，从接入、配置、使用上集成度非常高，普通用户5分钟就可以接入。并且过程中不需要担心容量、并发等问题，按量计费，弹性伸缩。

## 功能（查询+分析） {#section_mf4_wwy_zdb .section}

查询主要将符合条件的日志快速命中，分析功能是对数据进行统计与计算。

例如我们有如下需求：所有状态码大于200读请求，根据Ip统计次数和流量，这样的分析请求就可以转化为两个操作：查询到指定结果，对结果进行统计分析。在一些情况下我们也可以不进行查询，直接对所有日志进行分析。

```
1. Status in (200,500] and Method:Get*
 2. select count(1) as c, sum(inflow) as sum_inflow, ip group by Ip
```

## 查询能力对比 {#section_hkc_lqc_ry .section}

|类型|小项|自建ELK|Log-Search/Analytics|
|:-|:-|:----|:-------------------|
|文本|索引查询|支持|支持|
| |分词|支持|支持|
| |中文分词|支持|支持|
| |前缀|支持|支持|
| |后缀|支持| |
| |模糊|支持|支持|
| |Wildcast|支持|（计划支持）|
|数值|long|支持|支持|
| |double|支持|支持|
|Nested|Json查询|支持| |
|Geo|Geo查询|支持|不直接支持，但可以通过区间查询代替|
|Ip|Ip查询|支持|不直接支持，通过字符串支持|
|上下文|上下文查询| |支持|
| |上下文过滤| |支持|

ElasticSearch 支持的数据类型，以及查询方式上要更强。Log-Search/Analytics 支持大部分常用查询，也有一些特色（例如程序日志上下文查询与展开）。

## 分析能力对比 {#section_apk_cxy_zdb .section}

-   [ES 5.5 Aggregation](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations.html)
-   [实时分析简介](intl.zh-CN/用户指南/实时分析/实时分析简介.md)

|类型|小项|自建ELK|Log-Search/Analytics|
|:-|:-|:----|:-------------------|
|接口|方式|API/SDK|API/SDK + SQL92|
| |其他协议| |JDBC|
|Agg|Bucketing|支持|支持|
| |Metric|支持|支持|
| |Matrix|支持|支持|
| |Pipeline|有限支持|完整支持|
|运算|数值| |支持|
| |字符串| |支持|
| |估算| |支持|
| |数理统计| |支持|
| |日期转换| |支持|
|GroupBy|Agg|支持|支持|
| |Having条件| |支持|
|排序|排序| |支持|
|Join|多表Join| |支持|

从结果看Log-Search/Analytics 提供的功能是ES 超集，完整支持SQL 92 语义，只要会写SQL 都能直接使用。

## 性能 {#section_xcv_hxy_zdb .section}

接下来我们测试针对相同数据集，分别对比写入数据及查询，和聚合计算能力。

**实验环境**

1.  测试配置

    |类别|自建ELK|LogSearch/LogAnalytics|
    |:-|:----|:---------------------|
    |环境|ECS 4核16GB \* 4台 + 高效云盘 SSD|-|
    |Shard|10|10|
    |拷贝数|2|3 （默认配置，对用户不可见）|

2.  测试数据

    -   5列double
    -   5列long
    -   5列text，字典大小分别是256，512，768，1024，1280
    以上字段完全随机，如下为一条测试日志样例：

    ```
    timestamp:August 27th 2017, 21:50:19.000
     long_1:756,444 double_1:0 text_1:value_136
     long_2:-3,839,872,295 double_2:-11.13 text_2:value_475
     long_3:-73,775,372,011,896 double_3:-70,220.163 text_3:value_3
     long_4:173,468,492,344,196 double_4:35,123.978 text_4:value_124
     long_5:389,467,512,234,496 double_5:-20,10.312 text_5:value_1125
    ```

3.  数据集大小
    -   原始数据大小：50 GB
    -   原始数据除去Key后内容大小：27 GB（LogSearch/Analytics 以该大小作为存储计费单元）
    -   日志行数：162,640,232 （约为1.6亿条）

## 写入测试结果 {#section_uxt_rxy_zdb .section}

ES采用bulk api批量写入，LogSearch/Analytics 用PostLogstoreLogs API批量写入，结果如下：

|类型|项目|自建ELK|LogSearch/Analytics|
|:-|:-|:----|:------------------|
|延时|平均写入延时|40 ms|14 ms|
|存储|单拷贝数据量|86G|58G|
| |膨胀率：数据量/原始数据大小|172%|121%|

**说明：** LogSearch/Analytics 产生计费的存储量包括压缩的原始数据写入量（23G）+索引流量（27G），共50G存储费用。

从测试结果来看

-   LogSearch/Analytics写入延时好于ES，14 ms vs 40ms
-   空间：原始数据50G，因测试数据比较随机所以存储空间会有膨胀（大部分真实场景下，存储压缩后会比原始数据小）。ES胀到86G，膨胀率为172%，在存储空间超出LogSearch/Analytics 58%

## 读取（查询+分析）测试 {#section_zlh_vxy_zdb .section}

**测试场景**

我们在此选取两种比较常见的场景：日志查询和聚合计算。分别统计并发度为1，5，10时，两种case的平均延时。

1.  针对全量数据，对任意text列计算group by，计算5列数值的avg/min/max/sum/count，并按照count排序，取前1000个结果，例如：

    ```
    select count(long_1) as pv,sum(long_2),min(long_3),max(long_4),sum(long_5) 
    group by text_1 order by pv desc limit 1000
    ```

2.  针对全量数据，随机查询日志中的关键词，例如查询 “value\_126”，获取命中的日志数目与前100行，例如：

    ```
    value_126
    ```


**测试结果**

|类型|并发数|ES延时（单位s）|LogSearch/Analytics延时（单位s）|
|:-|:--|:--------|:-------------------------|
|case1：分析类|1|3.76|3.4|
| |5|3.9|4.7|
| |10|6.6|7.2|
|case2：查询类|1|0.097|0.086|
| |5|0.171|0.083|
| |10|0.2|0.082|

**结果分析**

-   从结果看，对于1.5亿数据量这个规模，两者都达到了秒级查询与分析能力。
-   针对统计类场景（case 1）， ES和日志服务延时处同一量级。ES采用SSD云盘，在读取大量数据时IO优势比较高。
-   针对查询类场景（case 2）， LogAnalytics在延时明显优于ES。随着并发的增加，ELK延时对应增加，而LogAnalytics延时保持稳定甚至略有下降。

## 规模 { .section}

1.  LogSearch/Analytics 一天可以索引PB级数据，一次查询可以在秒级过几十TB规模数据，在数据规模上可以做到弹性伸缩与水平扩展
2.  ES比较适合服务场景为：写入GB-TB/Day、存储在TB级。主要受限于2个原因：
    -   单集群规模：比较理想为20台左右，业界比较大的为100节点一个集群
    -   写入扩容：shard创建后便不可再修改，当吞吐率增加时，需要动态扩容节点，最多可使用的节点数便是shard的个数
    -   存储扩容：主shard达到磁盘的上线时，要么迁移到更大的一块磁盘上，要么只能分配更多的shard。一般做法是创建一个新的索引，指定更多shard，并且rebuild旧的数据

        LogSearch/Analytics 则没有扩展性方面的问题，每个shard都是分布式存储。并且当吞吐率增加时，可以动态分裂shard，达到处理能力水平扩展。


## 成本 {#section_t5s_rzy_zdb .section}

以上述测试数据为例，一天写入50GB数据（其中27GB 为实际的内容），保存90天，平均一个月的耗费。

1.  日志服务（LogSearch/LogAnalytics）计费规则参考[计费方式](../../../../intl.zh-CN/产品定价/计费方式.md)，包括读写流量、索引流量、存储空间等计费项，查询功能免费。

    |计费项目|值|单价|费用（元\)|
    |:---|:-|:-|:-----|
    |读写流量|23G \* 30|0.2 元/GB|138|
    |存储空间（保存90天）|50G \* 90|0.3 元/GB\*Month|1350|
    |索引流量|27G \* 30|0.35 元/GB|283|
    |总计|-|-|1771|

2.  ES费用包括机器费用，及存储数据SSD云盘费用

    -   云盘一般可以提供高可靠性，因此我们这里不计费副本存储量。
    -   存储盘一般需要预留15%剩余空间，以防空间写满，因此乘以一个1.15系数。
    |计费项目|值|单价|费用（元\)|
    |:---|:-|:-|:-----|
    |服务器|4台4核16G（三个月）（ecs.mn4.xlarge）|包年包月费用：675 元/Month|2021|
    |存储|86 \* 1.15 \* 90 （这里只计算一个副本）|SSD：1 元/GB\*M|8901|
    | |-|SATA：0.35 元/GB\*M|3115|
    |总计| | |12943 （SSD）|
    | | | |5135 （SATA）|


同样性能，使用LogSearch/Analytics与ELK（SSD）费用比为 13.6%。在测试过程中，我们也尝试把SSD换成SATA以节省费用（LogAnalytics与SATA版费用比为 34%），但测试发现延时会从40ms上升至150ms，在长时间读写下，查询和读写延时变得很高，无法正常工作了。

## 总结 { .section}

LogSearch/Analytics在提供同样查询速度、更高吞吐量、更强分析能力背后，只需要开源方案13%成本，并且按量付费免运维，让您把精力放在业务分析上。

日志服务除了LogSearch/Analytics外，还提供LogHub、LogShipper功能，支持实时采集数据、与流计算系统（Spark、Storm、Flink、StreamCompute）、离线分析系统（MaxCompute、E-MapReduce、Presto、Hive等）打通，提供一站式实时数据解决方案。

