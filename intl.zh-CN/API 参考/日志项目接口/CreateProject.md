# CreateProject {#reference_yc2_pgh_f2p .reference}

创建一个 Project。

示例：

``` {#codeblock_c57_an6_zoc}
POST /
```

## 请求语法 {#section_o54_dhh_f2b .section}

``` {#codeblock_llb_t4j_ucj}
POST / HTTP/1.1
Authorization: <AuthorizationString> 
x-log-bodyrawsize: 0
User-Agent: <UserAgent>
x-log-apiversion: 0.6.0
Host: <Project Endpoint>
x-log-signaturemethod: hmac-sha1
Date: <GMT Date>
Content-Type: application/json
Content-MD5: <Content-MD5>
Content-Length: <ContentLength>
Connection: Keep-Alive
{
  "projectName": <ProjectName>,
  "description": <Description>
}
```

## 请求参数 {#section_lvh_2hh_f2b .section}

|属性名称|类型|是否必须|描述|
|:---|:-|:---|:-|
|projectName|string|是|Project 名称。|
|description|string|是|Project 描述。|

 **请求头** 

CreateProject接口无特有请求头，关于 Log Service API 的公共请求头，请参考[公共请求头](intl.zh-CN/API 参考/公共请求头.md)。

 **响应头** 

CreateProject接口无特有响应头，关于 Log Service API 的公共响应头，请参考[公共响应头](intl.zh-CN/API 参考/公共响应头.md)。

 **响应元素** 

HTTP 状态码返回 200。

 **错误码** 

除了返回 Log Service API 的[通用错误码](intl.zh-CN/API 参考/通用错误码.md)，还可能返回如下特有错误码：

|HTTP状态码|ErrorCode|ErrorMessage|
|:------|:--------|:-----------|
|400|ProjectAlreadyExist|Project \{project\} already exist|
|400|ParameterInvalid|The body is not valid json string|
|500|InternalServerError|Specified Server Error Message|

## 示例 {#section_p5z_ghh_f2b .section}

**请求示例：** 

``` {#codeblock_8ec_cot_dxk}
POST / HTTP/1.1
Authorization: LOG <yourAccessKeyId>:<yourSignature>
x-log-bodyrawsize: 0
User-Agent: sls-java-sdk-v-0.6.1
x-log-apiversion: 0.6.0
Host: my-project-test.cn-shanghai.log.aliyuncs.com
x-log-signaturemethod: hmac-sha1
Date: Sun, 27 May 2018 07:43:26 GMT
Content-Type: application/json
Content-MD5: A7967D81EFF5E3CD447FB6D8DF294E20
Content-Length: 80
Connection: Keep-Alive
{
  "projectName": "my-project-test",
  "description": "Description of my-project-test"
}
```

**响应示例：** 

``` {#codeblock_fj7_ak8_yrg}
HTTP/1.1 200
Server: nginx
Content-Length: 0
Connection: close
Access-Control-Allow-Origin: *
Date: Sun, 27 May 2018 07:43:27 GMT
x-log-requestid: 5B0A619F205DC3F30EDA9322
```

