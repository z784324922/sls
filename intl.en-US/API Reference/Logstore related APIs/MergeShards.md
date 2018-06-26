# MergeShards {#reference_tyv_xtq_12b .reference}

Merge two adjacent shards in readwrite status. Specify a shard ID in the parameter and then Log Service automatically finds the adjacent shard on the right.

## Request syntax {#section_j5v_14t_12b .section}

```
POST /logstores/<logstorename>/shards/<shardid>? action=merge HTTP/1.1
Authorization: <AuthorizationString>
Date: <GMT Date>
Host: <Project Endpoint>
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

## Request Parameters {#section_mx5_b4t_12b .section}

|Parameter name|Type|Required|Description|
|:-------------|:---|:-------|:----------|
|logstoreName|string|No|The Logstore name.|
|shardid|int|No|The shard ID.|

**Request header**

The GetConfig API does not have a special request header.  For more information about the public request headers of Log Service APIs, see [Public request header](intl.en-US/API Reference/Public request header.md).

**Response header**

Content-Type: application/json

The ApplyConfigToMachineGroup API does not have a special response header.  For more information about the public response headers of Log Service APIs, see [Public response header](intl.en-US/API Reference/Public response header.md).

**Response element**

In an array consisting of 3 shards, the first shard is the result of merging, and the other two are not merged.

```
[
    {
        'shardID': 167, 
        'status': 'readwrite', 
        'inclusiveBeginKey': 'e0000000000000000000000000000000',
        'createTime': 1453953105,
        'exclusiveEndKey': 'ffffffffffffffffffffffffffffffff'
    }, 
    {
        'shardID': 30, 
        'status': 'readonly', 
        'inclusiveBeginKey': 'e0000000000000000000000000000000', 
        'createTime': 0, 
        'exclusiveEndKey': 
        'e7000000000000000000000000000000'
    },
    {
        'shardID': 166, 
        'status': 'readonly', 
        'inclusiveBeginKey': 'e7000000000000000000000000000000', 
        'createTime': 1453953073, 
        'exclusiveEndKey': 'ffffffffffffffffffffffffffffffff'
    }
]
```

**Error codes**

Besides the common error codes of Log Service APIs, the GetLogs API may return the following special error codes.

|HTTP status code|ErrorCode|ErrorMessage|
|:---------------|:--------|:-----------|
|404|LogStoreNotExist|logstore \{logstoreName\} does not exist|
|400|ParameterInvalid|invalid shard id|
|400|ParameterInvalid|can not merge the last shard|
|500|InternalServerError|Specified Server Error Message|
|400|LogStoreWithoutShard|logstore has no shard|

**Note:** The \{name\} in the preceding error message is replaced by a specific Logstore name.

## Examples {#section_e5p_d4t_12b .section}

**Request example:**

```
POST /logstores/logstorename/shards/30? action=merge
Header :
{
    "Content-Length": 0, 
    "x-log-signaturemethod": "hmac-sha1", 
    "x-log-bodyrawsize": 0, 
    "User-Agent": "log-python-sdk-v-0.6.0", 
    "Host": "ali-test-project.cn-hangzhou.sls.aliyuncs.com", 
    "Date": "Thu, 12 Nov 2015 03:40:31 GMT", 
    "x-log-apiversion": "0.6.0", 
    "Authorization": "LOG 94to3z418yupi6ikawqqd370:xEOsJ3xeidfdgRq0GbvACiO37jH0I="
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
        'shardID': 167, 
        'status': 'readwrite', 
        'inclusiveBeginKey': 'e0000000000000000000000000000000',
        'createTime': 1453953105,
        'exclusiveEndKey': 'ffffffffffffffffffffffffffffffff'
    }, 
    {
        'shardID': 30, 
        'status': 'readonly', 
        'inclusiveBeginKey': 'e0000000000000000000000000000000', 
        'createTime': 0, 
        'exclusiveEndKey': 
        'e7000000000000000000000000000000'
    },
    {
        'shardID': 166, 
        'status': 'readonly', 
        'inclusiveBeginKey': 'e7000000000000000000000000000000', 
        'createTime': 1453953073, 
        'exclusiveEndKey': 'ffffffffffffffffffffffffffffffff'
    }
]
```

