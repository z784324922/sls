# GetConfig {#reference_t52_j5q_12b .reference}

获得一个配置的详细信息。

示例：

```
GET /configs/{configName}
```

## 请求语法 {#section_j5v_14t_12b .section}

```
GET /configs/<configName> HTTP/1.1
Authorization: <AuthorizationString> 
Date: <GMT Date>
Host: <Project Endpoint>
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

## 请求参数 {#section_mx5_b4t_12b .section}

|参数名称|类型|是否必须|描述|
|:---|:-|:---|:-|
|ConfigName|String|是|日志配置名称|

**请求头**

无特有请求头。关于 Log Service API 的公共请求头，请参考 [公共请求头](intl.zh-CN/API 参考/公共请求头.md)。

**响应头**

无特有响应头。请参考 [公共响应头](intl.zh-CN/API 参考/公共响应头.md)。

**响应元素**

**说明：** 响应内容中包含的参数信息和各种模式的L配g置样例请参考[Logtail配置](intl.zh-CN/API 参考/公共资源说明/Logtail配置.md)。

**错误码**

除了返回 Log Service API 的 [通用错误码](intl.zh-CN/API 参考/通用错误码.md)，还可能返回如下特有错误码：

|HTTP 状态码|ErrorCode|ErrorMessage|
|:-------|:--------|:-----------|
|404|ConfigNotExist|Config \{Configname\} does not exist|
|500|InternalServerError|Specified Server Error Message|

## 示例 {#section_e5p_d4t_12b .section}

**请求示例：**

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

**响应示例：**

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
        "regex": "([\\d\\.]+) \\S+ \\S+ \\[(\\S+) \\S+\\] \"(\\w+) ([^\"]*)\" ([\\d\\.]+) (\\d+) (\\d+) (\\d+|-) \"([^\"]*)\" \"([^\"]*)\".*",
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

