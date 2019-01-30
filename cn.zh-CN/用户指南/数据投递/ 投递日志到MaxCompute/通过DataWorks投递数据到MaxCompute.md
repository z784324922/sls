# 通过DataWorks投递数据到MaxCompute {#concept_rjc_m4q_zdb .concept}

本文将以LogHub数据同步至MaxCompute为例，为您介绍如何通过DataWorks数据集成同步LogHub数据至数据集成已支持的目的端数据源（如MaxCompute、OSS、OTS、RDBMS、DataHub等）。

除了将日志投递到OSS存储之外，您还可以选择将日志数据通过DataWorks的数据集成（Data Integration）功能投递至MaxCompute。数据集成是阿里集团对外提供的稳定高效、弹性伸缩的数据同步平台，为阿里云大数据计算引擎（包括MaxCompute、AnalyticDB）提供离线、批量的数据进出通道。

**说明：** 此功能已在华北2、华东2、华南1、香港、美西1、亚太东南1、欧洲中部1、亚太东南2、亚太东南3、亚太东北1、亚太南部1等多个Region发布上线。

## 应用场景 {#section_c3g_fnf_vdb .section}

-   跨Region的LogHub与MaxCompute等数据源的数据同步。
-   不同阿里云账号下的LogHub与MaxCompute等数据源间的数据同步。
-   同一阿里云账号下的LogHub与MaxCompute等数据源间的数据同步。
-   公共云与金融云账号下的LogHub与MaxCompute等数据源间的数据同步。

## 跨阿里云账号的特别说明 {#section_pvp_22g_4fb .section}

以B账号进入数据集成配置同步任务，将A账号的LogHub数据同步至B账号的MaxCompute为例。

1.  用A账号的AccessId和Accesskey建LogHub数据源。

    此时B账号可以拖A账号下所有sls project的数据。

2.  用A账号下子账号A1的AccessId和Accesskey创建LogHub数据源。
    -   A给A1赋权日志服务的通用权限，即`AliyunLogFullAccess`和`AliyunLogReadOnlyAccess`，详情请参见[访问日志服务资源](cn.zh-CN/用户指南/         访问控制 RAM/授权RAM 用户.md)。
    -   A给A1赋权日志服务的自定义权限。

        主账号A进入**RAM控制台** \> **策略管理**页面，选择**自定义授权策略** \> **新建授权** \> **空白模板**。

        相关的授权请参见[访问控制RAM](cn.zh-CN/用户指南/         访问控制 RAM/简介.md)和[RAM子用户访问](../cn.zh-CN/API 参考/RAM子用户访问/概览.md)。

        根据下述策略进行授权后，B账号通过子账号A1只能同步日志服务project\_name1以及project\_name2的数据。

        ```
        {
        "Version": "1",
        "Statement": [
        {
        "Action": [
        "log:Get*",
        "log:List*",
        "log:CreateConsumerGroup",
        "log:UpdateConsumerGroup",
        "log:DeleteConsumerGroup",
        "log:ListConsumerGroup",
        "log:ConsumerGroupUpdateCheckPoint",
        "log:ConsumerGroupHeartBeat",
        "log:GetConsumerGroupCheckPoint"
        ],
        "Resource": [
        "acs:log:*:*:project/project_name1",
        "acs:log:*:*:project/project_name1/*",
        "acs:log:*:*:project/project_name2",
        "acs:log:*:*:project/project_name2/*"
        ],
        "Effect": "Allow"
        }
        ]
        }
        ```


## 操作步骤 {#section_jxb_hnf_vdb .section}

## 步骤1 新增数据源 {#section_nkh_hnf_vdb .section}

1.  B账号或B的子账号以开发者身份登录[DataWorks控制台](https://workbench.data.aliyun.com/console)，单击对应项目下的**进入数据集成**。
2.  进入**同步资源管理** \> **数据源**页面，单击右上角的**新增数据源**。
3.  选择数据源类型为**LogHub**，填写新增LogHub数据源对话框中的配置。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16203/15488222817541_zh-CN.png)

    |配置|说明|
    |:-|:-|
    |**数据源名称**|数据源名称必须以字母、数字、下划线组合，且不能以数字和下划线开头。|
    |**数据源描述**|对数据源进行简单描述，不得超过80个字符。|
    |**LogHub Endpoint**|LogHub的Endpoint，格式为`http://yyy.com`。详细说明请参考[服务入口](../cn.zh-CN/API 参考/服务入口.md)。|
    |**Project**|您想要投递至MaxCompute的日志服务Project。必须是已创建的Project。|
    |**Access Id/Access Key**|即访问密钥，相当于登录密码。您可以填写主账号或子账号的Access Id和Access Key。|

4.  单击**测试连通性**。
5.  测试连通性通过后，单击**确定**。

## 步骤2 配置同步任务 {#section_bf5_5nf_vdb .section}

单击左侧导航的**同步任务**，并单击第二步**新建同步任务**，进入配置同步任务流程。

您可选择**向导模式**，通过简单便捷的可视化页面完成任务配置；或者选择**脚本模式**，深度自定义配置您的同步任务。

## 通过向导模式配置同步任务 {#section_sfx_dmm_ngb .section}

