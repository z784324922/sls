# ListLogstore {#reference_cc1_wtq_12b .reference}

ListLogstores 接口列出指定 Project 下的所有 Logstore 的名称。

## 请求语法 {#section_j5v_14t_12b .section}

``` {#codeblock_6e1_51g_drn}
GET /logstores HTTP/1.1
Authorization: <AuthorizationString> 
Date: <GMT Date>
Host: <Project Endpoint>
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

## 请求参数 {#section_mx5_b4t_12b .section}

|参数名称|类型|是否必须|描述|
|:---|:-|:---|:-|
|offset\(optional\)|int|否|返回记录的起始位置，默认值为 1。|
|size\(optional\)|int|否|每页返回最大条目，默认 500（最大值）。|
|logstoreName|string|是|用于请求的 Logstore 名称（支持部分匹配）。|

 **请求头** 

ListLogstore接口无特有请求头。关于 Log Service API 的公共请求头，请参考 [公共请求头](intl.zh-CN/API 参考/公共请求头.md)。

 **响应头** 

ListLogstore接口无特有响应头。关于 Log Service API 的公共响应头，请参考 [公共响应头](intl.zh-CN/API 参考/公共响应头.md)。

 **响应元素** 

ListLogStores 请求成功，其响应 Body 会包括指定 Project 下的所有 Logstore 名称列表，具体格式如下：

|名称|类型|描述|
|:-|:-|:-|
|count|int|返回的 Logstore 数目。|
|total|int|Logstore 总数。|
|logstores|array|返回的 Logstore 名称列表。|

 **错误码** 

除了返回 Log Service API 的 [通用错误码](intl.zh-CN/API 参考/通用错误码.md)，还可能返回如下特有错误码：

|HTTP状态码|ErrorCode|ErrorMessage|
|:------|:--------|:-----------|
|404|ProjectNotExist|Project \{ProjectName\} does not exist|
|500|InternalServerError|Specified Server Error Message|
|400|ParameterInvalid|Invalid parameter size, \(0.6.0\]|
|400|InvalidLogStoreQuery|logstore Query is invalid|

## 示例 {#section_e5p_d4t_12b .section}

**请求示例：** 

``` {#codeblock_gyt_fq3_fmh}
GET /logstores HTTP/1.1
Header: 
{
x-log-apiversion=0.6.0, 
Authorization=LOG <yourAccessKeyId>:<yourSignature>, 
Host=ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com, 
Date=Wed, 11 Nov 2015 08:09:39 GMT, 
Content-Length=0, 
x-log-signaturemethod=hmac-sha1, 
User-Agent=sls-java-sdk-v-0.6.0, 
Content-Type=application/json
}
```

 **响应示例：** 

``` {#codeblock_s9q_x4j_gfu}
HTTP/1.1 200 OK
Header: 
{
Date=Wed, 11 Nov 2015 08:09:39 GMT, 
Content-Length=52, 
x-log-requestid=5642F7C399248C8D7B01342F, 
Connection=close, 
Content-Type=application/json, 
Server=nginx/1.6.1
}
Body:
{
"count":1,
"logstores":["test-logstore"],
"total":1
}
```

