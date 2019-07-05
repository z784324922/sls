# GetProject {#reference_wdt_tgh_f2b .reference}

GetProject 根据Project 名称查询 Project。

示例：

``` {#codeblock_qwx_np1_875}
GET /
```

## 请求语法 {#section_o54_dhh_f2b .section}

``` {#codeblock_fy2_7gs_lny}
GET / HTTP/1.1
Authorization: <AuthorizationString>
x-log-bodyrawsize: 0
User-Agent: <UserAgent>
x-log-apiversion: 0.6.0
Host: <Project Endpoint>
x-log-signaturemethod: hmac-sha1
Date: <GMT Date>
Content-Type: application/x-protobuf
Connection: Keep-Alive
```

## 请求参数 {#section_lvh_2hh_f2b .section}

|属性名称|类型|是否必须|描述|
|:---|:-|:---|:-|
|projectName|string|是|Project 的名称，作为Header中Host的一部分。|

 **请求头** 

GetProject接口无特有请求头，关于 Log Service API 的公共请求头，请参考[公共请求头](intl.zh-CN/API 参考/公共请求头.md)。

 **响应头** 

GetProject接口无特有响应头，关于 Log Service API 的公共响应头，请参考[公共响应头](intl.zh-CN/API 参考/公共响应头.md)。

 **响应元素** 

GetProject 请求成功，其响应 Body 中包含 Project 的详细信息，具体格式为：

|属性名称|类型|描述|
|:---|:-|:-|
|createTime|string|创建时间。|
|description|string|Project 描述|
|lastModifyTime|string|最后一次更新时间。|
|owner|string|创建人的账户Id。|
|projectName|string|Project名称。|
|status|string|Project 状态。|
|region|string|Project 所属的区域。|

 **错误码** 

除了返回 Log Service API 的[通用错误码](intl.zh-CN/API 参考/通用错误码.md)，还可能返回如下特有错误码：

|HTTP状态码|ErrorCode|ErrorMessage|
|:------|:--------|:-----------|
|404|ProjectNotExist|The Project does not exist : \{Project\}|
|500|InternalServerError|Specified Server Error Message|

## 示例 {#section_p5z_ghh_f2b .section}

**请求示例：** 

``` {#codeblock_xtv_ge1_tl6}
GET / HTTP/1.1
Authorization: LOG <yourAccessKeyId>:<yourSignature>
x-log-bodyrawsize: 0
User-Agent: sls-java-sdk-v-0.6.1
x-log-apiversion: 0.6.0
Host: my-project-test.cn-shanghai.log.aliyuncs.com
x-log-signaturemethod: hmac-sha1
Date: Sun, 27 May 2018 08:25:04 GMT
Content-Type: application/x-protobuf
Connection: Keep-Alive
```

**响应示例：** 

``` {#codeblock_096_ssq_6e4}
HTTP/1.1 200
Server: nginx
Content-Length: 0
Connection: close
Access-Control-Allow-Origin: *
Date: Sun, 27 May 2018 08:25:04 GMT
x-log-requestid: 5B0A6B60BB6EE39764D458B5
```

