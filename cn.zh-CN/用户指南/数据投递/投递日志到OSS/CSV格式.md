# CSV格式 {#concept_hch_k4q_zdb .concept}

本文介绍日志服务投递OSS使用CSV存储的相关细节，其它内容请参考[投递日志到OSS](intl.zh-CN/用户指南/数据投递/投递日志到OSS.md)。

## CSV存储字段配置 {#section_vhb_f5j_5cb .section}

**配置页面**

您可以在日志服务数据预览或索引查询页面查看一条日志的多个Key-Value，将你需要投递到OSS的字段名（Key）有序填入。

如您配置的Key名称在日志中找不到，CSV行中这里一列值将设置为空值字符串（null）。

![](images/5813_zh-CN.png "配置项")

**配置项**

|配置项|取值|备注|
|:--|:-|:-|
|分隔符 delimiter|字符|长度为1的字符串，用于分割不同字段。|
|转义符 quote|字符|长度为1的字符串，字段内出现分隔符（delimiter）或换行符等情况时，需要用quote前后包裹这个字段，避免读数据时造成字段错误切分。|
|跳出符 escape|字符|长度为1的字符串，默认设置与quote相同，暂不支持修改。字段内部出现quote（当成正常字符而不是转义符）时需要在quote前面加上escape做转义。|
|无效字段内容 null|字符串|当指定Key值不存在时，字段填写该字符串表示该字段无值。|
|投递字段名称 header|布尔|是否在csv文件的首行加上字段名的描述。|

更多内容请参考[CSV标准](https://tools.ietf.org/html/rfc4180)、[postgresql CSV说明](https://www.postgresql.org/docs/9.4/static/sql-copy.html)。

**可配置的保留字段**

在投递OSS过程中，除了使用日志本身的Key-Value外，日志服务保留同时提供以下几个保留字段可供选择：

|保留字段|语义|
|:---|:-|
|`__time__`|日志的 Unix 时间戳（是从 1970 年 1 月 1 日开始所经过的秒数），由用户日志字段的 time 计算得到。|
|`__topic__`|日志的 topic。|
|`__source__`|日志来源的客户端 IP。|

JSON格式存储会默认带上以上字段内容。

CSV存储可以根据您的需求自行选择。例如您需要日志的topic，那么可以填写字段名：`__topic__`。

## OSS存储地址 {#section_ilk_m5j_5cb .section}

|压缩类型|文件后缀|OSS文件地址举例|
|:---|:---|:--------|
|无压缩|.csv|oss://oss-shipper-shenzhen/ecs\_test/2016/01/26/20/54\_1453812893059571256\_937.csv|
|snappy|.snappy.csv|oss://oss-shipper-shenzhen/ecs\_test/2016/01/26/20/54\_1453812893059571256\_937.snappy.csv|

-   CSV是可读格式，未压缩的文件可以直接从OSS下载以文本形式打开查看。
-   如果使用了snappy压缩，解压缩的详细说明请查看[Snappy压缩文件](intl.zh-CN/用户指南/数据投递/投递日志到OSS/Snappy压缩文件.md)。

## 数据消费 {#section_uss_n5j_5cb .section}

**HybridDB**

建议配置如下：

-   分隔符 delimiter：逗号\(,\)
-   转义符 quote：双引号\(“\)
-   无效字段内容 null：不填写（空）
-   投递字段名称 header：不勾选（HybirdDB默认csv文件行首无字段说明）

