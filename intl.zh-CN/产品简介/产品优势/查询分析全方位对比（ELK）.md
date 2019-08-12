# 查询分析全方位对比（ELK） {#concept_wh3_tmq_zdb .concept}

通过将阿里云日志服务与ELK Stack进行全面对比，帮助您更好的了解阿里云日志服务的主要功能和优势。

## 背景信息 {#section_jvw_rwy_zdb .section}

提到日志实时分析，很多人都会想到基于ELK Stack（Elastic/Logstash/Kibana）来搭建。ELK方案开源，在社区中有大量的内容和使用案例。

阿里云 [日志服务](https://www.alibabacloud.com/zh/product/log-service?spm) 是阿里巴巴集团对日志场景的解决方案产品，前身是2012年初阿里云在研发飞天操作系统过程中用来监控+问题诊断的产物，但随着用户增长与产品发展，慢慢开始向面向Ops（DevOps，Market Ops，SecOps）日志分析领域发展，期间经历双十一、蚂蚁双十二、新春红包、国际业务等场景挑战，成为同时服务国内外的产品。

## 面向日志分析场景 {#section_url_qkj_ngb .section}

Apache Lucene是Apache软件基金会一个开放源代码的全文检索引擎工具包，一个全文检索引擎的架构，提供了完整的查询引擎和索引引擎、部分文本分析引擎。2012年Elastic把Lucene基础库包成了一个更好用的软件，并且在2015年推出ELK Stack（Elastic Logstash Kibana）解决集中式日志采集、存储和查询问题。Lucene设计场景是Information Retrial，面对是Document类型，因此对于Log这种数据有一定限制，例如规模、查询能力、以及智能聚类LogReduce等定制化功能。

LOG提供日志存储引擎是阿里内部自研究技术，经过3年万级应用锤炼，每日索引数据量达PB级，服务万级开发者每天亿次查询分析。在阿里集团内阿里云全站，SQL审计、鹰眼、蚂蚁云图、飞猪Tracing、阿里云谛听等都选择LOG作为日志分析引擎。

而日志查询是DevOps最基础需求，业界的调研《[50 Most Frequently Used Unix Command](https://www.thegeekstuff.com/2010/11/50-linux-commands/)》也验证了这一点，tar排名第一、Grep这个命令排名第二，由此可见日志查询对程序员的重要性。

在日志查询分析场景，以如下点对ELK 与 LOG 做一个全方位比较：

-   易用：上手及使用过程中的代价。
-   功能（重点）：主要针对查询与分析。
-   性能（重点）：对于单位大小数据量查询与分析需求，延时如何。
-   规模（重点）：能够承担的数据量，扩展性等。
-   成本（重点）：同样功能和性能，使用分别花多少钱。

## 易用性 {#section_hqd_twy_zdb .section}

对日志分析系统而言，有如下使用过程：

-   采集：将数据稳定写入。
-   配置：如何配置数据源。
-   扩容：接入更多数据源，更多机器，对存储空间，机器进行扩容。
-   使用：这部分在功能这一节介绍。
-   导出：数据能否方便导出到其他系统，例如做流计算、放到对象存储中进行备份。
-   多租户：如何将数据分享给其他人使用，使用是否安全等。

以下是比较结果：

|项目|分项|自建ELK|Log Analytics|
|:-|:-|:----|:------------|
|采集|协议|Restful API| -   Restful API
-   JDBC

 |
|客户端|Logstash/Beats/FluentD，生态十分丰富| -   Logtail（为主）
-   其他（例如Logstash）

 |
|配置|单元|提供Index概念用以区分不同日志| -   Project
-   Logstore

 提供两层概念，Project相当于命名空间，可以在Project下建立多个Logstore。

 |
|属性|API + Kibana| -   API + SDK
-   控制台

 |
|扩容|存储| -   增加机器
-   购买云盘

 |无需操作|
|计算|新增机器|无需操作|
|配置| -   通过配管系统应用机器
-   （Logstash在Beta版本中已经提供中心化配置功能）

 |控制台/API 操作，无需配管系统|
|采集点|通过配管系统控制，将配置和Logstash安装到机器组|控制台/API 操作，无需配管系统|
|容量|不支持动态扩容|动态扩容、弹性伸缩|
|导出|方式| -   API
-   SDK

 | -   API
-   SDK
-   类Kafka接口消费
-   各流计算引擎消费（Spark，Storm，Flink）
-   流计算类库消费（Python、Java）

 |
|多租户|安全|商业版| -   https
-   传输签名
-   多租户隔离
-   访问控制

 |
|流控|无流控| -   Project级
-   Shard级

 |
|多租户|Kibana支持|原生提供账号与权限级管理|

整体而言：

-   ELK有非常多的生态和写入工具，安装、配置等都有较多工具可以使用。
-   LOG是托管服务，从接入、配置、使用上集成度非常高，普通用户5分钟就可以接入。
-   LOG是SaaS化服务，在过程中不需要担心容量、并发等问题，弹性伸缩，免运维。

## 功能（查询+分析） {#section_mf4_wwy_zdb .section}

查询主要是将符合条件的日志快速命中，分析功能是对数据进行统计与计算。

例如我们需要所有状态码大于200的读请求，根据IP统计次数和流量。这样的分析请求可以转化为两种操作：

-   查询到指定结果，对结果进行统计分析。
-   不进行查询，直接对所有日志进行分析。

``` {#codeblock_66b_wdd_51m}
1. Status in (200,500] and Method:Get*
2. select count(1) as c, sum(inflow) as sum_inflow, ip group by Ip
```

-   **查询基础对比** 

    该对比基于[Elastic 6.5 Indices](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices.html)。

    |类型|子类|ELK|LOG|
    |:-|:-|:--|:--|
    |文本|索引查询|支持|支持|
    |分词|支持|支持|
    |中文分词|支持|支持|
    |前缀|支持|支持|
    |后缀|支持|-|
    |模糊|支持|可通过SQL支持|
    |Wildcast|支持|可通过SQL支持|
    |数值|long|支持|支持|
    |double|支持|支持|
    |Nested|Json|支持|-|
    |Geo|Geo|支持|可通过SQL支持|
    |IP|IP查询|支持|可通过SQL支持|

    对比结论

    -   ES 支持的数据类型丰富度，原生查询能力比LOG更完整。
    -   LOG​ 能够通过SQL方式（如下）来代替字符串模糊查询，Geo等比较函数，但性能会比原生查询稍差。
    ``` {#codeblock_owt_v5m_9tg}
    子串命中
    * | select content where content like '%substring%' limit 100
    
    正则表达式匹配
    * | select content where regexp_like(content, '\d+m'）limit 100
    
    JSON内容解析与匹配
    * | select content where json_extract(content, '$.store.book')='mybook' limit 100
    
    如果设置json类型索引也可以使用：
    field.store.book='mybook'
    ```

-   **查询扩展能力** 

    在日志分析场景中，光有检索还不够，需要能够围绕查询做进一步的工作：

    -   定位到错误日志后，想通过上下文查看是什么参数引起了错误。
    -   定位到错误后，想看看之后有没有类似错误，类似tail -f 原始日志文件，并进行grep。
    -   通过关键词搜索到大量日志（例如百万条），其中90%都是已知问题干扰调查线索。
    LOG 针对以上问题提供闭环解决方案：

    -   上下文查询（Context Lookup）：原始上下文翻页，免登服务器。
    -   LiveTail功能（Tail-f）：原始上下文tail-f，更新实时情况。
    -   智能聚类（LogReduce）：根据日志Pattern动态归类，合并重复模式，洞察异常。
    1.  [LiveTail](../../../../intl.zh-CN/查询与分析/查询语法与功能/LiveTail.md)功能

        在传统的运维方式中，如果需要对日志文件进行实时监控，需要到服务器上对日志文件执行`tail -f`命令，如果实时监控的日志信息不够直观，可以加上`grep`或者`grep -v`进行关键词过滤。LOG在控制台提供了日志数据实时监控的交互功能LiveTail，针对线上日志进行实时监控分析，减轻运维压力。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13138/156560278437745_zh-CN.png)

        Livetail特点如下：

        -   智能支持Docker、K8S、服务器、Log4J Appender等来源数据。
        -   监控日志的实时信息，标记并过滤关键词。
        -   日志字段做分词处理，以便查询包含分词的上下文日志。
    2.  [智能聚类](../../../../intl.zh-CN/查询与分析/查询语法与功能/日志聚类.md)（LogReduce）

        业务的高速发展对系统稳定性提出了更高的要求，各个系统每天产生大量的日志，存在如下担忧：

        -   系统有潜在异常，但被淹没在海量日志中。
        -   机器被入侵，有异常登录，却后知后觉。
        -   新版本上线，系统行为有变化，却无法感知这些问题，是因为信息太多、太杂，不能良好归类，同时记录信息的日志，往往还都是无主题，格式多样，归类难度更大。LOG提供实时日志**智能聚类（LogReduce）**功能根据日志的相似性进行归类，快速掌握日志全貌：
            -   支持任意格式日志：Log4J、Json、单行（syslog）。
            -   日志经任意条件过滤后再Reduce；对日志Reduce后Pattern，根据signature反查原始数据。
            -   不同时间段Pattern比较。
            -   动态调整Reduce精度。
            -   亿级数据，秒级出结果。

## 分析能力对比 {#section_apk_cxy_zdb .section}

ES在docvalue之上提供一层聚合（Aggregation）语法，并且在6.x版本中提供SQL语法能够对数据进行分组聚合运算。 LOG支持完整SQL92标准（提供restful 和 jdbc两种协议），除基本聚合功能外，支持完整的SQL计算，并支持外部数据源联合查询（Join），机器学习，模式分析等函数。

**说明：** 以下分析基于[ES 6.5 Aggregation](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations.html)和[LOG分析语法](../../../../intl.zh-CN/查询与分析/实时分析简介.md#)。

除SQL92标准语法外，我们根据实际日志分析需求，研发一系列实用的功能：

-   [同比和环比函数](../../../../intl.zh-CN/查询与分析/SQL分析语法与功能/同比和环比函数.md) 

    同比环比函数能够通过SQL嵌套对任意计算（单值、多值、曲线）计算同环比（任意时段），以便洞察增长趋势。

    ``` {#codeblock_uoj_gsd_x49}
    * | select compare( pv , 86400) from (select count(1) as pv from log)
    ```

    ``` {#codeblock_rot_gj1_0qv}
    *|select t, diff[1] as current, diff[2] as yestoday, diff[3] as percentage from(select t, compare( pv , 86400) as diff from (select count(1) as pv, date_format(from_unixtime(__time__), '%H:%i') as t from log group by t) group by t order by t) s 
    ```

-   [外部数据源联合查询](../../../../intl.zh-CN/查询与分析/SQL分析语法与功能/Logstore和RDS联合查询.md)（Join）

    可以在查询分析中关联外部数据源

    -   支持Logstore，MySQL，OSS（CSV）等数据源。
    -   支持left，right，out，innerjoin。
    -   SQL查询外表，SQLJoin外表。
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13138/156560278437750_zh-CN.png)

    Join外表的样例：

    ``` {#codeblock_dgs_rls_lce}
    sql
    创建外表：
    * | create table user_meta ( userid bigint, nick varchar, gender varchar, province varchar, gender varchar,age bigint) with ( endpoint='oss-cn-hangzhou.aliyuncs.com',accessid='LTA288',accesskey ='EjsowA',bucket='testossconnector',objects=ARRAY['user.csv'],type='oss')
    
    使用外表
    * | select u.gender, count(1) from chiji_accesslog l join user_meta1 u on l.userid = u.userid group by u.gender
    ```

-   **地理位置函数** 

    针对IP地址、手机等内容，内置地理位置函数方便分析用户来源，包括：

    -   IP：国家、省市、城市、经纬度、运营商。
    -   Mobile：运营商、省市。
    -   GeoHash：Geo位置与坐标转换。
    查询结果分析样例：

    ``` {#codeblock_4hu_n4j_za8}
    sql
    * | SELECT count(1) as pv, ip_to_province(ip) as province WHERE ip_to_domain(ip) != 'intranet' GROUP BY province ORDER BY pv desc limit 10
    
    * | SELECT mobile_city(try_cast("mobile" as bigint)) as "城市", mobile_province(try_cast("mobile" as bigint)) as "省份", count(1) as "请求次数" group by "省份", "城市" order by "请求次数" desc limit 100
    ```

    -   [GeoHash](../../../../intl.zh-CN/查询与分析/SQL分析语法与功能/地理函数.md)
    -   [电话号码](../../../../intl.zh-CN/查询与分析/SQL分析语法与功能/电话号码函数.md)
    -   [IP函数](../../../../intl.zh-CN/查询与分析/SQL分析语法与功能/IP地理函数.md)
-   [安全分析函数](../../../../intl.zh-CN/查询与分析/SQL分析语法与功能/安全检测函数.md) 

    依托全球白帽子共享安全资产库，提供安全检测函数，您只需要将日志中任意的IP、域名或者URL传给安全检测函数，即可检测是否安全。

    -   security\_check\_ip
    -   security\_check\_domain
    -   security\_check\_url
-   [机器学习](../../../../intl.zh-CN/查询与分析/机器学习语法与函数/简介.md)与时序检测函数

    新增机器学习与智能诊断系列函数：

    -   根据历史自动学习其中规律，并对未来的走势做出预测。
    -   实时发现不易察觉的异常变化，并通过分析函数组合推理导致异常的特征。
    -   结合环比、告警功能智能发现/巡检。该功能适用在智能运维、安全、运营等领域，帮助更快、更有效、更智能洞察数据。
    提供的功能有：

    -   预测：根据历史数据拟合基线。
    -   异常检测\\变点检测\\折点检测：找到异常点。
    -   多周期检测：发现数据访问中的周期规律。
    -   时序聚类：找到形态不一样的时序。
-   **模式分析函数** 

    模式分析函数能够洞察数据中的特征与规律，帮助快速、准确推断问题：

    -   定位频繁集。例如：错误请求中90%由某个用户ID构成。
    -   定位两个集合中最大支持因素，例如：
        -   延时\>10S请求中某个ID构成比例远远大于其他维度组合。
        -   并且该ID在对比集合（B）中的比例较低。
        -   A和B中差异明显。
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13138/156560278537754_zh-CN.png)