1.  进入**数据开发** \> **业务流程**页面，在左上角单击**新建数据同步节点**。
2.  填写新建数据同步节点对话框中的配置，单击**提交**，进入数据同步任务配置页面。
3.  选择数据来源。![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/24566/154882228114351_zh-CN.png)

    |配置|说明|
    |:-|:-|
    |**数据源**|填写LogHub数据源的名称。|
    |**Logstore**|导出增量数据的表的名称。该表需要开启Stream，可以在建表时开启，或者使用UpdateTable接口开启。|
    |**日志开始时间**|数据消费的开始时间位点，为时间范围（左闭右开）的左边界，为yyyyMMddHHmmss格式的时间字符串（比如20180111013000），可以和DataWorks的调度时间参数配合使用。|
    |**日志结束时间**|数据消费的结束时间位点，为时间范围（左闭右开）的右边界，为yyyyMMddHHmmss格式的时间字符串（比如20180111013010），可以和DataWorks的调度时间参数配合使用。|
    |**批量条数**|一次读取的数据条数，默认为256。|

    数据预览默认收起，您可单击进行预览。

    **说明：** 数据预览是选择LogHub中的几条数据展现在预览框，可能您同步的数据会跟您的预览的结果不一样，因为您同步的数据会指定开始时间可结束时间。

4.  选择数据去向。

    选择MaxCompute数据源及目标表ok。![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/24566/154882228214352_zh-CN.png)

    |配置|说明|
    |:-|:-|
    |**数据源**|填写配置的数据源名称。|
    |**表**|选择需要同步的表。|
    |**分区信息**|此处需同步的表是非分区表，所以无分区信息。|
    |**清理规则**|     -   **写入前清理已有数据**：导数据之前，清空表或者分区的所有数据，相当于insert overwrite。
    -   **写入前保留已有数据**：导数据之前不清理任何数据，每次运行数据都是追加进去的，相当于insert into。
 |
    |**压缩**|默认选择不压缩。|
    |**空字符串作为null**|默认选择否。|

5.  字段映射。

    选择字段的映射关系。需对字段映射关系进行配置，左侧源头表字段和右侧目标表字段为一一对应的关系。![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/24566/154882228214353_zh-CN.png)

6.  通道控制。

    配置作业速率上限和脏数据检查规则。![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/24566/154882228214354_zh-CN.png) 

    |配置|说明|
    |:-|:-|
    |**DMU**|数据集成的计费单位。**说明：** 设置DMU时，需注意DMU的值限制了最大并发数的值，请合理配置。

 |
    |**作业并发数**|配置时会结合读取端指定的切分建，将数据分成多个Task，多个Task同时运行，以达到提速的效果。|
    |**同步速率**|设置同步速率可保护读取端数据库，以避免抽取速度过大，给源库造成太大的压力。同步速率建议限流，结合源库的配置，请合理配置抽取速率。|
    |**错误记录数超过**|脏数据，类似源端是Varchar类型的数据，写到Int类型的目标列中，导致因为转换不合理而无法写入的数据。同步脏数据的设置，主要在于控制同步数据的质量问题。建议根据业务情况，合理配置脏数据条数。|
    |**任务资源组**|配置同步任务时，指定任务运行所在的资源组，默认运行在默认资源组上。当项目调度资源紧张时，也可以通过新增自定义资源组的方式来给调度资源进行扩容，然后将同步任务指定在自定义资源组上运行，新增自定义资源组的操作请参见[../DNide1894624/ZH-CN\_TP\_16266.md\#](../cn.zh-CN/使用指南/数据集成/常见配置/新增任务资源.md#)。您可根据数据源网络情况、项目调度资源情况和业务重要程度，进行合理配置。

|

7.  运行任务。

    您可通过以下两种方式运行任务。

    -   直接运行（一次性运行）

        单击任务上方的**运行**按钮，将直接在数据集成页面运行任务，运行之前需要配置自定义参数的具体数值。![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/24566/154882228214355_zh-CN.png)

        如上图所示，代表同步10:10到17:30这段时间的LogHub记录到MaxCompute。

    -   调度运行

        单击**提交**按钮，将同步任务提交到调度系统中，调度系统会按照配置属性在从第二天开始自动定时执行。![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/24566/154882228214356_zh-CN.png)

        如上图所示，设置按分钟调度，从00：00到23:59每5分钟调度一次。


## 通过脚本模式配置同步任务 {#section_hcl_h4g_4fb .section}

如果您需要通过脚本模式配置此任务，单击工具栏中的转换脚本，选择**确认**即可进入脚本模式。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/24565/154882228214347_zh-CN.png)

您可根据自身进行配置，示例脚本如下。

```
{
"type": "job",
"version": "1.0",
"configuration": {
"reader": {
"plugin": "loghub",
"parameter": {
"datasource": "loghub_lzz",//数据源名，保持跟您添加的数据源名一致
"logstore": "logstore-ut2",//目标日志库的名字，logstore是日志服务中日志数据的采集、存储和查询单元。
"beginDateTime": "${startTime}",//数据消费的开始时间位点，为时间范围（左闭右开）的左边界
"endDateTime": "${endTime}",//数据消费的开始时间位点，为时间范围（左闭右开）的右边界
"batchSize": 256,//一次读取的数据条数，默认为256。
"splitPk": "",
"column": [
"key1",
"key2",
"key3"
]
}
},
"writer": {
"plugin": "odps",
"parameter": {
"datasource": "odps_first",//数据源名，保持跟您添加的数据源名一致
"table": "ok",//目标表名
"truncate": true,
"partition": "",//分区信息
"column": [//目标列名
"key1",
"key2",
"key3"
]
}
},
"setting": {
"speed": {
"mbps": 8,/作业速率上限
"concurrent": 7//并发数
}
}
}
}
```

