# ListShards {#reference_gd4_wtq_12b .reference}

列出 Logstore 下当前所有可用 Shard。

## 请求语法 {#section_j5v_14t_12b .section}

``` {#codeblock_qtg_lz1_u1q}
GET /logstores/<logstorename>/shards HTTP/1.1
Authorization: <AuthorizationString>
Date: <GMT Date>
Host: <Project Endpoint>
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

## 请求参数 {#section_mx5_b4t_12b .section}

|参数名称|类型|是否必须|描述|
|:---|:-|:---|:-|
|logstoreName|string|是|日志库名称|

 **请求头** 

ListShards接口无特有请求头。关于 Log Service API 的公共请求头，请参考 [公共请求头](intl.zh-CN/API 参考/公共请求头.md)。

 **响应头** 

ListShards接口无特有响应头。关于 Log Service API 的公共响应头，请参考 [公共响应头](intl.zh-CN/API 参考/公共响应头.md)。

 **响应元素** 

shard 元素组成的数组。

``` {#codeblock_vz5_xxo_iaq}
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

 **错误码** 

除了返回 Log Service API 的 [通用错误码](intl.zh-CN/API 参考/通用错误码.md)，还可能返回如下特有错误码：

|HTTP 状态码|ErrorCode|ErrorMessage|
|:-------|:--------|:-----------|
|404|LogStoreNotExist|logstore \{logstoreName\} does not exist|
|500|InternalServerError|Specified Server Error Message|
|400|LogStoreWithoutShard|logstore has no shard|

**说明：** 上表错误消息中 \{name\} 表示该部分会被具体的 LogstoreName 来替换。

## 示例 {#section_e5p_d4t_12b .section}

**请求示例：** 

``` {#codeblock_dkz_nth_ego}
GET /logstores/sls-test-logstore/shards
Header :
{
    "Content-Length": 0, 
    "x-log-signaturemethod": "hmac-sha1", 
    "x-log-bodyrawsize": 0, 
    "User-Agent": "log-python-sdk-v-0.6.0", 
    "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com", 
    "Date": "Thu, 12 Nov 2015 03:40:31 GMT", 
    "x-log-apiversion": "0.6.0", 
    "Authorization": "LOG <yourAccessKeyId>:<yourSignature>"
}
```

 **响应示例：** 

``` {#codeblock_6g9_810_wsp}
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

