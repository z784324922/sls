# Logtail configuration {#concept_f3n_s5q_12b .concept}

You can create up to 100 Logtail configurations for a project. The configuration name must be unique in a project.

You can use a Logtail configuration to specify the location, method, and parameters for log collection.

**Logtail configuration naming rules are as follows:**

-   The name can contain only lowercase letters, digits, hyphens \(-\), and underscores \(\_\).
-   The name must start and end with a lowercase letter or digit.
-   The name must be 3 to 63 bytes in length.

A configuration example is as follows:

```
{
    "configName": "logConfigName", 
    "outputType": "LogService", 
    "inputType": "file", 
    "inputDetail": {
        "logPath": "/logPath", 
        "filePattern": "access.log", 
        "logType": "json_log", 
        "topicFormat": "default", 
        "discardUnmatch": false, 
        "enableRawLog": true, 
        "fileEncoding": "gbk", 
        "maxDepth": 10
    }, 
    "outputDetail": {
        "projectName": "test-project", 
        "logstoreName": "test-logstore"
    }
}
```

The configuration in JSON mode is used as an example. For more information, see [Configuration examples](#).

## Logtail configuration parameters {#section_zvx_2sc_mgb .section}

|Parameter|Type|Required|Description|
|:--------|:---|:-------|:----------|
|configName|String|Yes|The name of the Logtail configuration, which must be unique in a project.|
|logSample|String|No|The log sample of the Logtail configuration.|
|inputType|String|Yes|The input type of the Logtail configuration. Valid values: `plugin` and `file`.|
|[inputDetail](#)|Object|Yes|The configuration of the input type.|
|outputType|String|Yes|The output type of the Logtail configuration. Valid value: `LogService`.|
|[outputDetail](#)|Object|Yes|The configuration of the output type.|
|createTime \(output-only\)|Int32|No|The time when the Logtail configuration was created. This parameter is returned by the server and cannot be set.|
|lastModifyTime \(output-only\)|Int32|No|The time when the Logtail configuration was last modified. This parameter is returned by the server and cannot be set.|

## Basic configuration of the inputDetail parameter {#section_tnc_b4c_mgb .section}

The following table describes the basic configuration of the inputDetail parameter for all input types.

|Parameter|Type|Required|Description|
|:--------|:---|:-------|:----------|
|filterKey|Array|No|The key used to filter logs. A log is collected only when the key value matches the regular expression specified by the corresponding filterRegex parameter.|
|filterRegex|Array|No|The regular expression corresponding to each filterKey. The length of filterRegex must be the same as that of filterKey.|
|shardHashKey|Array|No|The key that is specified when log data is written in KeyHash mode. Log data is written in [LoadBalance](reseller.en-US/API Reference/Logstore related APIs/PutLogs.md) mode by default. If you specify this parameter, log data is written in [KeyHash](reseller.en-US/API Reference/Logstore related APIs/PutLogs.md) mode. Valid values: `__topic__`, `__hostname__`, and `__source__`.|
|enableRawLog|Boolean|No|Specifies whether to upload the raw log.|
|[sensitive\_keys](#)|Array|No|The desensitization configuration, which is a `SensitiveKey` array. For more information about how to configure `SensitiveKey`, see the following table.|
|mergeType|String|No|The aggregation method. Logs are aggregated by topic by default. Valid values: `topic` and `logstore`.|
|delayAlarmBytes|Integer|No|The alert threshold for slippage on log collection. Default value: `209715200`, which indicates 200 MB.|
|adjustTimezone|Boolean|No|Specifies whether to modify the time zone of logs. This parameter is required only when time needs to be parsed.|
|logTimezone|String|No|The offset of the time zone. If the time zone of logs is UTC+8, set this parameter to `GMT+08:00`.|
|priority|Integer|No|The priority for sending logs. Default value: `0`. If you need to specify a Logtail configuration with a high priority, set this parameter to `1`.|

The following table describes how to configure **SensitiveKey**.

|Parameter|Type|Required|Description|
|:--------|:---|:-------|:----------|
|key|String|Yes|The name of the log key.|
|type|String|Yes|The desensitization method. Valid values: `const` and `md5`. If you set this parameter to `const`, the sensitive content is replaced with the value of the `const` parameter. If you set this parameter to `md5`, the sensitive content is replaced with the corresponding MD5 hash value.|
|regex\_begin|String|Yes|The prefix of the sensitive content.|
|regex\_content|String|Yes|The regular expression for the sensitive content.|
|all|Boolean|Yes|Specifies whether to replace all sensitive content in the field specified by the key parameter. We recommend that you set this parameter to `true`.|
|const|String|No|The content used to replace the sensitive content. This parameter is required if you set the `type` parameter to `const`.|

For example, a log contains the `content` field, whose value is `[{'account':'1812213231432969','password':'04a23f38'}, {'account':'1812213685634','password':'123a'}]`. To desensitize `password`, set `SensitiveKey` as follows:

```
"key" : "content"
"type" : "const"
"regex_begin" : "'password':'"
"regex_content" : "[^']*"
"all" : true
"const" : "********"

```

The log content after desensitization is as follows:

```
[{'account':'1812213231432969','password':'********'}, {'account':'1812213685634','password':'********'}]
```

## Configuration of the inputDetail parameter for text files {#section_fhz_c4c_mgb .section}

The following table describes the basic configuration of the inputDetail parameter for text files.

|Parameter|Type|Required|Description|
|:--------|:---|:-------|:----------|
|logType|String|Yes|The log collection mode. This parameter is required if you set the inputType parameter to file. Valid values: `json_log`, `apsara_log`, `common_reg_log`, and `delimiter_log`.|
|logPath|String|Yes|The parent directory of logs to be collected, for example, `/var/logs/`.|
|filePattern|String|Yes|The pattern of the log file, for example, `access*.log`.|
|topicFormat|String|Yes|The method for generating the topic of logs. Valid values: `none`: indicates that the topic is set to a null string. `default`: indicates that the log path is used as the topic. `group_topic`: indicates that the topic attribute of the machine group that applies the Logtail configuration is used as the topic. Another value: indicates that a part of the log path is used as the topic, for example, `/var/log/(.*).log`.|
|timeFormat|String|No|The time format of logs, for example, `%Y/%m/%d %H:%M:%S`.|
|preserve|Boolean|No|The timeout period of the monitored directory. A value of true indicates that the monitored directory never times out. A value of false indicates that the monitored directory is regarded as timing out if it is not updated within 30 minutes. Default value: true.|
|preserveDepth|Integer|No|The depth of the monitored directory if you set the preserve parameter to true. The maximum value is 3.|
|fileEncoding|String|No|The file encoding method. Valid values: `utf8` and `gbk`.|
|discardUnmatch|Boolean|No|Specifies whether to discard logs that fail to match the specified filter conditions.|
|maxDepth|Integer|No|The maximum depth of the monitored directory. Valid values: \[0, 1000\]. A value of 0 indicates that only the current directory is monitored.|
|delaySkipBytes|Integer|No|The discarding threshold for slippage on log collection. Default value: 0, which indicates that lagged data is not discarded. If the amount of lagged data exceeds the value of this parameter, the lagged data is discarded.|
|isDocherFile|Boolean|No|Specifies whether the log file is in a container. Default value: false. For more information, see [Container text logs](../../../../reseller.en-US/User Guide/Logtail collection/Container log collection/Container text logs.md).|
|dockerIncludeLabel|Object|No|The whitelist of container labels. Logtail collects logs from Docker containers whose labels are in the whitelist. If you do not specify this parameter, Logtail collects logs from all containers.|
|dockerExcludeLabel|Object|No|The blacklist of container labels. Logtail does not collect logs from Docker containers whose labels are in the blacklist. If you do not specify this parameter, Logtail collects logs from all containers.|
|dockerIncludeEnv|Object|No|The whitelist of container environment variables. Logtail collects logs from Docker containers whose environment variables are in the whitelist. If you do not specify this parameter, Logtail collects logs from all containers.|
|dockerExcludeEnv|Object|No|The blacklist of container environment variables. Logtail does not collect logs from Docker containers whose environment variables are in the blacklist. If you do not specify this parameter, Logtail collects logs from all containers.|

-   **Configuration in full regex mode or simple mode**

    The following table describes the specific configuration in full regex mode or simple mode.

    |Parameter|Type|Required|Description|
    |:--------|:---|:-------|:----------|
    |key|Array|Yes|The keys in the log extraction results.|
    |logBeginRegex|String|No|The regular expression used to specify the starting header of a cross-line log.|
    |regex|String|No|The regular expression used to extract log fields.|

    The `simple mode` is a special case of the full regex mode. The following example shows the configuration in `simple mode`:

    ```
    "key" : ["content"]
    "logBeginRegex" : ".*"
    "regex" : "(. *)"
    ```

-   **Configuration in JSON mode**

    The following table describes the specific configuration in JSON mode.

    |Parameter|Type|Required|Description|
    |:--------|:---|:-------|:----------|
    |timeKey|String|No|The key that specifies the time field.|

-   **Configuration in delimiter mode**

    The following table describes the specific configuration in delimiter mode.

    |Parameter|Type|Required|Description|
    |:--------|:---|:-------|:----------|
    |separator|String|No|The delimiter used to delimit the fields of each log.|
    |quote|String|Yes|The quote used to enclose log fields that contain the delimiter.|
    |key|Array|Yes|The list of keys in the log extraction results.|
    |timeKey|String|Yes|The key that specifies the time field, which must be in the key list.|
    |autoExtend|Boolean|No|Specifies whether to automatically increase the number of keys if the actual number of parsed log fields is greater than the number of configured keys.|

-   **Configuration in Apsara mode**

    The following table describes the specific configuration in Apsara mode.

    |Parameter|Type|Required|Description|
    |:--------|:---|:-------|:----------|
    |logBeginRegex|String|No|The regular expression used to specify the starting header of a cross-line log.|


## Configuration of the outputDetail parameter {#section_sjv_3pc_mgb .section}

The following table describes how to configure the project and Logstore for output logs.

|Parameter|Type|Required|Description|
|:--------|:---|:-------|:----------|
|projectName|String|Yes|The name of the project, which must be the name as the requested project. Otherwise, an error is returned.|
|logstoreName|String|Yes|The name of the Logstore.|

## Configuration examples {#section_udh_xrc_mgb .section}

-   JSON mode:

```
{
    "configName": "logConfigName", 
    "outputType": "LogService", 
    "inputType": "file", 
    "inputDetail": {
        "logPath": "/logPath", 
        "filePattern": "access.log", 
        "logType": "json_log", 
        "topicFormat": "default", 
        "discardUnmatch": false, 
        "enableRawLog": true, 
        "fileEncoding": "gbk", 
        "maxDepth": 10
    }, 
    "outputDetail": {
        "projectName": "test-project", 
        "logstoreName": "test-logstore"
    }
}
```

-   Delimiter mode:

```
{
    "configName": "logConfigName", 
    "logSample": "testlog", 
    "inputType": "file", 
    "outputType": "LogService", 
    "inputDetail": {
        "logPath": "/logPath", 
        "filePattern": "*", 
        "logType": "delimiter_log", 
        "topicFormat": "default", 
        "discardUnmatch": true, 
        "enableRawLog": true, 
        "fileEncoding": "utf8", 
        "maxDepth": 999, 
        "separator": ",", 
        "quote": "\"", 
        "key": [
            "test", 
            "test2"
        ], 
        "autoExtend": true
    }, 
    "outputDetail": {
        "projectName": "test-project", 
        "logstoreName": "test-logstore"
    }
}
```

-   Full regex mode:

```
{
    "configName": "logConfigName", 
    "outputType": "LogService", 
    "inputType": "file", 
    "inputDetail": {
        "logPath": "/logPath", 
        "filePattern": "*", 
        "logType": "common_reg_log", 
        "topicFormat": "default", 
        "discardUnmatch": false, 
        "enableRawLog": true, 
        "fileEncoding": "utf8", 
        "maxDepth": 10, 
        "key": [
            "content"
        ], 
        "logBeginRegex": ".*", 
        "regex": "(.*)"
    }, 
    "outputDetail": {
        "projectName": "test-project", 
        "logstoreName": "test-logstore"
    }
}
```

-   Plug-in mode \(Docker standard output\):

```
{
    "configName": "logConfigName", 
    "outputType": "LogService", 
    "inputType": "plugin",
    "inputDetail": {
        "plugin": {
            "inputs": [
                {
                    "detail": {
                        "ExcludeEnv": null, 
                        "ExcludeLabel": null, 
                        "IncludeEnv": null, 
                        "IncludeLabel": null, 
                        "Stderr": true, 
                        "Stdout": true
                    }, 
                    "type": "service_docker_stdout"
                }
            ]
        }
    }, 
    "outputDetail": {
        "projectName": "test-project", 
        "logstoreName": "test-logstore"
    }
}
```


