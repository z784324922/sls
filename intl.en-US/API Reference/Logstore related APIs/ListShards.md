# ListShards {#reference_gd4_wtq_12b .reference}

List all available shards in a Logstore.

## Request syntax {#section_j5v_14t_12b .section}

```
GET /logstores/<logstorename>/shards HTTP/1.1
Authorization: <AuthorizationString>
Date: <GMT Date>
Host: <Project Endpoint>
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

## Request parameters {#section_mx5_b4t_12b .section}

|Parameter name |Type|Required |Description |
|:--------------|:---|:--------|:-----------|
|logstoreName |string|Yes|Logstore name|

**Request header**

No special request header is available. The ListShards API does not have a special request header. For more information about the public request headers of Log Service APIs, see [Public request header](intl.en-US/API Reference/Public request header.md).

**Response header**

No special response header is available. The ListShards API does not have a special request header. For more information about the public request headers of Log Service APIs, see [Public response header](intl.en-US/API Reference/Public response header.md).

**Response element**

An array  composed of shards.

```
[
    {
        "shardID": 0,
        "status": "readwrite",
        "inclusiveBeginKey": "00000000000000000000000000000000",
        "exclusiveEndKey": "8000000000000000000000000000000",
        "createTime": 1453949705
    },
    {
        ...
    },
    {
        ...
    }
]
```

**Error code**

Besides the common error codes  [common error codes](intl.en-US/API Reference/Common error codes.md) of Log Service APIs, the ListShards API may return the following special error codes.

|HTTP status code |Error code |Error message|
|:----------------|:----------|:------------|
|404|LogStoreNotExist |logstore \{logstoreName\} does not exist|
|500|InternalServerError |Specified server error message |
|400 |Logstorewithoutshard |logstore has no shard|

**Note:** The \{name\} in the preceding error message is replaced by a specific Logstore name.

## Example {#section_e5p_d4t_12b .section}

**Request example**

```
GET /logstores/sls-test-logstore/shards
Header:
{
    "Content-Length": 0, 
    "x-log-signaturemethod": "hmac-sha1", 
    "x-log-bodyrawsize": 0, 
    "User-Agent": "log-python-sdk-v-0.6.0", 
    "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com", 
    "Date": "Thu, 12 Nov 2015 03:40:31 GMT", 
    "x-log-apiversion": "0.6.0", 
    "Authorization": "LOG AK\_ID:Signature"
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
        "shardID": 1,
        "status": "readwrite",
        "inclusiveBeginKey": "00000000000000000000000000000000",
        "exclusiveEndKey": "8000000000000000000000000000000",
        "createTime": 1453949705
    },
    {
        "shardID": 2,
        "status": "readwrite",
        "inclusiveBeginKey": "80000000000000000000000000000000",
        "exclusiveEndKey": "ffffffffffffffffffffffffffffffff",
        "createTime": 1453949705
    },
    {
        "shardID": 0,
        "status": "readonly",
        "inclusiveBeginKey": "00000000000000000000000000000000",
        "exclusiveEndKey": "ffffffffffffffffffffffffffffffff",
        "createTime": 1453949705
    }
]
```

