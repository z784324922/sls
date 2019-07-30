# 通过日志服务投递日志到MaxCompute {#concept_bll_l4q_zdb .concept}

投递日志到 MaxCompute 是日志服务的一个功能，能够帮助您最大化数据价值。您可以自己决定对某个日志库是否启用该功能。一旦启用该功能，日志服务后台会定时把写入到该日志库内的日志投递到 MaxCompute 对应的表格中。

## 使用限制 {#section_olr_xqd_5cb .section}

-   **数加控制台创建、修改投递配置必须由主账号完成，不支持子账号操作。**
-   不同Logstore的数据请勿导入到同一个MaxCompute表中，否则会造成分区冲突、丢失数据等后果。
-   投递MaxCompute是批量任务，请谨慎设置分区列及其类型：保证一个同步任务内处理的数据分区数小于512个；用作分区列的字段值不能为空或包括`/`等MaxCompute保留字段 。配置细节请参考下文投递配置说明。
-   不支持海外Region的MaxCompute投递，海外Region的MaxCompute请使用[Dataworks](../../../../cn.zh-CN/使用指南/数据集成/最佳实践/日志服务（Loghub）通过数据集成投递数据.md)进行数据同步。

    支持数据投递的国内Region如下：

    |日志服务Region|MaxCompute Region|
    |:---------|:----------------|
    |华北1|华东2|
    |华北2|华北2、华东2|
    |华北3|华东2|
    |华北5|华东2|
    |华东1|华东2|
    |华东2|华东2|
    |华南1|华南1、华东2|
    |香港|华东2|


## 功能优势 {#section_r15_5lp_yy .section}

日志服务收集的日志除了可以被实时查询外，还可以把日志数据投递到大数据计算服务MaxCompute（原ODPS），进一步进行个性化BI分析及数据挖掘。通过日志服务投递日志数据到MaxCompute具有如下优势：

-   使用便捷

    您只需要完成2步配置即可以把日志服务Logstore的日志数据迁移到MaxCompute中。

-   避免重复收集工作

    由于日志服务的日志收集过程已经完成不同机器上的日志集中化，无需重复在不同机器上收集一遍日志数据后再导入到MaxCompute。

-   充分复用日志服务内的日志分类管理工作

    用户可让日志服务中不同类型的日志（存在不同Logstore中）、不同Project的日志自动投递到不同的MaxCompute表格，方便管理及分析MaxCompute内的日志数据。


**说明：** 

