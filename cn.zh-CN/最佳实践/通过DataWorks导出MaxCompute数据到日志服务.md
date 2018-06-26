# 通过DataWorks导出MaxCompute数据到日志服务 {#concept_fn1_4yw_f2b .concept}

## 应用场景 {#section_qzz_syw_f2b .section}

DataWorks是阿里云提供的数据中转服务，日志服务采集到的日志数据可以通过DataWorks投递至MaxCompute存储与分析。MaxCompute主要用于离线计算，如果您需要OLAP实时分析，您可以将投递到MaxCompute的日志数据及后续的计算结果通过DataWorks导出到日志服务，并对导出的数据进行实时查询与分析。

## 实现原理 {#section_ryt_yyw_f2b .section}

LogHub Writer通过DataWorks框架获取Reader生成的数据，然后将DataWorks支持的类型通过逐一判断并转换成String类型。当达到用户指定的`batchSize`时，会使用 SLS Java SDK 一次性将推送至日志服务。 默认情况下，一次推送1024条数据。`batchSize`值最大为4096。

## 前提条件 {#section_ndd_43x_f2b .section}

1.  您已开通日志服务，并创建了Project和Logstore。
2.  已开通MaxCompute，并创建了表。
3.  开通DataWorks。

## 操作步骤 {#section_lnn_bzw_f2b .section}

1.  登录DataWorks控制台，创建Loghub数据源。

    创建步骤请参考[步骤1 创建数据源](../../../../cn.zh-CN/用户指南/数据投递/通过DataWorks投递数据到MaxCompute.md#section_nkh_hnf_vdb)。

2.  创建脚本模式的同步任务。
    1.  单击左侧导航的**同步任务**，并单击**脚本模式**，进入配置同步任务流程。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15131/6581_zh-CN.png "脚本模式")

    2.  填写导入模板。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15131/6582_zh-CN.png "导入模板")

        |配置项|说明|
        |:--|:-|
        |来源类型|您的数据源的类型，此处请选择`ODPS`。|
        |数据源|您的来源数据源名称。您可以可以单击**新增数据源**，新建一个数据源。|
        |目标类型|您的投递目标类型，此处请选择`LogHub`。|
        |数据源|您的目标数据源名称。请选择以上步骤中创建的LogHub数据源，您可以单击**新增数据源**，新建一个目标数据源。|

        填写完毕后单击**确认**，开始配置同步任务。

    3.  输入您的配置。

        示例如下：

        ```
        {
          "type": "job",
          "version": "1.0",
          "configuration": {
            "setting": {
              "errorLimit": {
                "record": "0"
              },
              "speed": {
                "mbps": "1",
                "concurrent": 1,
                "dmu": 1,
                "throttle": false
              }
            },
            "reader": {
              "plugin": "odps",
              "parameter": {
              		 "accessKey":"*****",
                   "accessId":"*****",
                   "column":["*"],
                   "isCompress":"false",
                   "partition":["pt=20161226"],
                   "project":"aliyun_account",
                   "table":"ak_biz_log_detail"
              }
            },
            "writer": {
              "plugin": "loghub",
              "parameter": {
                "endpoint": "",
                "accessId": "",
                "accessKey": "",
                "project": "",
                "logstore": "",
                "batchSize": "1024",
                "topic": "",
                "time" :"time_str",
                "timeFormat":"%Y_%m_%d %H:%i:%S",
                "column": [
                  "col0",
                  "col1",
                  "col2",
                  "col3",
                  "col4",
                  "col5"
                ],
                "datasource": "sls"
              }
            }
          }
        }
        ```

        |参数|是否必选|说明|
        |:-|:---|:-|
        |endpoint|必选|日志服务的Endpoint，请参考[服务入口](../../../../cn.zh-CN/API 参考/服务入口.md)。|
        |accessKeyId|必选|主账号或子账号的AccessKeyId。|
        |accessKeySecret|必选|主账号或子账号的AccessKeySecret。|
        |project|必选|目标日志服务Project的名称。|
        |logstore|必选|目标日志服务Logstore的名称。|
        |topic|可选|指定MaxCompute的某个字段为日志服务中的topic字段，默认值为空字符串。|
        |batchSize|可选|一次推送发送的数据条数，默认为1024。|
        |column|必选|每条数据中column的名字。**说明：** 当column的个数与数据源不匹配时，会认为是脏数据。

|
        |time|可选|time字段的名称。**说明：** 如果没有time字段，默认采用系统时间作为日志时间。

|
        |timeFormat|如果有time配置，则timeFormat必填|time字段的格式，可以配置为：        -   bigint，表示unix时间戳。
        -   时间戳格式。即从字符串类型中提取时间。例如`%Y_%m_%d %H:%M:%S`。
如果time字段是bigint类型的1529382552，那么`timeFormat`为`bigint`。如果time字段是字符串类型的`2018_06_19 12:30:25`，那么`timeFormat`为`%Y_%m_%d %H:%M:%S`

|
        |datasource|必选|DataWorks中定义的数据类型。|

3.  保存并执行任务。

    单击**保存**后选择保存路径。您可以直接执行同步任务，或者提交到调度系统。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15131/6583_zh-CN.png "运行同步任务")

    -   直接运行。

        单击**运行**，直接开始一次数据同步任务。

    -   调度运行。

        单击**提交**，将同步任务提交到调度系统，调度系统会按照配置属性自动定时执行。

        **说明：** 推荐根据分区（Partition）生成周期进行调度。 例如一个小时的数据生成一个分区，则推荐调度周期为一小时。

        关于调度运行的更多信息请参考[配置调度](../../../../cn.zh-CN/用户指南/数据投递/通过DataWorks投递数据到MaxCompute.md#ul_ex4_ppf_vdb)。


## 数据类型 {#section_znq_rhx_f2b .section}

MaxCompute数据通过DataWorks成功导入到日志服务后，所有数据类型都会被转换为为String类型。如下表所示。

|MaxCompute数据类型|导入LogHub后的数据类型|
|:-------------|:-------------|
|Long|String|
|Double|String|
|String|String|
|Data|String|
|Boolean|String|
|Bytes|String|

