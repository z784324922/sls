# Enable and set indexes {#task_jqz_v55_cfb .task}

Before using the LogSearch/Analytics function of Log Service, you need to enable and set indexes for the logs.

You can query the collected logs only after you enable and set indexes for the logs. Set indexes based on the log fields and your query requirements.

**Note:** 

-   After the LogSearch/Analytics function is enabled, data is indexed on the backend server. Therefore, index traffic is incurred and index storage space is required.
-   Index settings take effect only on the data recorded after the settings are enabled or modified.
-   At least one of the following indexes must be enabled for a log: full text index and key/value index.
-   To use SQL statements to [analyze](reseller.en-US/User Guide/Index and query/Syntax description.md) the query result of a field, enable the **Analytics** function of the field.
-   If you want to set an index for a [Tags](../reseller.en-US/Product Introduction/Basic concepts/Log.md#table_jvn_5fd_jbb) field, such as an Internet IP address or a Unix timestamp, set the **Key Name** to a value in the `__tag__:key` format, for example, `_tag__:__receive_time__`. A Tags field does not support indexes of the numeric type. Instead, set the **Type** of all Tags fields to text. For example, to query a field with the key name `__tag__:__receive_time__`, you can use a fuzzy value, such as `__tag__:__receive_time__: 1537928*`, or the full value of the field, such as `__tag__:__receive_time__: 1537928404` as the keyword.

When a log is collected, information about the log, such as the source and time, is automatically added to the log as key/value pairs. These fields are reserved in Log Service. When you enable and set indexes for logs, the indexes and the Analytics function are automatically enabled for these fields.

**Note:** The delimiters of the `__topic__` and `__source__` fields are null. It means that the keywords used to query the two fields must match the field values.

|Name|Description|
|:---|:----------|
|`__topic__`|Indicates the log topic. If you set a [topic](reseller.en-US/User Guide/Logtail collection/Text logs/Log topic.md) for a log, Log Service automatically adds a topic field to the log. The key of the field is `__topic__`, and the value of the field is the log topic.|
|`__source__`|Indicates the source equipment that generates the log.|
|`__time__`|Indicates the time that is specified when the log is recorded by the SDK.|

1.  Log on to the [Log Service console](https://partners-intl.console.aliyun.com/#/sls) and click the project name. 
2.  In the **LogSearch** column, click **Search**. 
3.  Click **Enable** in the upper right corner. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21321/154469579112614_en-US.png)

    **Note:** If you have created an index, click **Index Attributes** \> **Modify**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21321/154469579114015_en-US.png)

4.  Set indexes for logs. Log Service supports two indexes: full text index and key/value index. At least one of the two indexes must be set for a log.

    **Note:** If both a full text index and a key/value index are set for a log, the key/value index prevails.

    |Index type|Description|
    |:---------|:----------|
    |Full text index|Indicates that all fields in the log are queried as text with a key/value index. The key and value of the index are text and can both be queried. You do not need to specify the key name in queries.|
    |Key/Value index|After setting a key/value index for a field, you must specify the key name to query the field. If a full text index is set for a log and a key/value index is set for a field in the log, the full text index does not take effect on the field.You can set multiple data types for a field, including:

    -   [Text](reseller.en-US/User Guide/Index and query/Data type of index/Text type.md)
    -   [JSON](reseller.en-US/User Guide/Index and query/Data type of index/JSON type.md)
    -   [Numeric \(Long and Double\)](reseller.en-US/User Guide/Index and query/Data type of index/Value type.md)
|

    1.  Set a full text index for a log. 

        You can set an index for the full content of a log. The values of all keys in the log are queried by default when you query the log.

        |Parameter|Description|Example|
        |:--------|:----------|:------|
        |**Full Text Index**|If this option is enabled, an index is enabled for the full content of the log. The values of all keys in the log are queried by default. The log can be queried if any one of the keys matches the keyword.|-|
        |**Case Sensitive**|Specifies whether the queries are case-sensitive.        -   If this option is disabled, the queries are not case-sensitive, that is, an internal error log can be queried by both of the keywords "INTERNALERROR" and "internalerror".****
        -   If this option is enabled, the queries are case-sensitive, that is, a log that includes "internalError" can be queried only by the keyword "internalError".****
