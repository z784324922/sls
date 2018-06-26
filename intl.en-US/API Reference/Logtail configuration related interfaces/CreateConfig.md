# CreateConfig {#reference_ktw_g5q_12b .reference}

Create a Logtail configuration in a project.

Example:

```
POST /configs
```

## Request syntax {#section_j5v_14t_12b .section}

```
POST /configs HTTP/1.1
Authorization: <AuthorizationString> 
Content-Type:application/json
Content-Length:<Content Length>
Content-MD5<:<Content MD5>
Date: <GMT Date>
Host: <Project Endpoint>
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
{
    "configName": "testcategory1",
    "inputType": "file",
    "inputDetail": {
        "logType": "common_reg_log",
        "logPath": "/var/log/httpd/",
        "filePattern": "access*.log",
        "localStorage": true,
        "timeFormat": "%Y/%m/%d %H:%M:%S",
        "logBeginRegex": ".*",
        "regex": "(\w+)(\s+)",
        "key" :["key1", "key2"],
        "filterKey":["key1"],
        "filterRegex":["regex1"],
        "fileEncoding":"utf8",
        "topicFormat": "none"
    },
    "outputType": "LogService",
    "outputDetail": 
    {
        "logstoreName": "perfcounter"
    }
}
```

## Request Parameters {#section_mx5_b4t_12b .section}

|Parameter name|Type |Required|Description |
|:-------------|:----|:-------|:-----------|
|configName|string|Yes|The Logtail configuration name, which is unique in the same project.|
|inputType|string|Yes|The input type. Currently, only file is supported.|
|inputDetail|json|Yes|See the descriptions in the following table.|
|outputType|string|Yes|The output type. Currently, only LogService is supported.|
|outputDetail|json|Yes| See the descriptions in the following table.|
|logSample|string|No |The log sample of the Logtail configuration. The log size cannot exceed 1,000 bytes.|

inputDetail contents

|Attribute name|Type|Required|Description|
|:-------------|:---|:-------|:----------|
|logType|string|Yes| The log type. Currently, only common\_reg\_log is supported.|
|LogPath |string|Yes|The parent directory where the log resides. For example, `/var/logs/`.|
|filePattern|string|YesYes.|The pattern of the log file, such as `access*.log`.|
|Localstorage|boolean |Yes|Whether or not to activate the local cache. Logs of 1 GB can be cached locally when the link to Log Service is disconnected.|
|timeFormat|string|Yes|The format of log time. For example, %Y/%m/%d %H:%M:%S.|
|logBeginRegex|string|Yse|The characteristics \(regular expression\) of the first log line, which is used to match with logs composed of multiple lines.|
|regex|string|Yes|The regular expression used for extracting logs.|
|key |array |Yes|The key generated after logs are extracted.|
|filterKey|array|Yes|The key used for filtering logs. The log meets the requirements only when the key value matches the regular expression specified in the corresponding filterRegex column.|
|filterRegex|array|Yes|The regular expression corresponding to each filterKey. The length of filterRegex must be the same as that of filterKey.|
|topicFormat|string|No|The topic generation mode. The four supported modes are as follows:-   Use a part of the log file path as the topic. For example, /`/var/log/(. *).log`.
-   none indicates the topic is empty.
-   default indicates to use the log file path as the topic.
-   group\_topic indicates to use the topic attribute of the machine group that applies this configuration as the topic.

|
|preserve|boolean|No|true indicates that the monitored directory never times out. false indicates that the timeout for monitored directory is 30 minutes. The default value is true.|
|preserveDepth|integer|No|If preserve is set to false, specify the depth of the directories with no monitoring timeout. The maximum depth is 3.|
|fileEncoding|string|No|Two types are supported: utf8 and gbk.|

outputDetail content

|Attribute name|Type|Required|Description|
|:-------------|:---|:-------|:----------|
|logstoreName|string|Yes|The Logstore name.|

**Request Header**

The CreateConfig API does not have a special request header.  For more information about the public request headers of Log Service APIs, see [Public request header](intl.en-US/API Reference/Public request header.md).

**Response header**

The CreateConfig API does not have a special response header. For more information about the public response headers of Log Service APIs, see [Public response header](intl.en-US/API Reference/Public response header.md).

**Response Element**

The returned HTTP status code is 200.

**Error code**

Besides the  [common error codes](intl.en-US/API Reference/Common error codes.md) of Log Service APIs, the CreateConfig API may return the following special error codes.

|HTTP status code |ErrorCode |ErrorMessage|
|:----------------|:---------|:-----------|
|400|ConfigAlreadyExist|config \{Configname\} already exists|
|400|InvalidParameter|invalid config resource json|
|500|InternalServerError|internal server error|

**Detailed description**

The configuration fails to be created if an error occurs during the creation, for example, the configuration already exists, the format is incorrect, the required parameters are missing, or the quota is exceeded.

## Example  {#section_e5p_d4t_12b .section}

**Request example**

```
POST /configs HTTP/1.1
Header :
{
    'Content-Length': 737, 
    'Host': 'ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com', 
    'x-log-bodyrawsize': 737, 
    'Content-MD5': 'FBA01ECF7255BE143379BC70C56BBF68', 
    'x-log-signaturemethod': 'hmac-sha1', 
    'Date': 'Mon, 09 Nov 2015 07:45:30 GMT', 
    'x-log-apiversion': '0.6.0', 
    'User-Agent': 'log-python-sdk-v-0.6.0', 
    'Content-Type': 'application/json', 
    'Authorization': 'LOG 94to3z418yupi6ikawqqd370:x/L1ymdn9wxe2zrwzcdSG82nXL0='
}
Body:
{
    "configName": "sample-logtail-config",
    "inputType": "file",
    "inputDetail": {
        "logType": "common_reg_log", 
        "logPath": "/var/log/httpd/",
        "filePattern": "access*.log",
        "localStorage": true, 
        "timeFormat": "%d/%b/%Y:%H:%M:%S", 
        "logBeginRegex": "\\d+\\.\\d+\\.\\d+\\.\\d+ - .*", 
        "regex": "([\\d\\.] +) \\S+ \\S+ \\[(\\S+) \\S+\\] \"(\\w+) ([^\"]*)\" ([\\d\\.]+) (\\d+) (\\d+) (\\d+|-) \"([^\"]*)\" \"([^\"]*)\".*", 
        "key": ["ip", "time", "method", "url", "request_time", "request_length", "status", "length", "ref_url", "browser"], 
        "filterKey": [], 
        "filterRegex": [],
        "topicFormat": "none", 
        "fileEncoding": "utf8"
    }, 
    "outputType": "LogService", 
    "outputDetail": 
    {
        "logstoreName": "sls-test-logstore"
    }
}
```

**Response example:**

```
HTTP/1.1 200 OK
Header
{
    'date': 'Mon, 09 Nov 2015 07:45:30 GMT',
    'connection': 'close',
    'x-log-requestid': '56404F1A99248CA26C002180',
    'content-length': '0',
    'server': 'nginx/1.6.1'
}
```