## 性能 {#section_xcv_hxy_zdb .section}

针对相同数据集，分别对比写入数据及查询，和聚合计算能力。

-   **实验环境** 
    -   测试配置

        |类别|自建ELK|LOG|
        |:-|:----|:--|
        |环境|ECS 4核16GB \* 4台 + 高效云盘 SSD|-|
        |Shard|10|10|
        |拷贝数|2|3 （默认配置，对用户不可见）|

    -   测试数据

        -   5列double，5列long，5列text，字典大小分别是256，512，768，1024，1280
        -   以上字段完全随机（测试日志样例如下）。
        -   原始数据大小：50 GB。
        -   日志行数：162640232 （约为1.6亿条）。
        以上字段完全随机，如下为一条测试日志样例：

        ``` {#codeblock_pm7_4aw_l56}
        timestamp:August 27th 2017, 21:50:19.000 
        long_1:756,444 double_1:0 text_1:value_136 
        long_2:-3,839,872,295 double_2:-11.13 text_2:value_475 
        long_3:-73,775,372,011,896 double_3:-70,220.163 text_3:value_3 
        long_4:173,468,492,344,196 double_4:35,123.978 text_4:value_124 
        long_5:389,467,512,234,496 double_5:-20,10.312 text_5:value_1125
        ```

