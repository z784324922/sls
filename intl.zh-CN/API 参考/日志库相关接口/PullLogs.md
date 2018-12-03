# PullLogs {#reference_f5p_ytq_12b .reference}

根据游标、数量获得日志。获得日志时必须指定 shard。如果在 storm 等情况下可以通过[LoghubClientLib](../../../../intl.zh-CN/用户指南/实时消费/消费组消费/消费组消费.md) 进行选举与协同消费。目前仅支持读取 [PB 格式 LogGroupList](intl.zh-CN/API 参考/公共资源说明/数据编码方式.md) 数据。

## 请求语法 {#section_j5v_14t_12b .section}

```
GET /logstores/ay42/shards/0?type=logs&cursor=MTQ0NzMyOTQwMTEwMjEzMDkwNA==&count=100 HTTP/1.1
Accept: application/x-protobuf
Accept-Encoding: lz4
Authorization: <AuthorizationString>
Date: <GMT Date>
Host: <Project Endpoint>
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

## 请求参数 {#section_mx5_b4t_12b .section}

URL 参数：

|参数名称|类型|是否必须|描述|
|:---|:-|:---|:-|
|type|string|是|此处为 logs。|
|cursor|string|是|游标，用以表示从什么位置开始读取数据，相当于起点。|
|count|int|是|返回的 loggroup 数目，范围为 1~1000。|

**请求头**

-   Accept: application/x-protobuf
-   Accept-Encoding: lz4 或 deflate 或 “”

关于 Log Service API 的公共请求头，请参考 [公共请求头](intl.zh-CN/API 参考/公共请求头.md)。

**响应头**

-   x-log-cursor：当前读取数据下一条 cursor
-   x-log-count：当前返回数量

关于 Log Service API 的公共响应头，请参考 [公共响应头](intl.zh-CN/API 参考/公共响应头.md)。

**响应元素**

protobuf 格式序列化后的数据（可能经过压缩）。

**错误码**

除了返回 Log Service API 的 [通用错误码](intl.zh-CN/API 参考/通用错误码.md)，还可能返回如下特有错误码：

|HTTP 状态码|ErrorCode|ErrorMessage|
|:-------|:--------|:-----------|
|404|LogStoreNotExist|Logstore \{Name\} does not exist|
|400|ParameterInvalid|Parameter Cursor is not valid|
|400|ParameterInvalid|ParameterCount should be in \[0-1000\]|
|400|ShardNotExist|Shard \{ShardID\} does not exist|
|400|InvalidCursor|this cursor is invalid|
|500|InternalServerError|Specified Server Error Message|

## 示例 {#section_e5p_d4t_12b .section}

**请求示例：**

```
读取 0 号 shard 上的数据
GET /logstores/sls-test-logstore/shards/0?cursor=MTQ0NzMyOTQwMTEwMjEzMDkwNA==&count=1000&type=log  
Header:
{
    "Authorization"="LOG 94to3z418yupi6ikawqqd370:WeMYZp6bH/SmWEgryMrLhbxK+7o=", 
    "x-log-bodyrawsize"=0, 
    "User-Agent" : "sls-java-sdk-v-0.6.0", 
    "x-log-apiversion" : "0.6.0", 
    "Host" : "ali-test-project.cn-hangzhou-failover-intranet.sls.aliyuncs.com", 
    "x-log-signaturemethod" : "hmac-sha1", 
    "Accept-Encoding" : "lz4", 
    "Content-Length": 0,
    "Date" : "Thu, 12 Nov 2015 12:03:17 GMT",
    "Content-Type" : "application/x-protobuf", 
    "accept" : "application/x-protobuf"
}
```

**响应示例：**

```
Header:
{
    "x-log-count" : "1000", 
    "x-log-requestid" : "56447FB20351626D7C000874", 
    "Server" : "nginx/1.6.1", 
    "x-log-bodyrawsize" : "34121", 
    "Connection" : "close", 
    "Content-Length" : "4231", 
    "x-log-cursor" : "MTQ0NzMyOTQwMTEwMjEzMDkwNA==", 
    "Date" : "Thu, 12 Nov 2015 12:01:54 GMT", 
    "x-log-compresstype" : "lz4", 
    "Content-Type" : "application/x-protobuf"
}
Body:
<protobuf 格式 loggrouplist 内容> 压缩后结果
```

**翻页**

如果只为了翻页（拿到下一组 Token），不返回数据，可以通过 HTTP HEAD 方式进行请求。

