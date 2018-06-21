# GetCursor {#concept_ffg_3dm_12b .concept}

GetCursor 根据时间获得游标（cursor），下图表示 Project、Logstore、Shard 与 cursor 的关系：

-   Project 下有多个 Logstore
-   每个 Logstore 会有多个 Shard
-   通过 cursor 可以获得特定日志对应的位置

## 请求语法 {#section_j5v_14t_12b .section}

```
GET /logstores/ay42/shards/2?type=cursor&from=1402341900 HTTP/1.1
Authorization: <AuthorizationString>
Date: <GMT Date>
Host: <Project Endpoint>
x-log-apiversion: 0.6.0
```

## 请求参数 {#section_mx5_b4t_12b .section}

|参数名称|类型|是否必须|描述|
|:---|:-|:---|:-|
|shard|string|是| |
|type|string|是|此处为 cursor|
|from|string|是|时间点（UNIX下秒数），或 begin，end|

**Logstore 生命周期：**

Logstore 生命周期由属性中 lifeCycle 字段指定。例如，当前时间为 2015-11-11 09:00:00，lifeCycle=24。则每个 shard 中可以消费的数据时间段为 \[2015-11-10 09:00:00,2015-11-11 09:00:00\)，这里的时间指的是 Server 端时间。

通过 from 可以在 Shard 中定位生命周期内的日志，假设 Logstore 生命周期为 \[begin\_time,end\_time\)，from=from\_time

```
 from_time <= begin_time or from_time == "begin" : 返回时间点为 begin_time 对应的 cursor 位置
 from_time >= end_time or from_time == "end" : 返回当前时间点下，下一条将被写入的 cursor 位置(当前该 cursor 位置上无数据)
 from_time > begin_time and from_time < end_time : 返回第一个服务端接收时间 >= from_time 的数据包对应的 cursor
```

**请求头**

无特有请求头。关于 Log Service API 的公共请求头，请参考 [公共请求头](intl.zh-CN/API 参考/公共请求头.md)。

**响应头**

无特有响应头。关于 Log Service API 的公共响应头，请参考 [公共响应头](intl.zh-CN/API 参考/公共响应头.md)。

**响应元素**

```
{
    "cursor": "MTQ0NzI5OTYwNjg5NjYzMjM1Ng=="
}
```

**错误码**

除了返回 Log Service API 的 [通用错误码](intl.zh-CN/API 参考/通用错误码.md)，还可能返回如下特有错误码：

|HTTP 状态码|ErrorCode|ErrorMessage|
|:-------|:--------|:-----------|
|404|LogStoreNotExist|Logstore \{Name\} does not exist|
|400|ParameterInvalid|Parameter From is not valid|
|400|ShardNotExist|Shard \{ShardID\} does not exist|
|500|InternalServerError|Specified Server Error Message|
|400|LogStoreWithoutShard|the logstore has no shard|

## 示例 {#section_e5p_d4t_12b .section}

**请求示例：**

```
GET /logstores/sls-test-logstore/shards/0?type=cursor&from=begin
Header:
{
    "Content-Length": 0, 
    "x-log-signaturemethod": "hmac-sha1", 
    "x-log-bodyrawsize": 0, 
    "User-Agent": "log-python-sdk-v-0.6.0", 
    "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com", 
    "Date": "Thu, 12 Nov 2015 03:56:57 GMT", 
    "x-log-apiversion": "0.6.0", 
    "Content-Type": "application/json", 
    "Authorization": "LOG 94to3z418yupi6ikawqqd370:+vo0Td6PrN0CGoskJoOiAsnkXgA="
}
```

**响应示例：**

```
Header:
{
    "content-length": "41", 
    "server": "nginx/1.6.1", 
    "connection": "close", 
    "date": "Thu, 12 Nov 2015 03:56:57 GMT", 
    "content-type": "application/json", 
    "x-log-requestid": "56440E0999248C070600C6AA"
}
Body:
{
    "cursor": "MTQ0NzI5OTYwNjg5NjYzMjM1Ng=="
}
```

