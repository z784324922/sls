# CSV storage {#concept_hch_k4q_zdb .concept}

This document introduces the configurations about CSV storage for Log Service logs that are shipped to Object Storage Service \(OSS\). For more information about shipping logs to OSS, see [Ship logs to OSS](intl.en-US/User Guide/Data shipping/Ship logs to OSS.md).

## Configure CSV storage fields {#section_vhb_f5j_5cb .section}

**Configuration page**

You can view multiple key-value pairs of one log on the Log Service data preview page or index query page. Enter the field names \(keys\) you want to ship to OSS in sequence.

If the key name you entered cannot be found in the log, the corresponding column is set to null. 

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13182/5813_en-US.png "Configuration item")

**Configuration item**

|Configuration item|Value|Note|
|:-----------------|:----|:---|
|Delimiter|character|A one-character string used to separate different fields.|
|Quote|character|A one-character string. If a field contains a delimiter or a line break, use quote to enclose this field to avoid incorrect field separation in data reading.|
|Escape|character|A one-character string. The default settings are the same as those of quote. Modification is not supported currently.  If a field contains a quote \(used as a regular character instead of an escape character\), an escape character must be added before this quote.|
|Invalid Key Value|string|If the specified key value does not exist, this string is entered in the field to indicate the field is null.|
|Display Key header|boolean|Indicates whether or not to add the field name to the first line of the CSV file.|

For more information, see [CSV](https://tools.ietf.org/html/rfc4180) standard and [postgresql CSV description](https://www.postgresql.org/docs/9.4/static/sql-copy.html).

**Configurable reserved fields**

Besides the key-value pairs of the log, Log Service also provides the following optional reserved fields when shipping logs to OSS.

|Reserved field|Description|
|:-------------|:----------|
|`__time__`|  The UNIX timestamp of a log \(the number of seconds since 1970-01-01\), which is calculated  according to the time field of your log. |
|`__topic__`|The log topic.|
|`__source__`|The IP address of the client from which a log comes.|

The preceding fields are included by default in JSON storage.

You can select which fields you want to include in the CSV storage as per your needs.  For example, you can enter the field name `__topic__` if you need the log topic.

## OSS storage address {#section_ilk_m5j_5cb .section}

|Compression type|File suffix|Example of OSS file address|
|:---------------|:----------|:--------------------------|
|Do Not Compress|.csv|oss://oss-shipper-shenzhen/ecs\_test/2016/01/26/20/54\_1453812893059571256\_937.csv|
|snappy|.snappy.csv|oss://oss-shipper-shenzhen/ecs\_test/2016/01/26/20/54\_1453812893059571256\_937.snappy.csv|

## Consume data {#section_uss_n5j_5cb .section}

**HybridDB**

We recommend that you configure as follows:

-   Delimiter: comma \(,\)
-   Quote: double quotation marks \(“\)
-   Invalid Key Value: empty
-   Display Key: not selected \(no field name in the first line of the CSV file for HybridDB by default\)

**For more information, see HybridDB document.**

CSV is a readable format, which means that a file in CSV format can be directly downloaded from OSS and viewed in text form.

If Compress \(snappy\) is used as the compression type, see the decompression descriptions of snappy in [JSON storage](intl.en-US/User Guide/Data shipping/Ship logs to OSS/JSON storage.md).

