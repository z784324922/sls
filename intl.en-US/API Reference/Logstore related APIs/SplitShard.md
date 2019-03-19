# SplitShard {#reference_of2_xtq_12b .reference}

Split a specified shard in readwrite status.

## Request syntax {#section_j5v_14t_12b .section}

```
POST /logstores/<logstorename>/shards/<shardid>? action=split&key=<splitkey> HTTP/1.1
Authorization: <AuthorizationString>
Date: <GMT Date>
Host: <Project Endpoint>
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

## Request parameters {#section_mx5_b4t_12b .section}

|Parameter|Type|Required |Description|
|:--------|:---|:--------|:----------|
|logstoreName|string|Yes|The Logstore name.|
|shardid|int|Yes|The shard ID.|
|Splitkey|string|Yes|The split location.|

**Request header**

No special request header is available. For more information about the public request headers of Log Service APIs, see [Public request header](reseller.en-US/API Reference/Public request header.md).

**Response header**

Content-Type: application/json

No special response header is available. For more information about the public response headers of Log Service APIs, see [Public response header](reseller.en-US/API Reference/Public response header.md).

**Response element**

In an array composed of three shards, the first shard is the original shard before the split, and the other two are the shards generated after the split.

```
[
    {
        "shardID": 33,
        "status": "readonly",
        "inclusiveBeginKey": "ee000000000000000000000000000000",
        "exclusiveEndKey": "ffffffffffffffffffffffffffffffff",
        "createTime": 1453949705
    },
    {
        "shardID": 163,
        "status": "readwrite",
        "inclusiveBeginKey": "ee000000000000000000000000000000",
        "exclusiveEndKey": "ef000000000000000000000000000000",
        "createTime": 1453949705
    },
    {
        "shardID": 164,
        "status": "readwrite",
        "inclusiveBeginKey": "ef000000000000000000000000000000",
        "exclusiveEndKey": "ffffffffffffffffffffffffffffffff",
        "createTime": 1453949705
    }
]
```

**Error code**

Besides the [common error codes](reseller.en-US/API Reference/Common error codes.md) of Log Service APIs, the SplitShard API may return the following special error codes.

|HTTP status code|ErrorCode|ErrorMessage|
|:---------------|:--------|:-----------|
|404|LogStoreNotExist|logstore \{logstoreName\} does not exist|
|400|ParameterInvalid|invalid shard id|
|400|ParameterInvalid|invalid mid hash|
|500|InternalServerError|Specified Server Error Message|
|400|LogStoreWithoutShard|logstore has no shard|

**Note:** The \{name\} in the preceding error message is replaced with a specific Logstore name.

## Example {#section_e5p_d4t_12b .section}

**Request example**

```
POST /logstores/logstorename/shards/33? action=split&key=ef000000000000000000000000000000
Header :
{
    "Content-Length": 0, 
    "x-log-signaturemethod": "hmac-sha1", 
    "x-log-bodyrawsize": 0, 
    "User-Agent": "log-python-sdk-v-0.6.0", 
    "Host": "ali-test-project.cn-hangzhou.sls.aliyuncs.com", 
    "Date": "Thu, 12 Nov 2015 03:40:31 GMT", 
    "x-log-apiversion": "0.6.0", 
    "Authorization": "LOG <yourAccessKeyId>:<yourSignature>"
}
```

**Response example**

```
Header:
{
    "content-length": "57", 
    "server": "nginx/1.6.1", 
    "connection": "close", 
    "date": "Thu, 12 Nov 2015 03:40:31 GMT", 
    "content-type": "application/json", 
    "x-log-requestid": "56440A2F99248C050600C74C"
}
Body :
[
    {
        "shardID": 33,
        "status": "readonly",
        "inclusiveBeginKey": "ee000000000000000000000000000000",
        "exclusiveEndKey": "ffffffffffffffffffffffffffffffff",
        "createTime": 1453949705
    },
    {
        "shardID": 163,
        "status": "readwrite",
        "inclusiveBeginKey": "ee000000000000000000000000000000",
        "exclusiveEndKey": "ef000000000000000000000000000000",
        "createTime": 1453949705
    },
    {
        "shardID": 164,
        "status": "readwrite",
        "inclusiveBeginKey": "ef000000000000000000000000000000",
        "exclusiveEndKey": "ffffffffffffffffffffffffffffffff",
        "createTime": 1453949705
    }
]
```

