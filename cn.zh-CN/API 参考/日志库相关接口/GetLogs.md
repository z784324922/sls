# GetLogs {#reference_bqw_wv5_zdb .reference}

GetLogs 接口查询指定 Project 下某个 Logstore 中的日志数据。还可以通过指定相关参数仅查询符合指定条件的日志数据。

当日志写入到 Logstore 中，日志服务的查询接口（GetHistograms 和 GetLogs）能够查到该日志的延时因写入日志类型不同而异。日志服务按日志时间戳把日志分为如下两类：

-   实时数据：日志中时间点为服务器当前时间点 \(-180秒，900秒\]。例如，日志时间为 UTC 2014-09-25 12:03:00，服务器收到时为 UTC 2014-09-25 12:05:00，则该日志被作为实时数据处理，一般出现在正常场景下。
-   历史数据：日志中时间点为服务器当前时间点 \[-7 x 86400秒, -180秒\)。例如，日志时间为 UTC 2014-09-25 12:00:00，服务器收到时为 UTC 2014-09-25 12:05:00，则该日志被作为历史数据处理，一般出现在补数据场景下。

其中，实时数据写入至可查询的最大延时为3秒（99.9%情况下1秒内即可查询）。

## 请求语法 {#section_j5v_14t_12b .section}

```
GET /logstores/<logstorename>?type=log&topic=<logtopic>&from=<starttime>&to=<endtime>&query=<querystring>&line=<linenum>&offset=<startindex>&reverse=<ture|false> HTTP/1.1
Authorization: <AuthorizationString>
Date: <GMT Date>
Host: <Project Endpoint>
x-log-bodyrawsize: 0
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

## 请求参数 {#section_mx5_b4t_12b .section}

|名称|类型|必选|描述|
|:-|:-|:-|:-|
|logstorename|字符串|是|需要查询日志的 Logstore 名称。|
|type|字符串|是|查询 Logstore 数据的类型。在 GetLogs 接口中该参数必须为 log。|
|from|整型|是|查询开始时间点（精度为秒，从 1970-1-1 00:00:00 UTC 计算起的秒数）。|
|to|整型|是|查询结束时间点（精度为秒，从 1970-1-1 00:00:00 UTC 计算起的秒数）。|
|topic|字符串|否|查询日志主题。|
|query|字符串|否|查询表达式。关于查询表达式的详细语法，请参考 [查询语法](../../../../intl.zh-CN/用户指南/查询与分析/查询语法与功能/查询语法.md)。|
|line|整型|否|请求返回的最大日志条数。取值范围为 0~100，默认值为 100。|
|offset|整型|否|请求返回日志的起始点。取值范围为 0 或正整数，默认值为 0。|
|reverse|布尔型|否|是否按日志时间戳逆序返回日志。true 表示逆序，false 表示顺序，默认值为 false。|

**请求头**

GetLogs接口无特殊请求头。关于 Log Service API 的公共请求头，请参考 [公共请求头](intl.zh-CN/API 参考/公共请求头.md)。

**响应头**

关于 Log Service API 的公共响应头，请参考 [公共响应头](intl.zh-CN/API 参考/公共响应头.md)。

响应头中有专门成员表示请求返回结果是否完整。具体响应元素格式如下：

|名称|类型|描述|
|:-|:-|:-|
|x-log-progress|字符串|查询结果的状态。可以有 Incomplete 和 Complete 两个选值，表示本次是否完整。|
|x-log-count|整型|当前查询结果的日志总数。|

**响应元素**

GetLogs 请求成功，其响应 Body 会包括查询命中的日志数据。当需要查询的日志数据量非常大\(T级别\)的时候，该接口的响应结果可能并不完整，GetLogs的响应body是一个数组，数组中每个元素是一条日志结果。数组中的每个元素结构如下：

|名称|类型|描述|
|:-|:-|:-|
|\_\_time\_\_|整型|日志的时间戳（精度为秒，从 1970-1-1 00:00:00 UTC 计算起的秒数）。|
|\_\_source\_\_|字符串|日志的来源，由写入日志时指定。|
|\[content\]|Key-Value对|日志原始内容，以 Key-value 对的形式组织。|

**细节描述**

-   该接口中由请求参数 from 和 to 定义的时间区间遵循“左闭右开”原则，即该时间区间包括区间开始时间点，但不包括区间结束时间点。如果 from 和 to 的值相同，则为无效区间，函数直接返回错误。
-   如上所述，该接口一次调用必须要在限定时间内返回结果，每次查询只能扫描指定条数的日志量。如果一次请求需要处理的数据量非常大的时候，该请求会返回不完整的结果（并在返回header中的 x-log-progress 成员标示是否完整）。如此同时，服务端会缓存 15 分钟内的查询结果。当查询请求的结果有部分被缓存命中，则服务端会在这次请求中继续扫描未被缓存命中的日志数据。为了减少您合并多次查询结果的工作量，服务端会把缓存命中的查询结果与本次查询新命中的结果合并返回给您。因此，日志服务可以让您通过以相同参数反复调用该接口来获取最终完整结果。因为您的查询涉及的日志数据量变化非常大，日志服务 API 无法预测需要调用多少次该接口而获取完整结果。所以需要用户通过检查每次请求的返回结果中的x-log-progress 成员状态值来确定是否需要继续。需要注意的是，每次重复调用该接口都会重新消耗相同数量的查询 CU。

**错误码**

除了返回 Log Service API 的 [通用错误码](intl.zh-CN/API 参考/通用错误码.md)，还可能返回如下特有错误码：

|HTTP状态码（Status Code）|错误码（Error Code）|错误消息（Error Message）|描述（Description）|
|:-------------------|:--------------|:------------------|:--------------|
|404|LogStoreNotExist|logstore \{Name\} does not exist.|日志库（logstore）不存在。|
|400|InvalidTimeRange|request time range is invalid|请求的时间区间无效。|
|400|InvalidQueryString|query string is invalid|请求的查询字符串无效。|
|400|InvalidOffset|offset is invalid|请求的 offset 参数无效。|
|400|InvalidLine|line is invalid|请求的 line 参数无效。|
|400|InvalidReverse|Reverse value is invalid|Reverse 参数的值无效。|
|400|IndexConfigNotExist|logstore without index config|日志库（logstore）未开启索引。|

**说明：** 上表错误消息中 \{name\} 表示该部分会被具体的 LogstoreName 来替换。

## 示例 {#section_e5p_d4t_12b .section}

以杭州地域内名为 big-game 的 Project 为例，查询该 project 内名为 app\_log 的 Logstore 中，主题为 groupA 的日志数据。查询区间为 2014-09-01 00:00:00 到 2014-09-01 22:00:00，查询关键字为 error，且从时间区间头开始查询，最多返回 20 条日志数据。

**请求示例：**

```
GET /logstores/app_log？type=log&topic=groupA&from=1409529600&to=1409608800&query=error&line=20&offset=0 HTTP/1.1
Authorization: <AuthorizationString>
Date: Wed, 3 Sept. 2014 08:33:46 GMT
Host: big-game.cn-hangzhou.log.aliyuncs.com
x-log-bodyrawsize: 0
x-log-apiversion: 0.4.0
x-log-signaturemethod: hmac-sha1
```

**响应示例：**

```
HTTP/1.1 200 OK
Content-MD5: 36F9F7F0339BEAF571581AF1B0AAAFB5
Content-Type: application/json
Content-Length: 269
Date: Wed, 3 Sept. 2014 08:33:47 GMT
x-log-requestid: efag01234-12341-15432f
x-log-progress : Complete
x-log-count : 10000
x-log-processed-rows: 10000
x-log-elapsed-millisecond:5
{
    "progress": "Complete",
    "count": 2,
    "logs": [
        {
            "__time__": 1409529660,
            "__source__": "10.237.0.17",
            "Key1": "error",
            "Key2": "Value2"
        },
        {
            "__time__": 1409529680,
            "__source__": "10.237.0.18",
            "Key3": "error",
            "Key4": "Value4"
        }
    ]
}
```

在这个响应示例中，x-log-progress 成员的状态为 Complete，表明整个日志查询已经完成，返回结果为完整结果。在这次请求中共查询到 2 条符合条件的日志，且日志数据在 logs 成员中。如果响应结果中的 x-log-progress 成员的状态为 Incomplete，则需要重复相同请求以获得完整结果。

