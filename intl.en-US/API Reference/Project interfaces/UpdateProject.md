# UpdateProject {#reference_k4w_l4z_ghb .reference}

Updates a Project.

## Request syntax {#section_utz_y4z_ghb .section}

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

## Request parameters {#section_x22_4pz_ghb .section}

|Parameter|Type|Required?|Description|
|---------|----|---------|-----------|
|projectName|String|Yes|The Project name. It is part of the host field in a header.|
|description|String|No.|The project description. By default, this parameter is an empty string.|

## Request header {#section_qts_vpz_ghb .section}

The UpdateProject interface does not have a special request header. For more information, see [Public request header](reseller.en-US/API Reference/Public request header.md).

## Response header {#section_ncv_zpz_ghb .section}

The UpdateProject interface does not have a special response header. For more information, see [Public response header](reseller.en-US/API Reference/Public response header.md#).

## Response elements {#section_ell_bqz_ghb .section}

The returned HTTP status code is 200.

## Error codes {#section_w2x_cqz_ghb .section}

In addition to the [Common error codes](reseller.en-US/API Reference/Common error codes.md) of Log Service, the following error codes that are exclusive to the UpdateProject interface may be returned.

|HTTP status code|Error code|Error message|Description|
|----------------|----------|-------------|-----------|
|404|ProjectNotExist|The Project does not exist : <Project\>.|The specified Project does not exist.|
|400|ParameterInvalid|The body is not valid json string.|The parameter is not a valid JSON string.|
|500|InternalServerError|Specified Server Error Message.|An error occurred to the specified server.|

## Examples {#section_jvr_rqz_ghb .section}

**Request example** 

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

**Response example** 

```
HTTP/1.1 200 
Server: nginx 
Content-Length: 0 
Connection: close 
Access-Control-Allow-Origin: * 
Date: Sun, 27 May 2018 07:43:27 GMT 
x-log-requestid: 5B0A619F205DC3F30EDA9322
```

