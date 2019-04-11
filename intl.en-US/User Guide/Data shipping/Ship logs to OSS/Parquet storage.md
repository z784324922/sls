# Parquet storage {#reference_ehj_j4q_zdb .reference}

This document introduces the configurations about Parquet storage for Log Service logs that are shipped to Object Storage Service \(OSS\). For more information about shipping logs to OSS, see [Ship logs to OSS](reseller.en-US/User Guide/Data shipping/Ship logs to OSS/Ship logs to OSS.md#).

## Configure Parquet storage fields {#section_dgc_4tj_5cb .section}

**Data types**

The Parquet supports the storage in six formats, including string, boolean, int32, int64, float, and double.

Log Service data will be converted from strings into the target Parquet type during log shipping. If any data fails to be converted into a non-string type, the corresponding column is filled with null.

**Configure columns**

Configure the Log Service data field names and the target data types required by Parquet. Parquet data is organized according to this field order when being shipped. The Log Service field names are used as the Parquet data column names. The data column is set to null if:

-   This field name does not exist in Log Service data.
-   This field fails to be converted from a string to a non-string \(such as double and int64\).

    ![](images/5812_en-US.png "Field Configuration")


**Configurable reserved fields**

Besides the key-values of the log, the Log Service also provides the following optional reserved fields for the shipping to OSS:

|Reserved field|Description|
|:-------------|:----------|
|`__time__`|The UNIX timestamp of a log \(the number of seconds since 1970-01-01\), which is calculated according to the time field of your log.|
|`__topic__`|The log topic.|
|`__source__`|The IP address of the client from which a log comes.|

The preceding fields are carried by default in JSON storage.

You can select which fields you want to include in the Parquet or CSV storage as per your needs. For example, you can enter the field name \_\_topic\_\_ and select string as the type if you need the log topic.

## OSS storage address {#section_xxm_5tj_5cb .section}

|Compression type|File suffix|Example of OSS file address|
|:---------------|:----------|:--------------------------|
|Do Not Compress| .parquet |oss://oss-shipper-shenzhen/ecs\_test/2016/01/26/20/54\_1453812893059571256\_937.parquet|
|snappy|.snappy.parquet|oss://oss-shipper-shenzhen/ecs\_test/2016/01/26/20/54\_1453812893059571256\_937.snappy.parquet|

## Consume data {#section_vfx_vtj_5cb .section}

**E-MapReduce/Spark/Hive**

See [community document](https://cwiki.apache.org//confluence/display/Hive/LanguageManual+DDL).

**Stand-alone verification tool**

The [parquet-tools](https://github.com/apache/parquet-mr/tree/master/parquet-tools?spm=0.0.0.0.dIDapW) provided by the open-source community is used to verify the Parquet format, view schema, and read data at the file level.

You can compile this tool by yourself or click [Download](http://logservice-resource.oss-cn-shanghai.aliyuncs.com/tools/parquet-tools-1.6.0rc3-SNAPSHOT.jar) to download the version provided by Log Service.

-   View the schema of the Parquet file

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

-   View all contents of the Parquet file

    ```
    $ java -jar parquet-tools-1.6.0rc3-SNAPSHOT.jar head -n 2 00_1490803532136470439_124353.snappy.parquet
    __time__ = 1490803230
    ip = 10.200.98.220
    __source__ = *. *. *.*
    method = POST
    __topic__ =
    seq = 1667821.0
    status = 200
    time = 30/Mar/2017:00:00:30 +0800
    url = /PutData? Category=YunOsAccountOpLog&AccessKeyId=*************&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=********************************* HTTP/1.1
    __time__ = 1490803230
    ip = 10.200.98.220
    __source__ = *. *. *.*
    method = POST
    __topic__ =
    seq = 1667822.0
    status = 200
    time = 30/Mar/2017:00:00:30 +0800
    url = /PutData? Category=YunOsAccountOpLog&AccessKeyId=*************&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=********************************* HTTP/1.1
    ```


For more operation instructions, run the j`java -jar parquet-tools-1.6.0rc3-SNAPSHOT.jar -h` command.

