# Log {#concept_jcq_kpn_vdb .concept}

Half a century ago, the term “log” was associated with a thick notebook written by a ship captain or operator. Nowadays, with the advent of computers, logs are generated and used everywhere. Servers, routers, sensors, GPS devices, orders, and various IoT devices describe the world we live from different angles by generating and using logs. With the computing power, we continuously update our recognition to the whole world and system by collecting, processing, and using logs.

## What is a log? {#section_mvy_wpd_qy .section}

Consider an example of a ship captain’s log. In addition to a recorded timestamp, a log can contain almost all sorts of information, such as a text record, an image, weather conditions, and the sailing course. After centuries passed, now the “ship captain’s log” has been expanded to various areas such as orders, payment records, user accesses, and database operations.

The reason why logs are widely used and enduring is that logs are the simplest storage abstraction. Logs are a collection of chronological records that can only be added. The following figure is what logs \(time-series data\) look like.

![](images/2376_en-US.png "Log")

We can add a record to the end of a log and read the log records from left to right. Each record has a unique log record number with a sequence.

The log sequence is determined by “time”. From the preceding figure, we can see that the log time sequence is from right to left. The new event is recorded, and the old event is gradually out of sight. But a log is a record of events. This is the foundation of recognition and reasoning to computers, humans, and the whole world.

## Logs in Log Service {#section_kfd_sfd_jbb .section}

A log is an abstraction of system changes during the running process. The log content is a time-ordered collection of some operations and the corresponding operation results of specified objects. LogFile, Event, BinLog, and Metric data are different carriers of logs. In LogFile, every log file is composed of one or more logs, and every log describes a single system event. A log is the minimum data unit processed in Log Service

Log Service defines a log by using the semi-structured data mode. This mode includes the following four data fields: Topic, Time, Content, and Source.

Meanwhile, Log Service has different format requirements for different log fields. For more information, see the following table.

|Data field|Meaning|Format|
|:---------|:------|:-----|
|Topic|A custom field used to mark multiple logs. For example, access logs can be marked according to sites.|Any string up to 128 bytes, including a null string. By default, this field is a null string.|
|Time|A reserved field in the log used to indicate the log generation time. Generally this field is generated directly based on the time in the log.|An integer in the standard UNIX time format.  The unit is in seconds. This field indicates the number of seconds since 1970-1-1 00:00:00 UTC.|
|Content|A field used to record the specific log content. The log content is composed of one or more content items, and each content item is a key-value pair.|The key is a UTF-8 encoded string up to 128 bytes, and can contain letters, underscores, and numbers. It cannot start with a number or use any of the following keywords: -   `__time__`
-   `__source__`
-   `__topic__`
-   `__partition_time__`
-   `_extract_others_`
-   `__extract_others__`

The value can be any string up to 1024\*1024 bytes.|
|Source|A field used to indicate the source of the log. For example, the IP address of the machine where the log is generated. |Any string up to 128 bytes.  By default, this field is null.|
|Tags|Tags that can be applied to logs include:-   Custom tags: which can be added when you use the [PostLogstoreLogs](../../../../../reseller.en-US/API Reference/Logstore related APIs/PostLogstoreLogs.md) API to write data.
-   [System tags](../../../../../reseller.en-US/User Guide/Preparation/Manage a Logstore .md#ul_ubv_lws_cfb): which are provided by servers, including `__client_ip__` and `__receive_time__`.

|Tags are displayed in the format used for directories, and tag keys and values must be strings. When you query logs in the console, tags are displayed with the prefix of `__tag__:`.|

Various log formats are used in actual usage scenarios. For better understanding, the following example describes how to map an original Nginx access log to the Log Service log data model. Assume that the IP address of your Nginx server is 10.249.201.117 . The following is an original log of this server.

```
10.1.168.193 - - [01/Mar/2012:16:12:07 +0800] "GET /Send? AccessKeyId=8225105404 HTTP/1.1" 200 5 "-" "Mozilla/5.0 (X11; Linux i686 on x86_64; rv:10.0.2) Gecko/20100101 Firefox/10.0.2"
```

Map the original log to the Log Service log data model as follows:

|Data field|Content|Description|
|:---------|:------|:----------|
|Topic|“”|Use the default value \(null string\).|
|Time|1330589527|The precise log generation time, indicating the number of seconds since 1970-1-1 00:00:00 UTC. The time is converted from the timestamp of the original log.|
|Content|Key-value pair|Specific log content.|
|Source|“10.249.201.117”|Use the IP address of the server as the log source.|
|Tags|None|Tags are added by users or provided by servers.|

You can decide how to extract the original log contents and combine them into key-value pairs. The following table is shown as an example.

|Key|Value|
|:--|:----|
|ip|“10.1.168.193”|
|method|“GET”|
|Status|“200”|
|length|“5”|
|ref\_url|“-“|
|browser|“Mozilla/5.0 \(X11; Linux i686 on x86\_64; rv:10.0.2\) Gecko/20100101 Firefox/10.0.2”|

