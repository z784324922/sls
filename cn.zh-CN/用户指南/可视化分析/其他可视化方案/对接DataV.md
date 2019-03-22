# 对接DataV {#concept_mh2_rnq_zdb .concept}

提到双十一人人都会想到天猫霸气的实时大屏。说起实时大屏，都会想到最典型的流式计算架构：

-   数据采集：将来自各源头数据实时采集。
-   中间存储：利用类Kafka Queue进行生产系统和消费系统解耦。
-   实时计算：环节中最重要环节，订阅实时数据，通过计算规则对窗口中数据进行运算。
-   结果存储：计算结果数据存入SQL和NoSQL。
-   可视化：通过API调用结果数据进行展示。

在阿里集团内，有大量成熟的产品可以完成此类工作，一般可供选型的产品如下：

![](images/5861_zh-CN.png "大屏展示相关产品")

除这种方案外，日志服务还可以为您一种新的方法：通过日志服务（LOG，原SLS）查询分析LogSearch/Analytics API 直接对接DataV进行大屏数据展示。

![](images/5862_zh-CN.png "日志服务+DataV")

2017年9月志服务\(原SLS\)加强日志实时分析功能，可以使用查询+SQL92语法对日志进行实时分析。在结果分析可视化上，除了使用自带Dashboard外，还支持Grafana、Tableua（JDBC）等对接方式。

## 功能特点 {#section_ls5_t2q_12b .section}

计算一般根据数据量、实时性和业务需求会分为以下两种方式：

-   实时计算（流计算）：固定的计算 + 变化的数据
-   离线计算（数据仓库+离线计算）：变化的计算+固定的数据

日志服务（原SLS）对实时采集数据提供两种方式对接，除此之外，对实时性有要求的日志分析场景，我们提供实时索引LogHub中数据机制，之后可通过LogSearch/Anlaytics直接进行查询分析。这种方式具有以下优势：

-   快速：API传入Query立马拿到结果，无需等待和预计算结果。
-   实时：99.9%情况下可做到日志产生1秒内反馈到大屏。
-   动态：无论修改统计方法还是补数据，支持实时刷新显示结果，无需等待重新计算。

没有一个计算系统是万能的，这种方式具有以下限制：

-   数据量：单次计算数据量限制为百亿G，一旦超过，需要限定时间段。
-   计算灵活度：目前计算限于SQL92语法，不支持自定义UDF。

![](images/5863_zh-CN.png "日志服务优势")

## 配置流程 {#section_awp_w2q_12b .section}

操作演示：

日志服务数据对接DataV大屏展示，其操作主要分为以下3个流程：