-   **写入测试结果**

    ES采用bulk api批量写入，LogSearch/Analytics 用PostLogstoreLogs API批量写入，结果如下：

    |类型|项目|自建ELK|LOG|
    |:-|:-|:----|:--|
    |延时|平均写入延时|40 ms|14 ms|
    |存储|单拷贝数据量|86G|58G|
    |膨胀率：数据量/原始数据大小|172%|116%|

    **说明：** 日志服务产生计费的存储量包括压缩的原始数据写入量（23G）和索引流量27G，共50G存储费用。

    从测试结果来看

    -   日志服务写入延时好于ES，40ms vs 14 ms。
    -   空间：原始数据50G，因测试数据比较随机所以存储空间会有膨胀（大部分真实场景下，存储因压缩会比原始数据小）。ES胀到86G，膨胀率为172%，在存储空间超出日志服务 58%。这个数据与ES推荐的存储大小为原始大小2.2倍比较接近。
-   **读取（查询+分析）测试** 
    -   **测试场景** 

        选取两种比较常见的场景：日志查询和聚合计算。分别统计并发度为1，5，10时，两种case的平均延时。

        -   针对全量数据，对任意text列计算group by，计算5列数值的avg/min/max/sum/count，并按照count排序，取前1000个结果，例如：

            ``` {#codeblock_6oq_rep_sr8}
            select count(long_1) as pv,sum(long_2),min(long_3),max(long_4),sum(long_5) 
              group by text_1 order by pv desc limit 1000
            ```

        -   针对全量数据，随机查询日志中的关键词，例如查询“value\_126”，获取命中的日志数目与前100行，例如：

            ``` {#codeblock_1bo_4sk_lub}
            value_126
            ```

    -   **测试结果** 

        |类型|并发数|ES延时（单位为秒）|日志服务延时（单位为秒）|
        |:-|:--|:---------|:-----------|
        |case1：分析类|1|3.76|3.4|
        |5|3.9|4.7|
        |10|6.6|7.2|
        |case2：查询类|1|0.097|0.086|
        |5|0.171|0.083|
        |10|0.2|0.082|

    -   **结果分析** 
        -   从结果看，对于1.6亿数据量这个规模，两者都达到了秒级查询与分析能力。
        -   针对统计类场景（case 1）， ES和日志服务延时处同一量级。ES采用SSD云盘，在读取大量数据时IO优势比较高。
        -   针对查询类场景（case 2）， LogAnalytics在延时明显优于ES。随着并发的增加，ELK延时对应增加，而LogAnalytics延时保持稳定甚至略有下降。

