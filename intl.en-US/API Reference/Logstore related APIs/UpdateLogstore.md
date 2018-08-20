# UpdateLogstore {#reference_zgr_5tq_12b .reference}

Update the Logstore attributes.  Currently, only Time To Live \(TTL\) and shard attributes can be updated.

## Request syntax {#section_j5v_14t_12b .section}

```
PUT /logstores/{logstoreName} HTTP/1.1
Authorization: <AuthorizationString> 
Date: <GMT Date>
Host: <Project Endpoint>
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
{
    "logstoreName": <logstoreName>,
    "ttl": <ttl>,
    "shardCount": <shardCount>
    "autoSplit": <autoSplit>,
    "maxSplitShard": <maxSplitShard>
}
```

## Request Parameters {#section_mx5_b4t_12b .section}

|Parameter Name|Type|Required |Description|
|:-------------|:---|:--------|:----------|
|logstoreName|string|Yes| The Logstore name, which must be unique in the same project.|
|TTL|integer|Yes|The lifecycle \(in days\) of log data. The value range is 1–365. Open a ticket for additional requirements.|
|shardCount|integer|Yes|The number of shards, ranging from 1 to 10.|

**Request header**

The UpdateLogstore API does not have a special request header.  For more information about the public request headers of Log Service APIs, see [Public request header](intl.en-US/API Reference/Public request header.md).

**Response head**

The UpdateLogstore API does not have a special response header.  For more information about the public response headers of Log Service APIs, see   [Public response header](intl.en-US/API Reference/Public response header.md).

**Response element**

The returned HTTP status code is 200.

**Error code**

Besides the  [common error codes](intl.en-US/API Reference/Common error codes.md) of Log Service APIs,  the UpdateLogstore API may return the following special error codes.

|HTTP status code|Error Code|Error Message|
|:---------------|:---------|:------------|
|404|ProjectNotExist|Project \{ProjectName\} does not exist|
|404|LogStoreNotExist|logstore \{logstoreName\} does not exist|
|400|LogStoreAlreadyExist|logstore \{logstoreName\} already exists|
|500|InternalServerError|Specified Server Error Message|
|400|ParameterInvalid|invalid shard count, you can only increase the count|

**Detailed description**

Currently, the number of shards can only be increased rather than decreased.

## Example {#section_e5p_d4t_12b .section}

**Request example**

```
PUT /logstores/test-logstore HTTP/1.1
Header:
{
x-log-apiversion=0.6.0, 
Authorization=LOG 94to3z418yupi6ikawqqd370:wFcl3ohVJupCi0ZFxRD0x4IA68A=, 
Host=ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com, 
Date=Wed, 11 Nov 2015 08:28:19 GMT, 
Content-Length=55, 
x-log-signaturemethod=hmac-sha1, 
Content-MD5=757C60FC41CC7D3F60B88E0D916D051E, 
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
Date=Wed, 11 Nov 2015 08:28:20 GMT, 
Content-Length=0, 
x-log-requestid=5642FC2399248C8F7B0145FD, 
Connection=close, 
Server=nginx/1.6.1
}
```

