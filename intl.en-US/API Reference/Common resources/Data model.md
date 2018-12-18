# Data model {#reference_qrp_q5q_12b .reference}

To facilitate understanding and use of Log Service, the following describes some basic concepts.

## Region {#section_pwb_jqq_b2b .section}

Region is is the service node of Alibaba Cloud. By deploying services in different Alibaba Cloud regions, you can get your services closer to the customers, resulting in lower access latency and a better user experience. Currently, Alibaba Cloud provides multiple  regions across the country.

## Project {#section_pbg_gmf_sy .section}

The project is a basic management unit in Log Service and is used for resource isolation and control. You can use a project to manage logs and related log sources of one application.

## Logstore {#section_fqf_hmf_sy .section}

The Logstore is a unit in Log Service to collect,  store, and consume logs. Each logstore belongs to one project, and multiple logstores can be created for each project. You can create multiple logstores for one project according to actual needs. The common practice is to create an independent logstore for each type of log in one application. For example, you have a game application big-game, and three types of logs are on the server: operation\_log, application\_log, and access\_log. You can first create a project named big-game, and then create three Logstores in this project for these three types of logs to collect, store, and consume logs respectively.

## Log {#section_g3j_3mf_sy .section}

A log is a minimum data unit processed in Log Service. Log Service uses a semi-structured data mode to define a log. The specific data model is as follows:

-   **Topic**: A user-defined field used to mark multiple logs. For example, access logs can be marked according to the sites. This field is a null string by default \(the null string is also a valid topic\).
-   **Time**: A reserved field in the log used to indicate the log generation time \(the number of seconds since 1970-1-1 00:00:00 UTC\). Generally this field is generated directly based on the time in the log.
-   **Content**: A field used to record the specific log content. Content is composed of one or more content items, and each content item is composed of a Key-Value pair.
-   **Source**: A field used to indicate the source of the log. For example, the IP address of the machine where the log is generated. This field is blank by default.

Log Service has different requirements on values of different fields as follows.

|Data domain|Requirement|
|:----------|:----------|
|time|An integer in the standard UNIX time format. The unit is in seconds.|
|topic |A UTF-8 encoded string up to 128 bytes.|
|source|A UTF-8 encoded string up to 128 bytes.|
|content|One or more Key-Value Pair. Key is a UTF-8 encoded string of no more than 128 bytes, which contains only letters, underscores, and numbers and cannot begin with a number. The value is  a UTF-8 encoded string up to 1024\*1024 bytes.|

**Note:** The key in the content cannot use any of the following keywords: `__time__`, `__source__`, `__topic__`, `__partition_time__`, `_extract_others_`, and `__extract_others__`.

## Topic {#section_eqx_kmf_sy .section}

Logs in one Logstore can be grouped by log topics. You can specify the topic when writing a log. For example, a platform user can use the user ID as the log topic and write it into the log. If there is no need to group the logs in one Logstore, the same log topic can be used for all logs.

**Note:** A null string is a valid log topic. The default log topic is a null string.

Various log formats are used in actual application scenarios. For ease of understanding, the following describes how to map an original Nginx access\_log to the log data model in Log Service. Assume that the IP address of your Nginx server is 10.10.10.1. An original log of this server is as follows.

```
10.1.1.1 - - [01/Mar/2012:16:12:07 +0800] "GET /Send? AccessKeyId=82251054** HTTP/1.1" 200 5 "-" "Mozilla/5.0 (X11; Linux i686 on x86_64; rv:10.0.2) Gecko/20100101 Firefox/10.0.2"
```

Map the original log to the Log Service log data model as follows.

|Data domain|Item|Description|
|:----------|:---|:----------|
|topic|“”|Use the default value \(empty string\).|
|time|1330589527|The precise log generation time \(in seconds\), which is converted from the timestamp of the original log.|
|source|“10.10.10.1”|Use the IP address of the server as the log source.|
|content|Key-Value Pair|Specific log content.|

You can decide how to extract the original log contents and combine them into key-value pairs. The following table is shown as an example.

|Key|Value|
|:--|:----|
|ip|“10.1.1.1”|
|method|“GET”|
|status|“200”|
|length|“5”|
|ref\_url|“-“|
|browser|“Mozilla/5.0 \(X11; Linux i686 on x86\_64; rv:10.0.2\) Gecko/20100101 Firef|

## Logs {#section_tww_gl5_12b .section}

A collection of multiple logs.

## Loggroup {#section_i3p_hl5_12b .section}

A group of logs.

## LogGroupList {#section_zjg_3l5_12b .section}

A collection of log groups used to return results.

## Encoding method {#section_xwc_5mf_sy .section}

Currently, the system supports the following content encoding method. The RESTful API layer is indicated by Content-Type.

|Meaning|Content-Type| |
|:------|:-----------|:-|
|Protobuf|The data model is encoded by ProtoBuf.|application/x-protobuf|

The following Protocol Buffer \(Protobuf\) format defines the object of the data model.

```
message LogTag
{
    required string Key = 1;
    required string Value = 2;
}

message Log
{
    required uint32 Time = 1;// UNIX Time Format
    message Content
    {
        required string Key = 1;
        required string Value = 2;
    }  
    repeated Content Contents = 2;
}

message LogGroup
{
    repeated Log Logs= 1;
    optional string Reserved = 2; // reserved fields
    optional string Topic = 3;
    optional string Source = 4;
    repeated LogTag LogTags = 6;
}

message LogGroupList
{
    repeated LogGroup logGroupList = 1;
}
```

**Note:** 

Protobuf does not require the key-value pair to be unique. You must avoid such a situation. Otherwise, the behavior is undefined.

Protobuf follows the order of field number to encode fields in one message. Otherwise, data parsing may fail.

