# GetConfig {#reference_t52_j5q_12b .reference}

Obtain the configuration details.

Example:

```
GET /configs/{configName}
```

## Request syntax {#section_j5v_14t_12b .section}

```
GET /configs/<configName> HTTP/1.1
Authorization: <AuthorizationString> 
Date: <GMT Date>
Host: <Project Endpoint>
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

## Request Parameters {#section_mx5_b4t_12b .section}

|Parameter Name|Type |Required|Description |
|:-------------|:----|:-------|:-----------|
|ConfigName|String |Yes |The Logtail configuration name.|

**Request Header**

The GetConfig API does not have a special request header.  For more information about the public request headers of Log Service APIs, see [Public request header](intl.en-US/API Reference/Public request header.md).

**Response header**

The GetConfig API does not have a special response header. For more information about the public response headers of Log Service APIs, see [Public response header](intl.en-US/API Reference/Public response header.md).

**Response Element**

|Attribute name |Type|Description|
|:--------------|:---|:----------|
|configName|string |The Logtail configuration name, which is unique in the same project.|
|Inputtype|string|The input type. Currently, only file is supported.|
|inputDetail|json|See the descriptions in the following table.|
|outputType|string|The output type. Currently, only LogService is supported.|
|outputDetail|json|See the descriptions in the following table.|
|createTime|Int | The created time of the configuration.|
|lastModifyTime|Int|The resource updated time in Log Service.|

inputDetail contents

|Attribute name|Type|Description|
|:-------------|:---|:----------|
|Logtype|String|The log type. Currently, only common\_reg\_log is supported.|
|LogPath |string| The parent directory where the log resides. For example,  `/var/logs/`.|
|filePattern|String|The pattern of a log file. For example, `access*.log`.|
|localStorage|boolean |Whether or not to activate the local cache. Logs of 1 GB can be cached locally when the link to Log Service is disconnected.|
|timeFormat|string|The format of log time. For example, `%Y/%m/%d %H:%M:%S`.|
|logBeginRegex|string|The characteristics \(regular expression\) of the first log line, which is used to match with logs composed of multiple lines.|
|regex|string| The regular expression used for extracting logs.|
|key|array| The key generated after logs are extracted.|
|filterKey|Array|The key used for filtering logs. The log meets the requirements only when the key value matches the regular expression specified in the corresponding filterRegex column.|
|filterRegex|array|The regular expression corresponding to each filterKey. The length of filterRegex must be the same as that of filterKey.|
|topicFormat|string|Use a part of the log file path as the topic. For example, `/var/log/(. *).log`. The default value is none, which indicates the  topic is empty.|
|preserve|boolean|true indicates that the monitored directory never times out. false indicates that the timeout for monitored directory is 30 minutes. The default value is true.|
|preserveDepth|integer| If preserve is set to false, specify the depth of the directories with no monitoring timeout. The maximum depth is 3.|
|fileEncoding|string| The encoding format of the log file, which supports utf8 and gbk.|

outputDetail content

|Attribute name|Type|Required|Description|
|:-------------|:---|:-------|:----------|
|endpoint|String|Yes| The access address of the region where the project resides.|
|logstoreName|string|Yes|The Logstore name.|

**Error code**

Besides the  [common error codes](intl.en-US/API Reference/Common error codes.md)of Log Service APIs, the GetConfig API may return the following special error codes.

|HTTP status code|Error Code|Error Message|
|:---------------|:---------|:------------|
|404|ConfigNotExist|Config \{configname\} does not exist|
|500|InternalServerError|Specified Server Error Message|

## Example {#section_e5p_d4t_12b .section}

**Request example:**

```
GET /configs/logtail-config-sample 
Header :
{
    "Content-Length": 0,
    "x-log-signaturemethod": "hmac-sha1",
    "x-log-bodyrawsize": 0,
    "User-Agent": "log-python-sdk-v-0.6.0",
    "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com",
    "Date": "Mon, 09 Nov 2015 08:29:15 GMT",
    "x-log-apiversion": "0.6.0",
    "Authorization": "LOG 94to3z418yupi6ikawqqd370:yV5LsYLmn1UrAXvBg8CbZNZoiTk="
}
```

**Response example**

```
Header :
{   
    "content-length": "730",
    "server": "nginx/1.6.1",
    "connection": "close",
    "date": "Mon, 09 Nov 2015 08:29:15 GMT",
    "content-type": "application/json",
    "x-log-requestid": "5640595B99248CAA23004A59"
}
Body :
{   
    "configName": "logtail-config-sample",
    "outputDetail": {
        "endpoint": "http://cn-hangzhou-devcommon-intranet.sls.aliyuncs.com",
        "logstoreName": "sls-test-logstore"
    },
    "outputType": "LogService",
    "inputType": "file",
    "inputDetail": {
        "regex": "([\\d\\.] +) \\S+ \\S+ \\[(\\S+) \\S+\\] \"(\\w+) ([^\"]*)\" ([\\d\\.]+) (\\d+) (\\d+) (\\d+|-) \"([^\"]*)\" \"([^\"]*)\".*",
        "filterKey": [],
        "logPath": "/var/log/httpd/",
        "logBeginRegex": "\\d+\\.\\d+\\.\\d+\\.\\d+ - .*",
        "logType": "common_reg_log",
        "topicFormat": "none",
        "localStorage": true,
        "key": [
            "ip",
            "time",
            "method",
            "url",
            "request_time",
            "request_length",
            "status",
            "length",
            "ref_url",
            "browser"
        ],
        "filePattern": "access*.log",
        "timeFormat": "%d/%b/%Y:%H:%M:%S",
        "filterRegex": []
    },
    "createTime": 1447040456,
    "lastModifyTime": 1447050456
}
```

