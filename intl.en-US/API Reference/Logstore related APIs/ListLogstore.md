# ListLogstore {#reference_cc1_wtq_12b .reference}

List the names of all the Logstores in a specified project.

## Request syntax {#section_j5v_14t_12b .section}

```
GET /logstores HTTP/1.1
Authorization: <AuthorizationString> 
Date: <GMT Date>
Host: <Project Endpoint>
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

## Request parameters {#section_mx5_b4t_12b .section}

|Parameter Name|Type|Required |Description|
|:-------------|:---|:--------|:----------|
|offset\(optional\)|Integer|No|The starting position of a returned record. The default value is 1.|
|size\(optional\)|integer|No|The maximum number of entries returned each page. The default value is 500 \(the maximum value\).|
|logstoreName|string|Yes|The Logstore name used for the request \(partial matching is supported\).|

**Request header**

The ListLogstore API does not have a special request header.  For more information about the public request headers of Log Service APIs, see [Public request header](intl.en-US/API Reference/Public request header.md).

**Response head**

The ListLogstore API does not have a special response header.  For more information about the public response headers of Log Service APIs, see [Public response header](intl.en-US/API Reference/Public response header.md).

**Response Element**

After the ListLogstore request is successful, the response body includes the name list of all the Logstores in a specified project. The formats are as follows.

|Name|Type|Description|
|:---|:---|:----------|
|count|integer|The number of returned Logstores.|
|total|integer|he total number of Logstores.|
|logstores|string array|The name list of returned Logstores.|

**Error code**

Besides the [common error codes](intl.en-US/API Reference/Common error codes.md) of Log Service APIs, the ListLogstore API may return the following special error codes.

|HTTP status code|ErrorCode|ErrorMessage|
|:---------------|:--------|:-----------|
|404|ProjectNotExist|Project \{ProjectName\} does not exist|
|500|InternalServerError|Specified Server Error Message|
|400|ParameterInvalid|Invalid parameter size, \(0.6.0\]|
|400|InvalidLogStoreQuery|logstore Query is invalid|

## Example {#section_e5p_d4t_12b .section}

**Request example:**

```
GET /logstores HTTP/1.1
Header: 
{
x-log-apiversion=0.6.0, 
Authorization=LOG 94to3z418yupi6ikawqqd370:we34Siz/SBVyVGMGmMDnvp0xSPo=, 
Host=ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com, 
Date=Wed, 11 Nov 2015 08:09:39 GMT, 
Content-Length=0, 
x-log-signaturemethod=hmac-sha1, 
User-Agent=sls-java-sdk-v-0.6.0, 
Content-Type=application/json
}
```

**Response example**

```
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