## 规模与成本 {#section_q08_sod_2xt .section}

-   **规模能力** 

    -   日志服务一天可以索引PB级数据，一次查询可以在秒级过几十TB规模数据，在数据规模上可以做到弹性伸缩与水平扩展。
    -   ES比较适合服务场景为：写入GB-TB/Day、存储在TB级。主要受限于2个原因：
        -   单集群规模：比较理想为20台左右，据了解业界比较大为100节点一个集群，为了应对业务往往拆成多个集群。
        -   写入扩容：shard创建后便不可再修改，当吞吐率增加时，需要动态扩容节点，最多可使用的节点数便是shard的个数。
        -   存储扩容：主shard达到磁盘的上限时，要么迁移到更大的一块磁盘上，要么只能分配更多的shard。一般做法是创建一个新的索引，指定更多shard，并且rebuild旧的数据。
    **用户案例（规模带来的问题）**

    客户A是国内最大资讯类网站之一，有数千台机器与百号开发人员。运维团队原先负责一套ELK集群用来处理Nginx日志，但始终处于无法大规模使用状态：

    -   一个大Query容易把集群打爆，导致其他用户无法使用。
    -   在业务高峰期间，采集与处理能力打满集群，造成数据丢失，查询结果不准确。
    -   业务增长到一定规模，因内存设置、心跳同步等节点经常内存失控导致OOM 不能保证可用性与准确性，开发最终没有使用起来，成为一个摆设。
    在2018年6月份，运维团队开始运行日志服务方案：

    1.  使用Logtail来采集线上日志，将采集配置，机器管理等通过API集成进客户自己运维与管控系统。
    2.  将日志服务查询页面嵌入统一登录与运维平台，进行业务与账户权限隔离。
    3.  通过控制台内嵌方案满足开发查询日志需求，通过Grafana插件调用日志服务统一业务监控，通过DataV连接日志服务进行大盘搭建。
    **说明：** 详细说明请参见文档：

    -   [控制台分享内嵌](../../../../intl.zh-CN/查询与分析/可视化分析/其他可视化方案/控制台分享内嵌.md)
    -   [对接Grafana](../../../../intl.zh-CN/查询与分析/可视化分析/其他可视化方案/对接Grafana.md)
    -   [对接DataV](../../../../intl.zh-CN/查询与分析/可视化分析/其他可视化方案/对接DataV.md)
    -   [对接Jaeger](../../../../intl.zh-CN/查询与分析/可视化分析/其他可视化方案/对接Jaeger.md)
    平台上线2个月后：

    -   每天查询的调用量大幅上升，开发逐步开始习惯在运维平台进行日志查询与分析，提升了研发的效率，运维部门也回收了线上登录的权限。
    -   除Nginx日志外，把App日志、移动端日志、容器日志也进行接入，规模是之前10倍。
    -   除查询日志外，也衍生出很多新的玩法，例如通过Jaeger插件与控制台基于日志搭建了Trace系统，将线上错误配置成每天的告警与报表进行巡检。
    -   通过统一日志接入管理，规范了各平台对接总线，不再有一份数据同时被采集多次的情况，大数据部门Spark、Flink等平台可以直接去订阅实时日志数据进行处理。
