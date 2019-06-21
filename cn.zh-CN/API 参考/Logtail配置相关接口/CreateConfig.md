# CreateConfig {#reference_ktw_g5q_12b .reference}

在Project下创建日志配置。

示例：

``` {#codeblock_05o_cmy_woq}
POST /configs
```

## 请求语法 {#section_j5v_14t_12b .section}

``` {#codeblock_fes_57i_d5m}
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

## 请求参数 {#section_mx5_b4t_12b .section}

**说明：** 请求参数和各种模式的Logtail配置样例请参考[Logtail配置](intl.zh-CN/API 参考/公共资源说明/Logtail配置.md)。

 **请求头** 

无特有请求头。关于 Log Service API 的公共请求头，请参考 [公共请求头](intl.zh-CN/API 参考/公共请求头.md)。

 **响应头** 

无特有响应头。请参考 [公共响应头](intl.zh-CN/API 参考/公共响应头.md)。

 **响应元素** 

HTTP 状态码返回 200。

 **错误码** 

除了返回 Log Service API 的 [通用错误码](intl.zh-CN/API 参考/通用错误码.md)，还可能返回如下特有错误码：

|HTTP 状态码|ErrorCode|ErrorMessage|
|:-------|:--------|:-----------|
|400|ConfigAlreadyExist|config \{Configname\} already exists|
|400|InvalidParameter|invalid config resource json|
|500|InternalServerError|internal server error|

 **细节描述** 

创建过程中遇到配置已经存在、格式错误、必要参数遗失或者 quota 超过限制等错误，则会创建失败。

## 示例 {#section_e5p_d4t_12b .section}

**请求示例：** 

``` {#codeblock_6v4_rig_of0}
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
    'Authorization': 'LOG <yourAccessKeyId>:<yourSignature>'
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
        "regex": "([\\d\\.]+) \\S+ \\S+ \\[(\\S+) \\S+\\] \"(\\w+) ([^\"]*)\" ([\\d\\.]+) (\\d+) (\\d+) (\\d+|-) \"([^\"]*)\" \"([^\"]*)\".*", 
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

 **响应示例：** 

``` {#codeblock_nrx_3o2_x17}
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

