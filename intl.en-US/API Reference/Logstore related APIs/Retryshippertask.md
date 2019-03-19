# Retryshippertask {#reference_its_ztq_12b .reference}

Rerun failed LogShipper tasks.

## Request syntax {#section_j5v_14t_12b .section}

```
PUT /logstores/{logstoreName}/shipper/{shipperName}/tasks HTTP/1.1
Authorization: <AuthorizationString> 
Date: <GMT Date>
Host: <Project Endpoint>
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
["task-id-1", "task-id-2", "task-id-2"]
```

## Request parameters {#section_mx5_b4t_12b .section}

|Parameter name|Type|Required|Description|
|:-------------|:---|:-------|:----------|
|logstoreName|string|Yes|The Logstore name, which is unique in the same project.|
|shipperName|string|Yes|The name of the log shipping rule, which is unique in the same Logstore.|

**Request header**

The RetryShipperTask API does not have a special request header. For more information about the public request headers of Log Service APIs, see [Public request header](reseller.en-US/API Reference/Public request header.md).

**Response head**

RetryShipperTask does not have a special response header. For more information about the public response headers of Log Service APIs, see [Public response header](reseller.en-US/API Reference/Public response header.md).

**Response element**

The returned HTTP status code is 200.

**Error code**

Besides the [common error codes](reseller.en-US/API Reference/Common error codes.md) of Log Service APIs, the RetryShipperTask API may return the following special error codes.

|HTTP status code|ErrorCode|ErrorMessage|
|:---------------|:--------|:-----------|
|404|ProjectNotExist|Project \{ProjectName\} does not exist|
|404|LogStoreNotExist|logstore \{logstoreName\} does not exist|
|400|ShipperNotExist|shipper \{logstoreName\} does not exist|
|500|InternalServerError|Specified Server Error Message|
|400|ParameterInvalid|Each time allows 10 task retries only|

**Detailed description**

You can rerun at most 10 failed LogShipper tasks at a time.

## Example {#section_e5p_d4t_12b .section}

**Request example**

```
PUT /logstores/test-logstore/shipper/test-shipper/tasks HTTP/1.1
Header:
{
x-log-apiversion=0.6.0, 
Authorization=LOG <yourAccessKeyId>:<yourSignature>, 
Host=ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com, 
Date=Wed, 11 Nov 2015 08:28:19 GMT, 
Content-Length=55, 
x-log-signaturemethod=hmac-sha1, 
Content-MD5=757C60FC41CC7D3F60B88E0D916D051E, 
User-Agent=sls-java-sdk-v-0.6.0, 
Content-Type=application/json
}
Body : 
["task-id-1", "task-id-2", "task-id-2"]
```

**Response example**

```
HTTP/1.1 200 OK
Header:
{
Date=Wed, 11 Nov 2015 08:28:20 GMT, 
Content-Length=0, 
x-log-requestid=5642FC2399248C8F7B0145FD, 
Connection=close, 
Server=nginx/1.6.1
}
```

