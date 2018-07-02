# 通过DataWorks投递数据到MaxCompute {#concept_rjc_m4q_zdb .concept}

除了将日志投递到OSS存储之外，您还可以选择将日志数据通过DataWorks的数据集成（Data Integration）功能投递至MaxCompute。数据集成是阿里集团对外提供的稳定高效、弹性伸缩的数据同步平台，为阿里云大数据计算引擎（包括MaxCompute、AnalyticDB、OSPS）提供离线、批量的数据进出通道。

该功能支持的地域请参见[DataWorks文档](https://help.aliyun.com/document_detail/66089.html)。

## 应用场景 {#section_c3g_fnf_vdb .section}

-   跨Region的LogHub与MaxCompute等数据源的数据同步
-   不同阿里云账号下的LogHub与MaxCompute等数据源间的数据同步
-   同一阿里云账号下的LogHub与MaxCompute等数据源间的数据同步
-   公共云与金融云账号下的LogHub与MaxCompute等数据源间的数据同步

## 前提条件 {#section_cw1_gnf_vdb .section}

1.  已开通日志服务、MaxCompute和DataWorks。
2.  已使用日志服务成功采集到日志数据，LogHub中已存在可投递的日志数据。
3.  数据源账号需要启用一对访问密钥Access Key。
4.  跨账号投递的情况下，需要先配置RAM授权。

    详细配置请参考本文档中[跨账号授权](#section_nkr_5pf_vdb)部分。


## 操作步骤 {#section_jxb_hnf_vdb .section}

## 步骤1 创建数据源 {#section_nkh_hnf_vdb .section}

1.  在DataWorks管理控制台打开数据集成页面，单击左侧导航栏的数据源页签。
2.  在数据源页面右上角单击新增数据源，弹出新增数据源页面。
3.  单击**消息队列**中的**LogHub**，进入新增LogHub数据源页面。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13185/5817_zh-CN.png "新增数据源")

4.  填写数据源的配置项。

    配置项说明见下表。

    |配置项|说明|
    |:--|:-|
    |数据源名称|由英文字母、数字、下划线组成且须以字符或下划线开头，长度不超过60个字符。|
    |数据源描述|对数据源进行简单描述，长度不超过80个字符。|
    |LOG Endpoint|日志服务的Endpoint，根据您的Region确定，格式为：http://yyy.com 。详细说明请参考[服务入口](../../../../cn.zh-CN/API 参考/服务入口.md)。|
    |LOG Project|您想要投递至MaxCompute的日志服务Project。必须是已创建的Project。|
    |Access Id/Access Key|数据源账号的访问密钥AccessKey（AK）相当于登录密码。您可以填写数据源主账号的AK或是子账号的AK，成功配置后，当前账号具备访问数据源账号日志数据的权限，可以将数据源账号的日志通过正在创建的同步任务进行投递。|

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13185/5818_zh-CN.png "新增LogHub数据源")

5.  单击**测试连通性**。页面右上角弹出提示**测试连通性成功**后，单击**完成**。

## 步骤2 配置同步任务 {#section_bf5_5nf_vdb .section}

单击左侧导航的**同步任务**，并单击第二步**新建同步任务**，进入配置同步任务流程。

您可选择**向导模式**，通过简单便捷的可视化页面完成任务配置；或者选择**脚本模式**，深度自定义配置您的同步任务。

**向导模式**

共有选择来源、选择目标、字段映射、通道控制和预览保存五步操作。

1.  选择来源。

    **数据源**请选择您在步骤1中配置的数据源。请参考下表填写所有配置项。

    |配置项|说明|
    |:--|:-|
    |数据源|选择来源的LogHub数据源的名称。|
    |Logstore|导出增量数据的表的名称。该表需要开启Stream，可以在建表时开启，或者使用UpdateTable接口开启。|
    |日志开始时间|数据消费的开始时间位点，为时间范围（左闭右开）的左边界，为yyyyMMddHHmmss格式的时间字符串（比如20180111013000），可以和DataWorks的调度时间参数配合使用。|
    |日志结束时间|数据消费的结束时间位点，为时间范围（左闭右开）的右边界，为yyyyMMddHHmmss格式的时间字符串（比如20180111013010），可以和DataWorks的调度时间参数配合使用。|
    |批量条数|一次读取的数据条数，默认为256。|

    填写完成后请单击**数据预览**的下拉按钮，展开**数据预览**详情。查看是否已拉取到日志数据，并单击**下一步**。

    **说明：** 数据预览是根据LogHub里的全部的数据选择几条展现在预览框，可能您同步的数据会跟您的预览的结果不一样，因为您同步的数据会指定开始时间可结束时间。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13185/5819_zh-CN.png "选择来源")

2.  选择目标。

    1.  选择MaxCompute数据源及目标表。

        如您还没有创建MaxCompute表，可以单击右侧的**一键生成目标表**。在弹出菜单中新建数据表。

    2.  填写**分区信息**。

        分区配置支持正则，比如分区pt值配置为\*表示读取所有的pt分区数据。

    3.  选择**清理规则**。

        您可以选择在写入前清理已有数据，即覆盖式写数据，或者写入前保留已有数据，即插入式写数据。

    完成后单击**下一步**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13185/5820_zh-CN.png "选择目标")

