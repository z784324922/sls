# Logtail configuration {#concept_f3n_s5q_12b .concept}

By default, you can create at most 100 Logtail configurations for a project.  The configuration name must be unique in the same project.

You can use the configuration to specify the location, method, and parameters for log collection.

**The configuration naming rules are as follows:**

-   The name can only contain lowercase letters, numbers, hyphens \(-\), and underscores \(\_\).
-   The name must begin and end with a lowercase letter or number.
-   The name must be 2–128 bytes long.

Example of the complete resource:

```
{
    "configName": "testcategory1",
    "inputType": "file",
    "inputDetail": {
        "logType": "common_reg_log",
        "logPath": "/var/log/httpd/",
        "filePattern": "access.log",
        "localStorage": true,
        "timeFormat": "%Y/%m/%d %H:%M:%S",
        "logBeginRegex": ".*",
        "regex": "(\w+)(\s+)",
        "key" :["key1", "key2"],
        "filterKey":["key1"],
        "filterRegex":["regex1"],
        "topicFormat": "none"
    },
    "outputType": "sls",
    "outputDetail": 
    {
        "logstoreName": "perfcounter"
    },
    "createTime"": 1400123456,
    "lastModifyTime": 1400123456
}
```

|Attribute name|Type|Required|Description|
|:-------------|:---|:-------|:----------|
|configName|string|Yes|The Logtail configuration name, which is unique in the same project.|
|Inputtype|string|Yes|The input type. Currently, only file is supported.|
|inputDetail|json|Yes|See the descriptions in the following table.|
|outputType|string|Yes| The output type. Currently, only LogService is supported.|
|outputDetail|string|Yes|See the descriptions in the following table.|
|createTime\(output-only\)|Integer|No|The created time of the configuration.|
|lastModifyTime\(output-only\)|Integer|No| The time when the resource is updated in Log Service.|

inputDetail contents

|Attribute name|Type|Attribute name|Description|
|:-------------|:---|:-------------|:----------|
|Type|string|Yes|The log type. Currently, only common\_reg\_log is supported.|
|LogPath|string|Yes| The parent directory where the log resides. For example, /var/logs/.|
|filePattern|string|Yes|The pattern of a log file. For example, `access*.log`.|
|localStorage|boolean|Yes|If the local cache is turned on, in the event that the link between the server is disconnected, 1 GB log can be cached locally.|
|timeFormat|string|Yes|The format of log time. For example, %Y/%m/%d %H:%M:%S.|
|logBeginRegex|string|Yes|The characteristics \(regular expression\) of the first log line, which is used to match with logs composed of multiple lines.|
|RegEx|string|Yes| The regular expression used for extracting logs.|
|key|array|Yes| The key generated after logs are extracted.|
|Filterkey|array|Yes|The key used for filtering logs. The log meets the requirements only when the key value matches the  regular expression specified in the corresponding filterRegex column.|
|filterRegex|array|Yes|The regular expression corresponding to each filterKey. The length of  filterRegex must be the same as that of filterKey.|
|topicFormat|string|No|Use a part of the log file path as the topic. For example, `/var/log/(. *).log`. The default value is none, which  indicates the topic is empty.|
|preserve|boolean|No|true indicates that the monitored directory never times out. false indicates that the timeout for monitored directory is 30 minutes. The default value is true.|
|preserveDepth|Integer|No| If preserve is set to false, specify the depth of the directories with no monitoring timeout. The maximum depth is 3.|

outputDetail content

|Attribute name|Type|Attribute name|Description|
|:-------------|:---|:-------------|:----------|
|logstoreName|string|Yes|The Logstore name.|

