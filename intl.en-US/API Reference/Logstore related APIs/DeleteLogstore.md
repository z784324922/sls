# DeleteLogstore {#reference_xzb_5tq_12b .reference}

Delete a Logstore, including all the shards and indexes in the Logstore.

## Request syntax {#section_vzt_dm2_sy .section}

```
DELETE /logstores/{logstoreName} HTTP/1.1
Authorization: <AuthorizationString> 
Date: <GMT Date>
Host: <Project Endpoint>
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

## Request Parameters {#section_j3x_2m2_sy .section}

|Parameter Name|Type|Required |Description|
|:-------------|:---|:--------|:----------|
|logstoreName|string|Yes|The Logstore name, which must be unique in the same project.|

## Request header {#section_nqn_nl2_sy .section}

The DeleteLogstore API does not have a special request header. For more information about the public request headers of Log Service APIs, see  [Public request header](reseller.en-US/API Reference/Public request header.md).

## Response head {#section_of3_4l2_sy .section}

The DeleteLogstore API does not have a special response header. For more information about the public response headers of Log Service APIs, see Public response header.

## Response element {#section_hy4_pl2_sy .section}

The returned HTTP status code is 200.

## Error code {#section_izy_km2_sy .section}

Besides the [Common error codes](reseller.en-US/API Reference/Common error codes.md) of Log Service APIs, the DeleteLogstore API may return the following special error codes.

|HTTP status code|Error Code| Error message|
|:---------------|:---------|:-------------|
|404|LogStoreNotExist|logstore \{logstoreName\} does not exist|
|500|InternalServerError|Specified Server Error Message|

## Example {#section_ayr_nm2_sy .section}

**Request example:**

```
DELETE /logstores/test_logstore HTTP/1.1
Header :
{
x-log-apiversion=0.6.0, 
Authorization=LOG <yourAccessKeyId>:<yourSignature>, 
Host=ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com, 
Date=Wed, 11 Nov 2015 08:09:38 GMT, 
Content-Length=0, 
x-log-signaturemethod=hmac-sha1, 
User-Agent=sls-java-sdk-v-0.6.0, 
Content-Type=application/json
}
```

**Response example:**

```
HTTP/1.1 200 OK
Header:
{
Date=Wed, 11 Nov 2015 08:09:39 GMT, 
Content-Length=0, 
x-log-requestid=5642F7C399248C817B013A07, 
Connection=close, 
Server=nginx/1.6.1
}
```

