# CreateLogstore {#reference_xl4_ttq_12b .reference}

Create a Logstore in a project.

Example:

```
POST /logstores
```

## Request syntax {#section_it1_kl2_sy .section}

```
POST /logstores HTTP/1.1
Authorization: <AuthorizationString> 
Date: <GMT Date>
Host: <Project Endpoint>
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
{
    "logstoreName" : <logStoreName>,
    "ttl": <ttl>,
    "shardCount": <shardCount>
    "autoSplit": <autoSplit>,
    "maxSplitShard": <maxSplitShard>
}
```

## Request Parameters {#section_d4g_ll2_sy .section}

|Attribute name|Type|Required|Description|
|--------------|----|--------|-----------|
|logstoreName|string|No|The Logstore name, which must be unique in the same project.|
|ttl|integer|No|The data retention time \(in days\).|
|shardCount|integer|No|The number of shards in this Logstore.|

## Request header {#section_nqn_nl2_sy .section}

The CreateLogstore API does not have a special request header. For more information about the public request headers of Log Service APIs, see  [Public request header](intl.en-US/API Reference/Public request header.md).

## Response header {#section_of3_4l2_sy .section}

The CreateLogstore API does not have a special response header. For more information about the public response headers of Log Service APIs, see  [Public response header](intl.en-US/API Reference/Public response header.md).

## Response element {#section_hy4_pl2_sy .section}

The system returns the HTTP status code 200.

## Error codes {#section_evd_ql2_sy .section}

Besides the  [Common error codes](intl.en-US/API Reference/Common error codes.md) of Log Service APIs, the CreateLogstore API may return the following special error codes.

|HTTP Status Code|ErrorCode|ErrorMessage|
|:---------------|:--------|:-----------|
|400|LogstoreAlreadyExist|logstore \{logstoreName\} already exists|
|500|InternalServerError|Specified Server Error Message|
|400|LogstoreInfoInvalid|logstore info is invalid|
|400|ProjectQuotaExceed|Project Quota Exceed|

## Detailed description {#section_rhb_tl2_sy .section}

The logstore cannot be created if quota is invalid.

## Examples {#section_ml2_5l2_sy .section}

**Request example:**

```
POST /logstores HTTP/1.1
Header :
{
x-log-apiversion=0.6.0, 
Authorization=LOG 94to3z418yupi6ikawqqd370:8IwDTWugRK1AZAo0dWQYpffhy48=, 
Host=ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com, 
Date=Wed, 11 Nov 2015 07:35:00 GMT, 
Content-Length=55,
x-log-signaturemethod=hmac-sha1, 
Content-MD5=7EF43D0B8F4A807B95E775048C911C72, 
User-Agent=sls-java-sdk-v-0.6.0, 
Content-Type=application/json
}
Body : 
{
    "logstoreName": "test-logstore",
    "ttl": 1,
    "shardCount": 2
    "autoSplit": true,
    "maxSplitShard": 64
}
```

**Response example:**

```
HTTP/1.1 200 OK
Header:
{
Date=Wed, 11 Nov 2015 07:35:00 GMT, 
Content-Length=0, 
x-log-requestid=5642EFA499248C827B012B39, 
Connection=close, 
Server=nginx/1.6.1
}
```

