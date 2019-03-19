# Reserved fields {#concept_adr_ktr_gfb .concept}

In Log Service, some fields are reserved fields. When you use APIs to write data to logs or add Logtail Configs, the names of the required fields cannot be the same as those of the reserved fields.

## Precautions {#section_ck5_ns2_ggb .section}

When collecting logs or delivering data to other cloud products, Log Service can add information, such as log sources and timestamps, in Key-Value format to logs. Fields with fixed names, for example, `_ Source __`, are reserved fields.

-   When using APIs to write data to logs or adding Logtail Configs, do not set the names of the required fields to be the same as those of the reserved fields. Otherwise, your queries may be inaccurate as a result.
-   Fields with a prefix of `__tag__` cannot be delivered.

## Reserved fields {#section_dy3_4s2_ggb .section}

The following table describes the reserved fields.

|Reserved field|Type|Index and statistics settings|Description|
|:-------------|:---|:----------------------------|:----------|
|`_ Time __`|Integer in standard Unix time format,for example, `__time__: 1523868463`

| -   Index settings: You do not need to add an index for this field because the field can be set through the from and to parameters in APIs.
-   Statistics settings: By default, statistics for this field are enabled after you enable the statistics function for any other column.

 |This field specifies the log generation time when you use APIs or SDKs to write data to logs. It can be used for log delivery, query, and analysis.|
|`__source__`|String| -   Index settings: After the index function is enabled, Log Service creates an index for this field by default. The index is of the text type, and no delimiter is specified. If you want to query this field, enter `source:127.0.0.1` or `__source__:127.0.0.1`.
-   Statistics settings: By default, statistics for this field are enabled after you enable the statistics function for any other column.

 |This field specifies the device from which logs are collected. It can be used for log delivery, query, analysis, and custom consumption.|
|`__topic__`|String| -   Index settings: After the index function is enabled, Log Service creates an index for this field by default. The index is of the text type, and no delimiter is specified. If you want to query this field, enter `__topic__:XXX`.
-   Statistics settings: By default, statistics for this field are enabled after you enable the statistics function for any other column.

 |This field specifies the log topic. If you have set a [log topic](reseller.en-US/User Guide/Logtail collection/Text logs/Log topic.md), Log Service automatically adds a field to your log with the Key set to `__topic__` and Value set as the topic content you specified. This field can be used for log delivery, query, analysis, and custom consumption.|
|`_extract_others_`|String that can be deserialized into a JSON map|You do not need to add an index for this field because the field does not exist in any log.|This field works the same as `__extract_others__`. We recommend that you use `__extract_others__`.|
|`__tag__:__client_ip__`|String| -   Index settings: After the index function is enabled, Log Service creates indexes for all [tags](reseller.en-US/Product Introduction/Basic concepts/Log.md) by default. The index is of the text type, and no delimiter is specified. Both accurate search and fuzzy search are supported.
-   Statistics settings: By default, the statistics function is disabled for the column indicated by this field. If you want to enable statistics for this field, add an index for the field and then enable the statistics function.

 |This field is a system tag and specifies the Internet IP address of the device from which logs are collected. After the [recording Internet IP addresses](../../../../../reseller.en-US/User Guide/Preparation/Manage a Logstore.md) function is enabled, the server adds this field for a raw log after receiving the log. This field can be used for log query, analysis, and custom consumption.|
|`__tag__:__receive_time__`|String that can be converted to integer in standard Unix time format| -   Index settings: After the index function is enabled, Log Service creates indexes for all [tags](reseller.en-US/Product Introduction/Basic concepts/Log.md) by default. The index is of the text type, and no delimiter is specified. Both accurate search and fuzzy search are supported.
-   Statistics settings: By default, the statistics function is disabled for this column. If you want to enable statistics for this field, add an index for the field and then enable the statistics function.

 |This field is a system [tag](reseller.en-US/Product Introduction/Basic concepts/Log.md) and specifies the time when the server receives a log. After the [recording Internet IP addresses](../../../../../reseller.en-US/User Guide/Preparation/Manage a Logstore.md) function is enabled, the server adds this field for a raw log after receiving the log. This field can be used for log query, analysis, and custom consumption.|
|`__tag__:__path__`|String| -   Index settings: After the index function is enabled, Log Service creates an index for this field by default. The index type is text, and no delimiter is specified. If you want to query this field, enter `__tag__:__path__:XXX`.
-   Statistics settings: By default, statistics for this field are enabled after you enable the statistics function for any other column.

 |This field specifies the log file path collected by Logtail. Logtail automatically adds this field to logs. It can be used for log query, analysis, and custom consumption.|
|`__tag__:__hostname__`|String| -   Index settings: After the index function is enabled, Log Service creates an index for this field by default. The index is of the text type, and no delimiter is specified. If you want to query this field, enter `__tag__:__hostname__:XXX`.
-   Statistics settings: By default, statistics for this field are enabled after you enable the statistics function for any other column.

 |This field specifies the name of the host from which Logtail collects data. Logtail automatically adds this field to logs. It can be used for log query, analysis, and custom consumption.|
|`__raw_log__`|String|You need to add and set an index of the text type for this field and enable the statistics function as needed.|This field specifies raw logs with parsing failure. After the [discarding logs with parsing failure](../../../../../reseller.en-US/User Guide/Logtail collection/Text logs/Collect text logs.md#table_eq2_ccc_wdb) function is disabled, Logtail uploads raw logs once log parsing fails. In this field, Key is `__raw_log__` and Value is the log content. This field can be used for log delivery, query, analysis, and custom consumption.|
|`__raw__`|String|You need to add and set an index of the text type for this field and enable the statistics function as needed.|This field indicates raw logs that are successfully parsed. After the [uploading raw logs](../../../../../reseller.en-US/User Guide/Logtail collection/Text logs/Collect text logs.md#table_eq2_ccc_wdb) function is enabled, Logtail regards raw logs as this field and upload the logs with the logs that are successfully parsed. Generally, this field is used for log audit and compliance check. It can also be used for log delivery, query, analysis, and custom consumption.|