3.  字段映射。

    选择字段的映射关系。需对字段映射关系进行配置，左侧源头表字段和右侧目标表字段默认为一一对应的关系，您也可以单击右侧的**同行映射**以选择或取消**同行映射**。

    **说明：** 如您需要将手动添加的日志字段作为同步列，请使用[脚本模式](cn.zh-CN/用户指南/数据投递/通过DataWorks投递数据到MaxCompute.md#section_gnq_k4f_vdb)配置。

    在**源头表字段列**一列的末尾单击**增加一行+**，可以配置日志服务中的元数据作为同步列，源头表字段填写`C_Topic`、`C_MachineUUID`、 `C_HostName`、`C_Path`、`C_LogTime`等，分别表示日志主题、采集机器唯一标识，主机名，路径，日志时间。

    **说明：** 

    -   可以输入常量，输入的值需要使用英文单引号包括，如’abc’、’123’等；
    -   可以配合调度参数使用，如 $\{bdp.system.bizdate\} 等；
    -   可以输入关系数据库支持的函数，如now\(\)、count\(1\)等；
    -   如果您输入的值无法解析，则类型显示为’未识别’。
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13185/5821_zh-CN.png "字段映射")

4.  通道控制。

    配置作业速率上限和脏数据检查规则，如下图所示：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13185/5822_zh-CN.png "通道控制")

    配置项说明：

    -   **DMU：**数据移动单位 \(DMU\) 是数据集成消耗资源（包含 CPU、内存、网络等资源分配）的度量单位。
    -   **作业并发数**：可将此属性视为数据同步任务内，可从源并行读取或并行写入数据存储端的最大线程数。
5.  预览保存。

    完成以上配置后，上下滚动鼠标可查看任务配置，如若无误，单击**保存**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13185/5823_zh-CN.png "预览保存")


## 脚本模式 {#section_gnq_k4f_vdb .section}

若您需要用脚本模式配置此任务，请参考如下脚本。

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
"endDateTime": "${endTime}",//数据消费的结束时间位点，为时间范围（左闭右开）的右边界
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

## 步骤3 运行任务 {#section_gg5_4pf_vdb .section}

您可通过以下两种方式运行任务。

-   直接运行（一次性运行）

    单击任务上方的**运行**按钮，将直接在数据集成页面运行任务。运行之前需要配置自定义参数的具体数值。

     ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13185/5824_zh-CN.png "运行任务配置") 

    如上图所示，显示的时间是同步10：10到17:30这段时间的LogHub记录到MaxCompute。

-   调度运行

    单击**提交**按钮，将同步任务提交到调度系统中，调度系统会按照配置属性在从第二天开始自动定时执行。相关调度的配置请参见[调度配置](https://help.aliyun.com/document_detail/50130.html)。

    设置5分钟调度一次，从00：00到23:59，startTime=$\[yyyymmddhh24miss-10/24/60\]系统前10分钟到 endTime=$\[yyyymmddhh24miss-5/24/60\]系统前5分钟时间。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13185/5825_zh-CN.png "调度配置")


## 跨账号授权 {#section_nkr_5pf_vdb .section}

配置跨账号的投递任务，需要通过访问控制RAM授权。

-   **跨账号授权**

    主账号之间的数据投递，可以直接在**新增LogHub数据源**步骤中，填入数据源主账号的AK。连通性测试通过后，表示授权成功。

    例如，A账号为日志数据源，将A账号收集的日志数据通过B账号开通的DataWorks服务投递至B账号的MaxCompute表中，需要B账号配置数据集成任务，并且在**新增LogHub数据源**步骤中，填入A账号的主账号AK。配置成功后，B账号有权限读取A账号下的所有日志数据。

-   **子账号授权**

    如果您不想暴露主账号的AK、或者需要投递子账号收集的日志数据，需要对子账号进行显式授权。

    -   **赋予子账号管理权限**

        如您需要通过子账号投递主账号名下的所有日志数据，需要按照以下步骤授权并配置AK。

        1.  主账号A为其名下的子账号A1赋予日志服务的管理权限，即`AliyunLogFullAccess`和`AliyunLogReadOnlyAccess`。详情请参见[RAM 用户](cn.zh-CN/用户指南/访问控制 RAM/RAM 用户.md)。
        2.  B账号配置数据集成任务，并在**新增LogHub数据源**步骤中，填入数据源子账号的AK。
        完成以上步骤后，B账号有权限读取A账号下的所有日志数据。

    -   **赋予子账号自定义权限**

        如您需要通过子账号投递主账号名下的部分日志数据，需要按照以下步骤授权并配置AK。

        1.  主账号A为其子账号A1配置自定义授权策略。相关的授权请参见[授权-简介](cn.zh-CN/用户指南/访问控制 RAM/授权-简介.md)和[概览](../../../../cn.zh-CN/API 参考/RAM子用户访问/概览.md)。
        2.  B账号配置数据集成任务，并在**新增LogHub数据源**步骤中，填入数据源子账号的AK。
        完成以上步骤后，B账号有权限读取A账号下的部分日志数据。

        **自定义授权策略示例**：

        此时，B账号通过子账号A1只能同步日志服务project\_name1以及project\_name2的数据。

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

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13185/5826_zh-CN.png "自定义授权策略")


