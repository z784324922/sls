# Parquet格式 {#reference_ehj_j4q_zdb .reference}

本文档主要介绍日志服务投递OSS使用Parquet存储的相关配置，关于投递日志到OSS的其它内容请参考[投递日志到OSS](cn.zh-CN/用户指南/数据投递/投递日志到OSS.md)。

## Parquet存储字段配置 {#section_dgc_4tj_5cb .section}

**数据类型**

Parquet存储支持6种类型：string、boolean、int32、int64、float、double。

日志投递过程中，会将日志服务数据由字符串转换为Parquet目标类型。如果转换到非String类型失败，则该列数据为null。

**列配置**

请依次填写Parquet中需要的日志服务数据字段名和目标数据类型，在投递时将按照该字段顺序组织Parquet数据，并使用日志服务的字段名称作为Parquet数据列名，以下两种情况发生时将置数据列值为null：

-   该字段名在日志服务数据中不存在。
-   该字段由string转换非string（如double、int64等）失败。

    ![](images/5812_zh-CN.png "字段配置")


**可配置的保留字段**

在投递OSS过程中，除了使用日志本身的Key-Value外，日志服务保留同时提供以下几个保留字段可供选择：

|保留字段|语义|
|:---|:-|
|`__time__`|日志的 Unix 时间戳（是从 1970 年 1 月 1 日开始所经过的秒数），由用户日志字段的 time 计算得到。|
|`__topic__`|日志的 Topic。|
|`__source__`|日志来源的客户端 IP。|

JSON格式存储会默认带上以上字段内容。

Parquet、CSV存储可以根据您的需求自行选择。例如您需要日志的Topic，那么可以填写字段名：`__topic__`，字段类型string。

## OSS存储地址 {#section_xxm_5tj_5cb .section}

|压缩类型|文件后缀|OSS文件地址举例|
|:---|:---|:--------|
|无外部压缩|.parquet|oss://oss-shipper-shenzhen/ecs\_test/2016/01/26/20/54\_1453812893059571256\_937.parquet|
|snappy|.snappy.parquet|oss://oss-shipper-shenzhen/ecs\_test/2016/01/26/20/54\_1453812893059571256\_937.snappy.parquet|

## 数据消费 {#section_vfx_vtj_5cb .section}

**E-MapReduce / Spark / Hive**

请参考[社区文档](https://cwiki.apache.org//confluence/display/Hive/LanguageManual+DDL)。

**单机校验工具**

开源社区提供的[parquet-tools](https://github.com/apache/parquet-mr/tree/master/parquet-tools?spm=0.0.0.0.dIDapW)可以用来文件级别验证Parquet格式、查看schema、读取数据内容。

您可以自行编译该工具或者点击[下载](http://logservice-resource.oss-cn-shanghai.aliyuncs.com/tools/parquet-tools-1.6.0rc3-SNAPSHOT.jar)日志服务提供的版本。

-   查看Parquet文件schema

    ```
    $ java -jar parquet-tools-1.6.0rc3-SNAPSHOT.jar schema -d 00_1490803532136470439_124353.snappy.parquet | head -n 30
    message schema {
      optional int32 __time__;
      optional binary ip;
      optional binary __source__;
      optional binary method;
      optional binary __topic__;
      optional double seq;
      optional int64 status;
      optional binary time;
      optional binary url;
      optional boolean ua;
    }
    creator: parquet-cpp version 1.0.0
    file schema: schema
    --------------------------------------------------------------------------------
    __time__: OPTIONAL INT32 R:0 D:1
    ip: OPTIONAL BINARY R:0 D:1
    .......
    ```

-   查看Parquet文件全部内容

    ```
    $ java -jar parquet-tools-1.6.0rc3-SNAPSHOT.jar head -n 2 00_1490803532136470439_124353.snappy.parquet
    __time__ = 1490803230
    ip = 10.200.98.220
    __source__ = *.*.*.*
    method = POST
    __topic__ =
    seq = 1667821.0
    status = 200
    time = 30/Mar/2017:00:00:30 +0800
    url = /PutData?Category=YunOsAccountOpLog&AccessKeyId=*************&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=********************************* HTTP/1.1
    __time__ = 1490803230
    ip = 10.200.98.220
    __source__ = *.*.*.*
    method = POST
    __topic__ =
    seq = 1667822.0
    status = 200
    time = 30/Mar/2017:00:00:30 +0800
    url = /PutData?Category=YunOsAccountOpLog&AccessKeyId=*************&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=********************************* HTTP/1.1
    ```


更多用法请执行：`java -jar parquet-tools-1.6.0rc3-SNAPSHOT.jar -h`，参考帮助。

