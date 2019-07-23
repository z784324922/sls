# Log {#concept_jcq_kpn_vdb .concept}

A log is an abstraction of changes that happen in a system. A log is a sequence of records ordered by time, and contains information about operations and results of specific objects. Log files, events, binary logs, and metrics are different types of log carriers. A log file is composed of one or more logs, and every log describes a single system event. A log is the minimum data unit processed in Log Service.

Log Service uses a semi-structured data model to define a log. This model includes these data fields: Topic, Time, Content, Source, and Tags.

Log Service has different format requirements for different data fields, as described in the following table.

|Data field|Description|Format|
|:---------|:----------|:-----|
|Topic|This is a user-defined field used to mark multiple logs. For example, access logs can be marked based on sites.|Any string of up to 128 bytes in length, including a null string. By default, this field is a null string.|
|Time|This is a reserved field in a log and is used to indicate the time when a log is generated. In most cases, it is generated directly based on the time in a log.|An integer in the UNIX timestamp format. The unit is in seconds. This field indicates the number of seconds that have elapsed since 1970-1-1 00:00:00 UTC.|
|Content|This field is used to record the specific content of a log. The content consists of one or more content items, and each content item is a key-value pair.|A key is a UTF-8 encoded string of up to 128 bytes in length. It can contain letters, underscores, and digits. It cannot start with a digit or use any of the following keywords: -   `__time__`
-   `__source__`
-   `__topic__`
-   `__partition_time__`
-   `_extract_others_`
-   `__extract_others__`

 The value can be any string of up to 1,024 bytes.|
|Source|This field indicates the source of a log. For example, the IP address of the server where a log is generated.|Any string of up to 128 bytes in length. This field is null by default.|
|Tags|Log tags include: -   User-defined tags: the tags that you add when you use the [PutLogs](../../../../reseller.en-US/API Reference/Logstore related APIs/PutLogs.md) API operation to write data.
-   [System tags](../../../../reseller.en-US/User Guide/Preparation/Manage a Logstore.md#ul_ubv_lws_cfb): the tags added by Log Service, including `__client_ip__` and `__receive_time__`.

 |Dictionary format. Both keys and values are strings. When you query logs in the console, the system displays the tags with the `__tag__:` prefix.|

Logs are used in various formats in actual scenarios. The following example shows you how to map an original NGINX access log to the log data model of Log Service. For example, the IP address of your NGINX server is `10.249.201.117`. An original log of this server is as follows:

``` {#codeblock_ecx_ewo_lur}
10.1.168.193 - - [01/Mar/2012:16:12:07 +0800] "GET /Send? AccessKeyId=8225105404 HTTP/1.1" 200 5 "-" "Mozilla/5.0 (X11; Linux i686 on x86_64; rv:10.0.2) Gecko/20100101 Firefox/10.0.2"
```

The following example shows how to map the original log to the log data model of Log Service.

|Data field|Content|Description|
|:---------|:------|:----------|
|Topic|""|The default null string is used.|
|Time|1330589527|The exact timestamp when the log is generated. This timestamp is the number of seconds that have elapsed since 1970-1-1 00:00:00 UTC. The time is converted from the timestamp of the original log.|
|Content|Key-value pair|The specific content of a log.|
|Source|"10.249.201.117"|The IP address of the server is used as the log source.|
|Tags|None|You or Log Service add the tags.|

You can decide how to extract the original content of a log and combine the extracted content into key-value pairs, as shown in the following table.

|Key|Value|
|:--|:----|
|ip|"10.1.168.193"|
|method|"GET"|
|status|"200"|
|length|"5"|
|ref\_url|"-"|
|browser|"Mozilla/5.0 \(X11; Linux i686 on x86\_64; rv:10.0.2\) Gecko/20100101 Firefox/10.0.2"|

