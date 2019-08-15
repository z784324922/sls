# Reserved fields {#concept_adr_ktr_gfb .concept}

In Log Service, some fields are reserved. When you add Logtail configurations or call API operations to write log data, do not set the names of fields to be the same as those of reserved fields.

## Important notes {#section_ck5_ns2_ggb .section}

When collecting logs or delivering data to other cloud products, Log Service can add information such as log sources and timestamps to logs in key-value format. Fields with fixed names are reserved fields, for example, `__source__`.

-   When you add Logtail configurations or call API operations to write log data, do not set the names of fields \(that is, keys\) to be the same as those of reserved fields. Otherwise, duplicate field names may cause problems such as inaccurate queries.
-   Log Service does not deliver any fields prefixed with `__tag__`.
-   Log Service charges you in [Billing method](../../../../reseller.en-US/Pricing/Billing method.md) mode for the new fields that you specify to be recorded in logs. If you enable the indexing feature for the fields, you are also charged a small fee for the indexing and storage traffic.

## Reserved fields {#section_dy3_4s2_ggb .section}

The following table describes the reserved fields in Log Service.

|Field|Type|Index and statistics settings|Description|
|:----|:---|:----------------------------|:----------|
|`__time__`|Integer, in UNIX timestamp format. For example, `__time__: 1523868463`.

 | -   Index settings: You do not need to create an index on the `__time__` field because this field can be set through the from and to parameters in API operations.
-   Statistics settings: Statistics for the `__time__` field are enabled after you enable the statistics feature for any column.

 |The log time that you specify when you use the API or SDK to write log data. This field can be used for log delivery, query, and analysis.|
|`__source__`|String.| -   Index settings: After the indexing feature is enabled for the `__source__` field, Log Service creates an index on this field by default. The index is of the text type, and no delimiter is specified. To query logs based on the index on this field, enter `source:127.0.0.1` or `__source__:127.0.0.1`.
-   Statistics settings: Statistics for the `__source__` field are enabled after you enable the statistics feature for any column.

 |The device from which logs are collected. This field can be used for log delivery, query, analysis, and custom consumption.|
|`__topic__`|String.| -   Index settings: After the indexing feature is enabled for the `__topic__` field, Log Service creates an index on this field by default. The index is of the text type, and no delimiter is specified. To query logs based on the index on this field, enter `__topic__:XXX`.
-   Statistics settings: Statistics for the `__topic__` field are enabled after you enable the statistics feature for any column.

 |The topic of logs. If you have set a [log topic](reseller.en-US/Data Collection/Logtail collection/Text logs/Set a log topic.md), Log Service automatically adds the topic field to your logs with the key set to `__topic__` and the value set to the topic content that you specified. This field can be used for log delivery, query, analysis, and custom consumption.|
|`_extract_others_`|String, which can be deserialized into a JSON map.|You do not need to create an index on this field because this field does not exist in any logs.|This field is the same as the `__extract_others__` field. We recommend that you use the `__extract_others__` field.|
|`__tag__:__client_ip__`|String.| -   Index settings: After the indexing feature is enabled for this field, Log Service creates indexes on all the [tag](reseller.en-US/Product Introduction/Basic concepts/Log.md) fields by default. The index is of the text type, and no delimiter is specified. When you query logs based on the index on a tag field, both exact match and fuzzy match are supported.
-   Statistics settings: The statistics feature is disabled for the column indicated by this field. To enable statistics for this field, create an index on the `__tag__:__client_ip__` field and enable the statistics feature.

 |The public IP address of the device from which logs are collected. This field is a system tag. After the [Log Public IP](../../../../reseller.en-US/Preparation/Manage a Logstore.md) feature is enabled, the server adds this field to each raw log received. This field can be used for log query, analysis, and custom consumption. When conducting SQL analysis on this field, you must enclose this field in double quotation marks \(" "\).

 |
|`__tag__:__receive_time__`|String, which can be converted to an integer in UNIX timestamp format.| -   Index settings: After the indexing feature is enabled for this field, Log Service creates indexes on all the [tag](reseller.en-US/Product Introduction/Basic concepts/Log.md) fields by default. The index is of the text type, and no delimiter is specified. When you query logs based on the index on a tag field, both exact match and fuzzy match are supported.
-   Statistics settings: The statistics feature is disabled for the column indicated by this field. To enable statistics for this field, create an index on the `__tag__:__receive_time__` field and enable the statistics feature.

 |The time when the server receives a log. This field is a system [tag](reseller.en-US/Product Introduction/Basic concepts/Log.md). After the [Log Public IP](../../../../reseller.en-US/Preparation/Manage a Logstore.md) feature is enabled, the server adds this field to each raw log received. This field can be used for log query, analysis, and custom consumption.|
|`__tag__:__path__`|String.| -   Index settings: After the indexing feature is enabled for the `__tag__:__path__` field, Log Service creates an index on this field by default. The index is of the text type, and no delimiter is specified. To query logs based on the index on this field, enter `__tag__:__path__:XXX`.
-   Statistics settings: The statistics feature is disabled for the column indicated by this field. To enable statistics for this field, create an index on the `__tag__:__path__` field and enable the statistics feature.

 |The path to log files collected by Logtail. Logtail automatically adds this field to logs. This field can be used for log query, analysis, and custom consumption. When conducting SQL analysis on this field, you must enclose this field in double quotation marks \(" "\).

 |
|`__tag__:__hostname__`|String.| -   Index settings: After the indexing feature is enabled for the `__tag__:__hostname__` field, Log Service creates an index on this field by default. The index is of the text type, and no delimiter is specified. To query logs based on the index on this field, enter `__tag__:__hostname__:XXX`.
-   Statistics settings: The statistics feature is disabled for the column indicated by this field. To enable statistics for this field, create an index on the `__tag__:__hostname__` field and enable the statistics feature.

 |The hostname of the device from which Logtail collects data. Logtail automatically adds this field to logs. This field can be used for log query, analysis, and custom consumption. When conducting SQL analysis on this field, you must enclose this field in double quotation marks \(" "\).

 |
|`__raw_log__`|String.|You need to create and configure an index of the text type on this field and enable the statistics feature as needed.|The raw logs that fail to be parsed. After the [Drop Failed to Parse Logs](../../../../reseller.en-US/Data Collection/Logtail collection/Text logs/Collect text logs.md#table_eq2_ccc_wdb) feature is disabled, Logtail uploads raw logs if log parsing fails. In this field, the key is `__raw_log__` and the value is the log content. This field can be used for log delivery, query, analysis, and custom consumption.|
|`__raw__`|String.|You need to create and configure an index of the text type on this field and enable the statistics feature as needed.|The raw logs that are parsed. After the [Upload Raw Log](../../../../reseller.en-US/Data Collection/Logtail collection/Text logs/Collect text logs.md#table_eq2_ccc_wdb) feature is enabled, Logtail uploads the raw logs in the `__raw__` field together with the parsed logs. This field is used in log audit and compliance check scenarios. This field can be used for log delivery, query, analysis, and custom consumption.|