-   **成本** 

    以上述测试数据为例，一天写入50GB数据（其中23GB 为实际的内容），保存90天，平均一个月的耗费。

    -   日志服务（LogSearch/LogAnalytics）计费规则参见[按量付费](../../../../intl.zh-CN/产品定价/按量付费.md)，包括读写流量、索引流量、存储空间等计费项，查询功能免费。

        |计费项目|值|单价|费用（元）|
        |:---|:-|:-|:----|
        |读写流量|23G \* 30|0.2 元/GB|138|
        |存储空间（保存90天）|50G \* 90|0.3 元/GB\*Month|1350|
        |索引流量|27G \* 30|0.35 元/GB|283|
        |总计|-|-|1771|

    -   ES费用包括机器费用，及存储数据SSD云盘费用

        -   云盘一般可以提供高可靠性，因此我们这里不计费副本存储量。
        -   存储盘一般需要预留15%剩余空间，防止空间写满，因此乘以一个1.15系数。
        |计费项目|值|单价|费用（元）|
        |:---|:-|:-|:----|
        |服务器|4台4核16G（三个月）（ecs.mn4.xlarge）|包年包月费用：675 元/Month|2021|
        |存储|86 \* 1.15 \* 90 （这里只计算一个副本）|SSD：1 元/GB\*M|8901|
        |-|SATA：0.35 元/GB\*M|3115|
        |总计|12943 （SSD）|
        |5135 （SATA）|

        同样性能，使用LogSearch/Analytics与ELK（SSD）费用比为 13.6%。在测试过程中，我们也尝试把SSD换成SATA以节省费用（LogAnalytics与SATA版费用比为 34%），但测试发现延时会从40ms上升至150ms，在长时间读写下，查询和读写延时变得很高，无法正常工作了。

-   **时间成本（Time to Value）** 

    除硬件成本外，日志服务在新数据接入、搭建新业务、维护与资源扩容成本基本为0。

    -   支持各种日志处理生态，可以和Spark、Hadoop、Flink、Grafana等系统无缝对接。
    -   在全球化部署（有20+ Region），方便拓展全球化业务。
    -   提供30+日志接入SDK，与阿里云产品无缝打通集成。

## 总结 {#section_bx2_8x8_ahy .section}

ES支撑更新、查询、删除等更通用场景，在搜索、数据分析、应用开发等领域有广泛使用，ELK组合在日志分析场景上把ES灵活性与性能发挥到极致；日志服务是纯定位在日志类数据分析场景的服务，在该领域内做了很多定制化开发。一个服务更广，一个场景更具针对性。当然离开了场景纯数字的比较没有意义，找到适合自己场景的才重要。

