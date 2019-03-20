# UpdateConfig {#reference_bkh_k5q_12b .reference}

更新配置内容，如果配置被应用到机器组，对应机器也会同时更新。

示例：

```
PUT /configs/{configName}
```

## 请求语法 {#section_j5v_14t_12b .section}

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

## 请求参数 {#section_mx5_b4t_12b .section}

**说明：** 请求参数和各种模式的Logtail配置样例请参考[Logtail配置](intl.zh-CN/API 参考/公共资源说明/Logtail配置.md)。

**请求头**

无特有请求头。关于 Log Service API 的公共请求头，请参考 [公共请求头](intl.zh-CN/API 参考/公共请求头.md)。

**响应头**

无特有响应头。请参考 [公共响应头](intl.zh-CN/API 参考/公共响应头.md)。

**响应元素**

返回值：成功返回 200 状态码。

**错误码**

除了返回 Log Service API 的 [通用错误码](intl.zh-CN/API 参考/通用错误码.md)，还可能返回如下特有错误码：

|HTTP 状态码|ErrorCode|ErrorMessage|
|:-------|:--------|:-----------|
|404|ConfigNotExist|config \{Configname\} does not exist|
|400|InvalidParameter|invalid config resource json|
|400|BadRequest|config resource configname does not match with request|
|500|InternalServerError|internal server error|

**细节描述**

创建过程中遇到格式错误、必要参数遗失、quota 超过限制等错误，则创建失败。

## 示例 {#section_e5p_d4t_12b .section}

**请求示例：**

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
    "Authorization": "LOG <yourAccessKeyId>:<yourSignature>"
}
Body :
{
    "outputDetail": {
        "logstoreName": "sls-test-logstore"
    }, 
    "inputType": "file", 
    "inputDetail": {
        "regex": "([\\d\\.]+) \\S+ \\S+ \\[(\\S+) \\S+\\] \"(\\w+) ([^\"]*)\" ([\\d\\.]+) (\\d+) (\\d+) (\\d+|-) \"([^\"]*)\" \"([^\"]*)\".*", 
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

**响应示例：**

```
{
    "date": "Mon, 09 Nov 2015 09:14:32 GMT",
    "connection": "close",
    "x-log-requestid": "564063F899248CAA2300B778",
    "content-length": "0",
    "server": "nginx/1.6.1"
}
```