1.  数据采集。请参考[文档](../../../../../intl.zh-CN/快速入门/五分钟快速入门.md#)配置，将数据源接入日志服务。
2.  索引设置。请参考[简介](intl.zh-CN/用户指南/查询与分析/简介.md#)，或最佳实践中[网站日志分析案例](../../../../../intl.zh-CN/快速入门/采集并分析Nginx访问日志.md#)。
3.  对接DataV插件，将实时查询SQL转化为视图。

完成1、2步骤后，在查询页面可以看到原始日志，本文档主要演示步骤3。

## 配置步骤 {#section_sxv_y2q_12b .section}

****

## 步骤1 创建DataV数据源 {#section_ipp_z2q_12b .section}

在**我的数据**页签中单击**添加数据**，并填写数据源的基本信息。各个配置项含义请参考下表。

![](images/5865_zh-CN.png "新建数据")

|配置项|说明|
|:--|:-|
|类型|选择**简单日志服务-SLS**。|
|名称|为数据源配置一个自定义名称。|
|AK ID|主账号的Acess Key ID，或者有权限读取日志服务的子账号Acess Key ID。|
|AK Secret|主账号的Acess Key Secret，或者有权限读取日志服务的子账号Acess Key Secret。|
|Endpoint|日志服务的Project所在region的地址。示例图中为杭州region地址。|

## 步骤2 创建折线图 {#section_ynp_z2q_12b .section}

1.  创建一个折线图。

    在折线图的数据配置中，数据源类型选择**简单日志服务-SLS**，然后选择上一步中创建的数据源**log\_service\_api**，并在**查询**框中输入参数。

    ![](images/5866_zh-CN.png "数据源")

    查询参数样例如下，参数说明见下表。

    ```
    {
     "projectName": "dashboard-demo",
     "logStoreName": "access-log",
     "topic": "",
     "from": ":from",
     "to": ":to",
     "query": "*| select approx_distinct(remote_addr) as uv ,count(1) as pv , date_format(from_unixtime(date_trunc('hour',__time__) ) ,'%Y/%m/%d %H:%i:%s')   as time group by time  order by time limit 1000" ,
     "line": 100,
     "offset": 0
    }
    ```

    |配置项|说明|
    |:--|:-|
    |projectName|您的Project名称。|
    |logstoreName|您的Logstore名称。|
    |topic|您的日志Topic，如您没有设置Topic，此处请留空。|
    |from、to|from和to分别是日志的起始和结束时间。**说明：** 示例中填写的是`:from`和`:to`。 在测试时，您可以先填写unix time，例如1509897600。发布之后换成`:from`和`:to`，然后您可以在url参数里控制这两个数值的具体时间范围。例如，预览是的url是`http://datav.aliyun.com/screen/86312`，打开`http://datav.aliyun.com/screen/86312?from=1510796077&to=1510798877`后，会按照指定的时间进行计算。

|
    |query|您的查询条件，样例中为每分钟的pv数。query的语法请参考[实时分析简介](intl.zh-CN/用户指南/查询与分析/实时分析简介.md)。**说明：** query中的时间格式，一定要是2017/07/11 12:00:00这种，所以采用以下方式把时间对齐到整点，再转化成目标格式。

    ```
date_format(from_unixtime(date_trunc('hour',__time__) ) ,'%Y/%m/%d
                    %H:%i:%s')
    ```

|
    |line|请填写默认值100。|
    |offset|请填写默认值0。|

    配置完成后，单击**查看数据响应结果**：

    ![](images/5867_zh-CN.png "数据相应结果")

2.  新建过滤器。

    勾选**使用过滤器**，然后新建一个过滤器。

    过滤器内容请按照以下格式填写：

    ```
    return Object.keys(data).map((key) => {
    let d= data[key];
    d["pv"] = parseInt(d["pv"]);
    return d;
    }
    )
    ```

    在过滤器中，要把y轴用到的结果变成int类型，上述样例中，y轴是pv，所以需要转换pv列。

    可以看到在结果中有t和pv两列，您可以将x轴配置为t，y轴配置成pv。


## 步骤3 配置饼状图 {#section_sx2_sfq_12b .section}

1.  新建**轮播饼图**。

    ![](images/5869_zh-CN.png "轮播饼图")

    查询框中请按照如下内容填写：

    ```
    {
     "projectName": "dashboard-demo",
     "logStoreName": "access-log",
     "topic": "",
     "from": 1509897600,
     "to": 1509984000,
     "query": "*| select count(1) as pv ,method group by method" ,
     "line": 100,
     "offset": 0
    }
    ```

    在查询中，可以计算不同method的占比。

2.  添加一个过滤器，过滤器填写：

    ```
    return Object.keys(data).map((key) => {
    let d= data[key];
    d["pv"] = parseInt(d["pv"]);
    return d;
    }
    )
    ```

    饼图的type填写method，value填写pv。


## 步骤4 预览和发布 {#section_yyj_vfq_12b .section}

单击**预览和发布**，一个大屏就创建成功了。开发者和业务同学可以在双十一当天实时看到自己的业务访问情况。

## 实际案例：不断调整统计口径下实时大屏 {#section_vd4_yfq_12b .section}

例如，云栖大会期间有个临时需求，统计线上（网站）的全国各地访问量。此前已配置采集全量日志数据、并且在日志服务中打开了查询分析，所以只要输入查询分析Query即可。

1.  以统计UV为例，对所有访问日志中nginx下forward字段获取10月11日到目前唯一计数。

    ```
    * | select approx_distinct(forward) as uv
    ```

2.  线上运行1天后，需求变更了。目前只需要统计yunqi这个域名下的数据。可以增加一个过滤条件（host），实时查询。

    ```
    host:yunqi.aliyun.com | select approx_distinct(forward) as uv
    ```

3.  后来发现Nginx访问日志中有多个IP情况，默认情况下只要第一个IP即可，在查询中对Query进行处理。

    ```
    host:yunqi.aliyun.com | select approx_distinct(split_part(forward,',',1)) as uv
    ```

4.  第三天接到需求，针对访问计算中需要把uc中广告访问去掉。此时可以加上一个过滤条件（not …），马上算到最新结果。

    ```
    host:yunqi.aliyun.com not url:uc-iflow  | select approx_distinct(split_part(forward,',',1)) as uv
    ```

     


