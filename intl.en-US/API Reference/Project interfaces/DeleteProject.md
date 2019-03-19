# DeleteProject {#reference_et5_rgh_f2b .reference}

Delete a specified project.

Example:

```
DELETE /
```

## Request syntax {#section_o54_dhh_f2b .section}

```
DELETE / HTTP/1.1
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

## Request parameters {#section_lvh_2hh_f2b .section}

|Attribute name|Type |Required or not|Description|
|:-------------|:----|:--------------|:----------|
|projectName|string|Yes|Project name, which is a part of Host in the header.|

**Request header**

The DeleteProject interface does not have a specific request header. For details about public request headers of Log Service APIs, see [Public request header](reseller.en-US/API Reference/Public request header.md).

**Response header**

The DeleteProject interface does not have a specific response header. For details about public response headers of Log Service APIs, see [Public response header](reseller.en-US/API Reference/Public response header.md).

**Response element **

The returned HTTP status code is  200.

**Error code**

The interface may return the following error codes in addition to Log Service API [Common error codes](reseller.en-US/API Reference/Common error codes.md):

|HTTP status code|ErrorCode|ErrorMessage|
|:---------------|:--------|:-----------|
|404|ProjectNotExist|The Project does not exist : \{Project\}|
|500|InternalServerError|Specified Server Error Message|

## Example {#section_p5z_ghh_f2b .section}

**Request example:**

```
DELETE / HTTP/1.1
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

**Response example:**

```
HTTP/1.1 200
Server: nginx
Content-Length: 0
Connection: close
Access-Control-Allow-Origin: *
Date: Sun, 27 May 2018 08:25:04 GMT
x-log-requestid: 5B0A6B60BB6EE39764D458B5
```