|-|
        |**Chinese character**|Sets whether to distinguish between English and Chinese.        -   After opening, if the log contains Chinese, the Chinese word segmentation is carried out according to the Chinese grammar, word Segmentation is carried out in English according to the word segmentation characters.
        -   When closed, word all the content according to the word segmentation.
|-|
        |**Delimiter**|Specifies single-byte characters used to separate a log into multiple keywords. For example, if the content of a log is `a,b;c;D-F`, you can specify the comma \(,\), semi-colon \(;\), and hyphen \(-\) as delimiters to separate the log into five keywords: ”a”, “b”, “c”, “D”, and “F”.|`, '";=()[]{}? @&<>/:\n\t`|

    2.  Set key/value indexes for a log. 

        You can set indexes for specified keys. After setting key/value indexes for a log, you can query specified keys to narrow down the query scope.

        **Note:** 

        -   Log Service automatically creates indexes for the [reserved fields](#) and enables the Analytics function of the fields. The reserved fields include `__topic__`, `__source__`, and `__time__`.

        -   The settings in the Customize tab page are described as an example in this topic. The Nginx Template and MNS Template are used only to collect Nginx logs and MNS logs and do not support customized index settings.

        -   If you want to set an index for a [Tags](../reseller.en-US/Product Introduction/Basic concepts/Log.md#table_jvn_5fd_jbb) field, such as an Internet IP address or a Unix timestamp, set the **Key Name** to a value in the `__tag__:key` format, for example, `_tag__:__receive_time__`. A Tags field does not support indexes of the numeric type. Set the **Type** of all Tags fields to text. For example, to query a field with the key name `__tag__:__receive_time__`, you can use a fuzzy value, such as `__tag__:__receive_time__: 1537928*`, or the full value of the field, such as `__tag__:__receive_time__: 1537928404` as the keyword.
        |Parameter|Description|Example|
        |:--------|:----------|:------|
        |**Key Name**|Specifies the name of a field in the log.|`_address_`|
        |**Type**|Specifies the data type of a field in the log, including:        -   **text**: Indicates that the content of the field is text.
        -   **long**: Indicates that the content of the field is an integer. This field must be queried by a value range.
        -   **double**: Indicates that the content of the field is a floating-point number. This field must be queried by a value range.
        -   **json**: Indicates that the content of the field is in JSON format.
**Note:** Numeric types \(Long and Double\) do not support **Case Sensitive** or **Delimiter**.****

|-|
        |**Alias**|Indicates the alias of a column.An alias is used only for SQL statistics. A field is still identified by its original name in the underlying storage. Therefore, you must use the original name of a field to query the field. For more information, see [Column alias](reseller.en-US/User Guide/Index and query/Analysis grammar/Column alias.md).

|`address`|
        |**Case Sensitive**|Specifies whether the queries are case-sensitive. This parameter has two values:        -    **false**: The queries are not case-sensitive, that is, the sample log can be queried by both of the keywords "INTERNALERROR" and "internalerror".
        -   **true**: The queries are case-sensitive, that is, the sample log can be queried only by the keyword "internalError".
|-|
        |**Delimiter**|Specifies single-byte characters used to separate a log into multiple keywords.For example, if the content of a log is `a,b;c;D-F`, you can specify the comma \(,\), semi-colon \(;\), and hyphen \(-\) as delimiters to separate the log into five keywords: ”a”, “b”, “c”, “D”, and “F”.

|`, '";=()[]{}? @&<>/:\n\t`|
        |**Enable Analytics**|Specifies whether the Analytics function is enabled. This function is enabled by default.After enabling the Analytics function, you can use query and analysis statements to analyze the query results.

|-|

5.  Click **OK**. 

    **Note:** 

    -   The index settings take effect within one minute.
    -   Index settings take effect only on data recorded after the settings are enabled or modified.

