# DeleteIndex {#reference_ivq_pvz_c2b .reference}

Deletes an index for a specified Logstore.

Example:

```
DELETE /logstores/{logstoreName}/index
```

## Request syntax {#section_o54_dhh_f2b .section}

```
DELETE /logstores/<logstoreName>/index HTTP/1.1

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

## Request parameters: {#section_lvh_2hh_f2b .section}

|**Attribute name**|**Type**|**Required or not**|**Description**|
|:-----------------|:-------|:------------------|:--------------|
|logstoreName|string|Yes|Logstore name.|

 **Request header** 

The DeleteIndex interface does not have a specific request header. For more information, see [Public request header](reseller.en-US/API Reference/Public request header.md).

 **Response header** 

The DeleteIndex interface does not have a specific response header. For more information, see [Public response header](reseller.en-US/API Reference/Public response header.md).

 **Response element** 

The returned HTTP status code is 200.

 **Error code** 

The interface may return the following error codes in addition to Log Service API [Common error codes](reseller.en-US/API Reference/Common error codes.md):

|HTTP status code|Error code|Error message|
|:---------------|:---------|:------------|
|400|ParameterInvalid|log store index not created|
|404|ProjectNotExist|The Project does not exist : \{Project\}|
|404|LogStoreNotExist|logstore \{logstoreName\} dose not exist|
|500|InternalServerError|Specified Server Error Message|

## Example {#section_p5z_ghh_f2b .section}

**Request example** 

```
DELETE /logstores/my-logstore/index HTTP/1.1

Authorization: LOG <yourAccessKeyId>:<yourSignature>
x-log-bodyrawsize: 0
User-Agent: sls-java-sdk-v-0.6.1
x-log-apiversion: 0.6.0
Host: my-project.cn-shanghai.log.aliyuncs.com
x-log-signaturemethod: hmac-sha1
Date: Sun, 06 May 2018 13:22:57 GMT
Content-Type: application/x-protobuf
Connection: Keep-Alive
```

**Response example:** 

```
HTTP/1.1 200

Server: nginx/1.12.1
Content-Length: 0
Connection: close
Access-Control-Allow-Origin: *
Date: Sun, 06 May 2018 13:22:57 GMT
x-log-requestid: 5AEF01B1A458E41C2F8326A8
```

