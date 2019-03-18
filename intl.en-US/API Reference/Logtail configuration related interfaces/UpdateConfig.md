# UpdateConfig {#reference_bkh_k5q_12b .reference}

Update the configuration. If the configuration is applied to a machine group, the corresponding machines are also updated.

Example:

```
PUT /configs/{configName}
```

## Request syntax {#section_j5v_14t_12b .section}

```
PUT /configs/<configName> HTTP/1.1
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
    "outputType": "LogService",
    "outputDetail": 
    {
        "logstoreName": "perfcounter"
    }
}
```

## Request Parameters {#section_mx5_b4t_12b .section}

|Attribute name |Type |Required|Description |
|:--------------|:----|:-------|:-----------|
|configName|string |Yes |The Logtail configuration name, which is unique in the same project.|
|inputType|string|Yes|The input type. Currently, only file is supported.|
|inputDetail|json|Yes|The output type. Currently, only LogService is supported.|
|outputType|string|Yes|The output type. Currently, only LogService is supported.|
|outputDetail|JSON|Yes|See the descriptions in the following table.|

inputDetail contents

|Attribute name|Type|Required|Description|
|:-------------|:---|:-------|:----------|
|Logtype|String|Yes|The log type. Currently, only common\_reg\_log is supported.|
|logPath|string|Yes|The parent directory where the log resides. For example, `/var/logs/`.|
|filePattern|string|Yes|The pattern of a log file. For example, `access*.log`.|
|localStorage|boolean|Yes|Whether or not to activate the local cache. Logs of 1 GB can be cached locally when the link to Log Service is disconnected.|
|timeFormat|string|Yes|The format of log time. For example, %Y/%m/%d %H:%M:%S.|
|logBeginRegex|string|Yes|The characteristics \(regular expression\) of the first log line, which is used to match with logs composed of multiple lines.|
|regex|string|Yes|The regular expression used for extracting logs.|
|key |array |Yes|The key generated after logs are extracted.|
|filterKey|array|Yes|The key used for filtering logs. The log meets the requirements only when the key value matches the regular expression specified in the corresponding filterRegex column.|
|filterRegex|array|Yes|The regular expression corresponding to each filterKey. The length of filterRegex must be the same as that of filterKey.|
|topicFormat|string|No|Use a part of the log file path as the topic. For example, `/var/log/(. *).log`. The default value is none, which indicates the topic is  empty.|
|preserve|boolean|No|true indicates that the monitored directory never times out. false indicates that the timeout for monitored directory is 30 minutes. The default value is true.|
|preserveDepth|integer|No|If preserve is set to false, specify the depth of the directories with no monitoring timeout. The maximum depth is 3.|
|fileEncoding|string|No|Two types are supported: utf8 and gbk. The default value is utf8.|

outputDetail content

|Attribute name|Type|Required|Description|
|:-------------|:---|:-------|:----------|
|logstoreName|string|Yes|The Logstore name.|

**Request Header**

The UpdateConfig API does not have a special request header. The UpdateConfig API does not have a special request header.

**Response header**

The UpdateConfig API does not have a special response header.  For more information about the public response headers of Log Service APIs, see [Public response header](reseller.en-US/API Reference/Public response header.md).

**Response Element**

The returned HTTP status code is 200.

**Error code**

Besides the  [common error codes](reseller.en-US/API Reference/Common error codes.md) of Log Service APIs, the UpdateConfig API may return the following special error codes.

|HTTP status code |Error Code|Error Message|
|:----------------|:---------|:------------|
|404|ConfigNotExist|config \{Configname\} does not exist|
|400|InvalidParameter|invalid config resource json|
|400|BadRequest|config resource configname does not match with request|
|500|InternalServerError|internal server error|

**Detailed description**

The configuration fails to be created if an error occurs during the creation, for example, the format is incorrect, the required parameters are missing, or the quota is exceeded.

## Example  {#section_e5p_d4t_12b .section}

**Request example:**

```
PUT /configs/logtail-config-sample
Header : 
{
    "Content-Length": 737,
    "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com",
    "x-log-bodyrawsize": 737,
    "Content-MD5": "431263EB105D584A5555762A81E869C0",
    "x-log-signaturemethod": "hmac-sha1",
    "Date": "Mon, 09 Nov 2015 09:14:32 GMT",
    "x-log-apiversion": "0.6.0",
    "User-Agent": "log-python-sdk-v-0.6.0",
    "Content-Type": "application/json", 
    "Authorization": "LOG AK\_ID:Signature"
}
Body :
{
    "outputDetail": {
        "logstoreName": "sls-test-logstore"
    }, 
    "inputType": "file", 
    "inputDetail": {
        "regex": "([\\d\\.] +) \\S+ \\S+ \\[(\\S+) \\S+\\] \"(\\w+) ([^\"]*)\" ([\\d\\.]+) (\\d+) (\\d+) (\\d+|-) \"([^\"]*)\" \"([^\"]*)\".*", 
        "filterKey": [], 
        "logPath": "/var/log/nginx/", 
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
    "outputType": "LogService",
    "configName": "logtail-config-sample"
}
```

**Response example**

```
{
    "date": "Mon, 09 Nov 2015 09:14:32 GMT",
    "connection": "close",
    "x-log-requestid": "564063F899248CAA2300B778",
    "content-length": "0",
    "server": "nginx/1.6.1"
}
```

