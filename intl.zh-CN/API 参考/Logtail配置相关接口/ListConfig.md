# ListConfig {#reference_elm_h5q_12b .reference}

列出 Project 下所有配置信息，可以通过参数进行翻页。

示例：

```
GET /configs?offset=1&size=100
```

## 请求语法 {#section_j5v_14t_12b .section}

```
GET /configs?offset=0&size=100 HTTP/1.1
Authorization: <AuthorizationString> 
Date: <GMT Date>
Host: <Project Endpoint>
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

## 请求参数 {#section_mx5_b4t_12b .section}

URL 参数如下：

|参数名称|类型|是否必须|描述|
|:---|:-|:---|:-|
|offset\(optional\)|integer|否|返回记录的起始位置，默认为 0|
|size\(optional\)|integer|否|每页返回最大条目，默认为 500（最大值）|

**请求头**

无特有请求头。关于 Log Service API 的公共请求头，请参考 [公共请求头](intl.zh-CN/API 参考/公共请求头.md)。

**响应头**

无特有响应头。请参考 [公共响应头](intl.zh-CN/API 参考/公共响应头.md)。

**响应元素**

返回值：Body 包含该 project 下所有 config 列表，具体格式如下：

|名称|类型|描述|
|:-|:-|:-|
|count|整型|返回的 config 数目|
|total|整型|在服务端 config 总数|
|configs|字符串数组|返回的 config 名称列表|

**错误码**

除了返回 Log Service API 的 [通用错误码](intl.zh-CN/API 参考/通用错误码.md)，还可能返回如下特有错误码：

|HTTP 状态码|ErrorCode|ErrorMessage|
|:-------|:--------|:-----------|
|404|ConfigNotExist|config \{Configname\} does not exist|
|500|InternalServerError|internal server error|

## 示例 {#section_e5p_d4t_12b .section}

**请求示例：**

```
GET /configs?offset=0&size=10 HTTP/1.1
Header :
{
    "Content-Length": 0, 
    "x-log-signaturemethod": "hmac-sha1", 
    "x-log-bodyrawsize": 0, 
    "User-Agent": "log-python-sdk-v-0.6.0", 
    "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com", 
    "Date": "Mon, 09 Nov 2015 09:19:13 GMT", 
    "x-log-apiversion": "0.6.0", 
    "Authorization": "LOG 94to3z418yupi6ikawqqd370:teWnMylnM4Toohhp9dfBECrEgac="
}
```

**响应示例：**

```
Header :
{
    "content-length": "103", 
    "server": "nginx/1.6.1", 
    "connection": "close", 
    "date": "Mon, 09 Nov 2015 09:19:13 GMT", 
    "content-type": "application/json", 
    "x-log-requestid": "5640651199248CAA2300C2BA"
}
Body:
{
    "count": 3, 
    "configs": 
    [
        "logtail-config-sample", 
        "logtail-config-sample-2", 
        "logtail-config-sample-3"
    ], 
    "total": 3
}
```

