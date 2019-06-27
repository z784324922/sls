# PutLogs {#reference_a5t_kxr_zdb .reference}

向指定的 LogStore 写入日志数据。目前仅支持写入[PB 格式 LogGroup](intl.zh-CN/API 参考/公共资源说明/数据编码方式.md)日志数据。

**说明：** 日志数据为一个LogGroup。

写入时有两种模式：

-   负载均衡模式（LoadBalance）：自动根据 Logstore 下所有可写的 shard 进行负载均衡写入。该方法对写入可用性较高（SLA：99.95%），适合写入与消费数据与 shard 无关的场景，例如不保序。
-   根据 Key 路由 shard 模式（KeyHash）：写入时需要传递一个 Key，服务端自动根据 Key 选择当前符合该 Key 区间的 Shard 写入。例如，可以将某个生产者（例如 instance）根据名称 Hash 到固定 Shard 上，这样就能保证写入与消费在该 Shard 上是严格有序的（在 Merge/Split 过程中能够严格保证对于 Key 在一个时间点只会出现在一个 Shard 上，[参见 shard 数据模型](../../../../intl.zh-CN/产品简介/基本概念/分区.md)）。

## 请求语法 {#section_j5v_14t_12b .section}

 **负载均衡写入模式** 

``` {#codeblock_yqv_7s3_uny}
POST /logstores/<logstorename>/shards/lb HTTP/1.1
Authorization: <AuthorizationString>
Content-Type: application/x-protobuf
Content-Length: <Content Length>
Content-MD5: <Content MD5>
Date: <GMT Date>
Host: <Project Endpoint>
x-log-apiversion: 0.6.0
x-log-bodyrawsize: <BodyRawSize>
x-log-compresstype: lz4
x-log-signaturemethod: hmac-sha1
<PB 格式日志压缩数据>
```

 **根据 Key 路由 shard 模式** 

在 header 中增加 x-log-hashkey，用来判断落在哪个 shard 的 range 中。该参数为可选参数，不填情况下自动切换负载均衡写模式。

``` {#codeblock_6qm_t9u_sqa}
POST /logstores/<logstorename>/shards/lb HTTP/1.1
Authorization: <AuthorizationString>
Content-Type: application/x-protobuf
Content-Length: <Content Length>
Content-MD5: <Content MD5>
Date: <GMT Date>
Host: <Project Endpoint>
x-log-apiversion: 0.6.0
x-log-bodyrawsize: <BodyRawSize>
x-log-compresstype: lz4
x-log-hashkey : 14d2f850ad6ea48e46e4547edbbb27e0
x-log-signaturemethod: hmac-sha1
<PB 格式日志压缩数据>
```

## 请求参数 {#section_mx5_b4t_12b .section}

|名称|类型|必选|描述|
|:-|:-|:-|:-|
|logstorename|string|是|需要写入日志的 Logstore 名称。|

 **请求头** 

根据 Key 路由 Shard 模式下需要增加 x-log-hashkey 请求头（参见上述示例）。

关于 Log Service API 的公共请求头，请参考 [公共请求头](intl.zh-CN/API 参考/公共请求头.md)。

 **响应头** 

无特有响应头。关于 Log Service API 的公共响应头，请参考 [公共响应头](intl.zh-CN/API 参考/公共响应头.md)。

 **响应元素** 

成功后无任何响应元素。

 **细节描述** 

-   PutLogs 接口每次可以写入的日志组数据量上限为 3MB 或者 4096 条，日志组中每条日志下的Value部分不超过1MB大小。只要日志数据量超过这两条上限中的任意一条则整个请求失败，且无任何日志数据成功写入。
-   服务端会对每次 PutLogs 写入的日志数据做格式检查（具体日志格式要求请参考 [核心概念](../../../../intl.zh-CN/产品简介/基本概念/简介.md)，只要日志数据中有任何一条日志不符合规范，则整个请求失败，且无任何日志数据成功写入。

 **错误码** 

除了返回 Log Service API 的 [通用错误码](intl.zh-CN/API 参考/通用错误码.md)，还可能返回如下特有错误码：

|HTTP 状态码（Status Code）|错误码（Error Code）|错误消息（Error Message）|描述（Description）|
|:--------------------|:--------------|:------------------|:--------------|
|400|PostBodyInvalid|Protobuffer content cannot be parsed.|Protobuffer 内容不能够解析。|
|400|InvalidTimestamp|Invalid timestamps are in logs.|日志内容中有无效的日志时间戳。|
|400|InvalidEncoding|Non-UTF8 characters are in logs.|日志内容中有非 UTF8 字符。|
|400|InvalidKey|Invalid keys are in logs.|日志内容中有无效的 key。|
|400|PostBodyTooLarge|Logs must be less than or equal to 3 MB and 4096 entries.|日志内容包含的日志必须小于 3MB 和 4096 条。|
|400|PostBodyUncompressError|Failed to decompress logs.|日志内容解压失败。|
|499|PostBodyInvalid|The post data time is out of range|日志中时间范围不在 \[-7\*24Hour, +15Min\] 有效范围内。|
|404|LogStoreNotExist|logstore \{Name\} does not exist.|日志库（Logstore）不存在。|

**说明：** 上表错误消息中 \{name\} 表示该部分会被具体的 LogstoreName 来替换。

## 示例 {#section_e5p_d4t_12b .section}

 **请求示例：** 

``` {#codeblock_z5q_o6g_008}
POST /logstores/sls-test-logstore
{
    "Content-Length": 118,
    "Content-Type":"application/x-protobuf",
    "x-log-bodyrawsize":1356,
    "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com",
    "Content-MD5":"6554BD042149C844761C2C094A8FECCE",
    "Date":"Thu, 12 Nov 2015 06:54:26 GMT",
    "x-log-apiversion": "0.6.0",
    "x-log-compresstype":"lz4"
    "x-log-signaturemethod": "hmac-sha1",
    "Authorization":"LOG <yourAccessKeyId>:<yourSignature>"
}
<PB 格式日志使用 Lz4 压缩后的二进制数据>
```

 **响应示例：** 

``` {#codeblock_u9d_vzj_ayg}
Header
{   
    "date": "Thu, 12 Nov 2015 06:53:03 GMT",
    "connection": "close",
    "x-log-requestid": "5644160399248C060600D216",
    "content-length": "0",
    "server": "nginx/1.6.1"
}
```