一般情况下日志数据在写入Logstore后的1个小时导入到MaxCompute，您可以在控制台投递任务管理查看导入状态。导入成功后即可在MaxCompute内查看到相关日志数据。判断数据是否已完全投递请参考[文档](https://help.aliyun.com/document_detail/53708.html)。

结合日志服务的实时消费，投递日志数据到MaxCompute的数据通道以及日志索引功能，可以让用户按照不同的场景和需求、以不同的方式复用数据，充分发挥日志数据的价值。

## 配置流程 {#section_pwg_1mp_yy .section}

举例日志服务的一条日志如下：

``` {#codeblock_dcz_3p8_bu3}
16年01月27日20时50分13秒
10.10.*.*
ip:10.10.*.*
status:200
thread:414579208
time:27/Jan/2016:20:50:13 +0800
url:POST /PutData?Category=YunOsAccountOpLog&AccessKeyId=****************&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=******************************** HTTP/1.1
user-agent:aliyun-sdk-java
```

日志左侧的ip、status、thread、time、url、user-agent等是日志服务数据的字段名称，需要在下方配置中应用到。

1.  **初始化数加平台** 
    1.  在日志服务控制台的日志库页面选择需要投递的日志库名称并依次展开节点，**日志库名称** \> **数据处理** \> **导出** \> **MaxCompute\(原ODPS\)**，单击**开启投递**。

        自动跳转到初始化数加平台的页面。MaxCompute默认为按量付费模式，具体参见MaxCompute文档说明。

    2.  查看服务协议和条款后单击确定，初始化数加平台。

        初始化开通需10~20秒左右，请耐心等待。如果已经开通数加及大数据计算服务MaxCompute（原ODPS），将直接跳过该步骤。

2.  **数据模型映射** 

    在日志服务和大数据计算服务MaxCompute（原ODPS）之间同步数据，涉及两个服务的数据模型映射问题。您可以参考[日志服务日志数据结构](../../../../cn.zh-CN/产品简介/基本概念/日志.md)了解数据结构。

    将样例日志导入MaxCompute，分别定义MaxCompute数据列、分区列与日志服务字段的映射关系：

    |MaxCompute 列类型|MaxCompute 列名（可自定义）|MaxCompute 列类型（可自定义）|日志服务字段名（投递配置里填写）|日志服务字段类型|日志服务字段语义|
    |--------------|-------------------|--------------------|----------------|--------|--------|
    |数据列|log\_source|string|\_\_source\_\_|系统保留字段|日志来源的机器IP。|
    | |log\_time|bigint|\_\_time\_\_|系统保留字段|日志的Unix时间戳（是从1970年1月1日开始所经过的秒数），对应数据模型中的Time域。|
    | |log\_topic|string|\_\_topic\_\_|系统保留字段|日志主题。|
    | |time|string|time|日志内容字段|解析自日志，对应数据模型中的key-value，例如Logtail采集的数据在很多时候\_\_time\_\_与time取值相同。|
    | |ip|string|ip|日志内容字段|解析自日志。|
    | |thread|string|thread|日志内容字段|解析自日志。|
    | |log\_extract\_others|string|\_\_extract\_others\_\_|系统保留字段|未在配置中进行映射的其他日志内字段会通过key-value序列化到json，该json是一层结构，不支持字段内部 json嵌套。|
    |分区列|log\_partition\_time|string|\_\_partition\_time\_\_|系统保留字段|由日志的 \_\_time\_\_ 字段对齐计算而得，分区粒度可配置，在配置项部分详述。|
    | |status|string|status|日志内容字段|解析自日志，该字段取值应该是可以枚举的，保证分区数目不会超出上限。|

    -   MaxCompute表至少包含一个数据列、一个分区列。
    -   系统保留字段中建议使用 \_\_partition\_time\_\_，\_\_source\_\_，\_\_topic\_\_。
    -   MaxCompute单表有分区数目6万的限制，分区数超出后无法再写入数据，所以日志服务导入MaxCompute表至多支持3个分区列。请谨慎选择自定义字段作为分区列，保证其值是可枚举的。
    -   系统保留字段\_\_extract\_others\_\_历史上曾用名\_extract\_others\_，填写后者也是兼容的。
    -   MaxCompute分区列的值不支持”/“等特殊字符，这些是 MaxCompute 的保留字段。
    -   MaxCompute分区列取值不支持空，所以映射到分区列的字段必须出自保留字段或日志字段，且可以通过cast运算符将string类型字段值转换为对应分区列类型，空分区列的日志会在投递中被丢弃。
    -   日志服务数据的一个字段最多允许映射到一个MaxCompute表的列（数据列或分区列），不支持字段冗余，同一个字段名第二次使用时其投递的值为null，如果null出现在分区列会导致数据无法被投递。
3.  **配置投递规则** 
    1.  开启投递。

        初始化数加平台之后，根据页面提示进入LogHub —— 数据投递页面，选择需要投递的Logstore，并单击**确定**。

        您也可以在日志库页面选择需要投递的日志库名称并依次展开节点，**日志库名称** \> **数据处理** \> **导出** \> **MaxCompute\(原ODPS\)**，进入MaxCompute（原ODPS）投递管理页面。单击**开启投递**以进入LogHub —— 数据投递页面。

        ![](images/5814_zh-CN.png "开启投递")

    2.  配置投递规则。

        在LogHub —— 数据投递页面配置**字段关联**等相关内容。

        ![](images/5816_zh-CN.png "配置投递规则")

        配置项含义：

        |参数|语义|
        |:-|:-|
        |投递名称|自定义一个投递的名称，方便后续管理。|
        |MaxCompute Project|MaxCompute项目名称，该项默认为新创建的Project，如果已经是MaxCompute老客户，可以下拉选择已创建其他Project。|
        |MaxCompute Table|MaxCompute表名称，请输入自定义的新建的MaxCompute表名称或者选择已有的MaxCompute表。|
        |MaxCompute 普通列|按序，左边填写与MaxCompute表数据列相映射的日志服务字段名称，右边填写或选择MaxCompute表的普通字段名称及字段类型。|
        |MaxCompute 分区列|按序，左边填写与MaxCompute表分区列相映射的日志服务字段名称，右边填写或选择MaxCompute表的普通字段名称及字段类型。|
        |分区时间格式|\_\_partition\_time\_\_输出的日期格式，参考 [Java SimpleDateFormat](https://docs.oracle.com/javase/6/docs/api/java/text/SimpleDateFormat.html?spm=5176.doc29001.2.4.vP4zF4)。|
        |导入MaxCompute间隔|MaxCompute数据投递间隔，默认1800，单位：秒。|

        -   该步会默认为客户创建好新的MaxCompute Project和Table，其中如果已经是MaxCompute老客户，可以下拉选择其他已创建Project。
        -   日志服务投递MaxCompute功能按照字段与列的顺序进行映射，修改MaxCompute表列名不影响数据导入，如更改MaxCompute表schema，请重新配置字段与列映射关系。

## 参考信息 {#section_o2c_cmf_vdb .section}

**\_\_partition\_time\_\_ 格式**

将日志时间作为分区字段，通过日期来筛选数据是MaxCompute常见的过滤数据方法。

`__partition_time__`是根据日志\_\_time\_\_值计算得到（不是日志写入服务端时间，也不是日志投递时间），结合分区时间格式，向下取整（为避免触发MaxCompute单表分区数目的限制，日期分区列的值会按照导入MaxCompute间隔对齐）计算出日期作为分区列。

举例来说，日志提取的time字段是“27/Jan/2016:20:50:13 +0800”，日志服务据此计算出保留字段\_\_time\_\_为1453899013（Unix时间戳），不同配置下的时间分区列取值如下：

|导入MaxCompute间隔|分区时间格式|\_\_partition\_time\_\_|
|:-------------|:-----|:----------------------|
|1800|yyyy\_MM\_dd\_HH\_mm\_00|2016\_01\_27\_20\_30\_00|
|1800|yyyy-MM-dd HH:mm|2016-01-27 20:30|
|1800|yyyyMMdd|20160127|
|3600|yyyyMMddHHmm|201601272000|
|3600|yyyy\_MM\_dd\_HH|2016\_01\_27\_20|

-   请勿使用精确到秒的日期格式：1. 很容易导致单表的分区数目超过限制（6万）；2. 单次投递任务的数据分区数目必须在512以内。
-   以上分区时间格式是测试通过的样例，您也可以参考[Java SimpleDateFormat](https://docs.oracle.com/javase/6/docs/api/java/text/SimpleDateFormat.html?spm=5176.doc29001.2.4.vP4zF4)自己定义日期格式，但是该格式不得包含斜线字符”/“（这是MaxCompute的保留字段）。

**\_\_partition\_time\_\_ 使用方法**

使用MaxCompute的字符串比较筛选数据，可以避免全表扫描。比如查询2016年1月26日一天内日志数据：

``` {#codeblock_uas_kyg_691}
select * from {ODPS_TABLE_NAME} where log_partition_time >= "2015_01_26" and log_partition_time < "2016_01_27";
```

**\_\_extract\_others\_\_使用方法**

log\_extract\_others为一个json字符串，如果想获取该字段的user-agent内容，可以进行如下查询：

``` {#codeblock_6j4_e66_27y}
select get_json_object(sls_extract_others, "$.user-agent") from {ODPS_TABLE_NAME} limit 10;
```

**说明：** 

-   get\_json\_object是[MaxCompute提供的标准UDF](http://docs.aliyun.com/?spm=a2c4g.11186623.2.11.lo7w0K#/odps/SQL/udf&summary)。请联系MaxCompute团队开通使用该标准UDF的权限。
-   示例供参考，请以MaxCompute产品建议为最终标准。

## 其他操作 {#section_blf_2pp_yy .section}

**编辑投递配置**

在投递管理页面，单击**修改**即可针对之前的配置信息进行编辑。其中如果想新增列，可以在大数据计算服务MaxCompute（原ODPS）修改投递的数据表列信息，则单击**修改**后会加载最新的数据表信息。

**投递任务管理**

在启动投递功能后，日志服务后台会定期启动离线投递任务。用户可以在控制台上看到这些投递任务的状态和错误信息。具体请参考[管理日志投递任务](cn.zh-CN/用户指南/数据投递/ 管理日志投递任务.md)。

如果投递任务出现错误，控制台上会显示相应的错误信息：

|错误信息|建议方案|
|:---|:---|
|MaxCompute项目空间不存在|在MaxCompute控制台中确认配置的MaxCompute项目是否存在，如果不存在则需要重新创建或配置。|
|MaxCompute表不存在|在MaxCompute控制台中确认配置的MaxCompute表是否存在，如果不存在则需要重新创建或配置。|
|MaxCompute项目空间或表没有向日志服务授权|在MaxCompute控制台中确认授权给日志服务账号的权限是否还存在，如果不存在则需要重新添加上相应权限。|
|MaxCompute错误|显示投递任务收到的MaxCompute错误，请参考MaxCompute相关文档或联系MaxCompute团队解决。日志服务会自动重试最近两天时间的失败任务。|
|日志服务导入字段配置无法匹配MaxCompute表的列|重新配置MaxCompute表格的列与日志服务数据字段的映射配置。|

当投递任务发生错误时，请查看错误信息，问题解决后可以通过云控制台中日志投递任务管理或SDK来重试失败任务。

## MaxCompute中消费日志 {#section_q4k_3pp_yy .section}

MaxCompute用户表中示例数据如下：

``` {#codeblock_ccd_wr5_0xf}
| log_source | log_time | log_topic | time | ip | thread | log_extract_others | log_partition_time | status |
+------------+------------+-----------+-----------+-----------+-----------+------------------+--------------------+-----------+
| 10.10.*.* | 1453899013 | | 27/Jan/2016:20:50:13 +0800 | 10.10.*.* | 414579208 | {"url":"POST /PutData?Category=YunOsAccountOpLog&AccessKeyId=****************&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=******************************** HTTP/1.1","user-agent":"aliyun-sdk-java"} | 2016_01_27_20_50 | 200 |
+------------+------------+-----------+-----------+-----------+-----------+------------------+--------------------+-----------+
```

同时，我们推荐您直接使用已经与MaxCompute绑定的大数据开发Data IDE来进行可视化的BI分析及数据挖掘，这将提高数据加工的效率。

## 授予MaxCompute数据投递权限 {#section_n22_1nf_vdb .section}

如果在数加平台执行表删除重建动作，会导致默认授权失效。请手动重新为日志服务投递数据授权。

在MaxCompute项目空间下添加用户：

``` {#codeblock_d31_oud_gkw}
ADD USER aliyun$shennong_open@aliyun.com;
```

shennong\_open@aliyun.com 是日志服务系统账号（请不要用自己的账号），授权目的是为了能将数据写入到MaxCompute

MaxCompute项目空间Read/List权限授予：

``` {#codeblock_cb7_mez_qo9}
GRANT Read, List ON PROJECT {ODPS_PROJECT_NAME} TO USER aliyun$shennong_open@aliyun.com;
```

MaxCompute项目空间的表Describe/Alter/Update权限授予：

``` {#codeblock_ndx_sdq_ph4}
GRANT Describe, Alter, Update ON TABLE {ODPS_TABLE_NAME} TO USER aliyun$shennong_open@aliyun.com;
```

确认MaxCompute授权是否成功：

``` {#codeblock_7jj_nzo_v9m}
SHOW GRANTS FOR aliyun$shennong_open@aliyun.com;

A       projects/{ODPS_PROJECT_NAME}: List | Read
A       projects/{ODPS_PROJECT_NAME}/tables/{ODPS_TABLE_NAME}: Describe | Alter | Update
```

