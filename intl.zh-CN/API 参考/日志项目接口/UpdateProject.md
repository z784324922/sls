# UpdateProject {#reference_k4w_l4z_ghb .reference}

修改一个指定的Project 。

示例：

```
PUT /
```

## 请求语法 {#section_utz_y4z_ghb .section}

```
PUT / HTTP/1.1 
Authorization:  <AuthorizationString>
x-log-bodyrawsize: 0 
User-Agent:  <UserAgent>
x-log-apiversion: 0.6.0 
Host:  <Project Endpoint>
x-log-signaturemethod: hmac-sha1 
Date:  <GMT Date>
Content-Type: application/json 
Content-MD5:  <Content-MD5>
Content-Length:  <ContentLength>
Connection: Keep-Alive
```

## 请求参数 {#section_x22_4pz_ghb .section}

|属性名称|类型|是否必须|描述|
|----|--|----|--|
|projectName|string|是|Project 的名称，作为Header中Host的一部分。|
|description|string|否，默认为空字符串。|Project 描述。|

## 请求头 {#section_qts_vpz_ghb .section}

UpdateProject 接口无特有请求头，关于 Log Service API 的公共请求头请参考 [公共请求头](intl.zh-CN/API 参考/公共请求头.md)。

## 响应头 {#section_ncv_zpz_ghb .section}

UpdateProject 接口无特有响应头，关于 Log Service API 的公共响应头请参考 [公共请求头](intl.zh-CN/API 参考/公共请求头.md)。

## 响应元素 {#section_ell_bqz_ghb .section}

HTTP 状态码返回 200。

## 错误码 {#section_w2x_cqz_ghb .section}

除了返回 Log Service API 的 [通用错误码](intl.zh-CN/API 参考/通用错误码.md)，还可能返回如下特有错误码：

|HTTP状态码|ErrorCode|ErrorMessage|
|-------|---------|------------|
|404|ProjectNotExist|The Project does not exist : <Project\>.|
|400|ParameterInvalid|The body is not valid json string.|
|500|InternalServerError|Specified Server Error Message.|

## 示例 {#section_jvr_rqz_ghb .section}

**请求示例：**

```
PUT / HTTP/1.1 
Authorization: LOG <yourAccessKeyId>:<yourSignature> 
x-log-bodyrawsize: 0 
User-Agent: sls-java-sdk-v-0.6.1 
x-log-apiversion: 0.6.0 
Host: my-project-test.cn-shanghai.log.aliyuncs.com 
x-log-signaturemethod: hmac-sha1 
Date: Sun, 27 May 2018 07:43:26 GMT 
Content-Type: application/json 
Content-MD5: A7967D81EFF5E3CD447FB6D8DF294E20 
Content-Length: 40 
Connection: Keep-Alive 
{ 
  "description": "Description of my-project-test" 
}
```

**响应示例：**

```
HTTP/1.1 200 
Server: nginx 
Content-Length: 0 
Connection: close 
Access-Control-Allow-Origin: * 
Date: Sun, 27 May 2018 07:43:27 GMT 
x-log-requestid: 5B0A619F205DC3F30EDA9322
```

