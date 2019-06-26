# GetHistograms {#reference_oxy_xv5_zdb .reference}

GetHistograms 接口查询指定的 Project 下某个 Logstore 中日志的分布情况。还可以通过指定相关参数仅查询符合指定条件的日志分布情况。

当日志写入到 logstore 中，日志服务的查询接口（GetHistograms 和 GetLogs）能够查到该日志的延时因写入日志类型不同而异。日志服务按日志时间戳把日志分为如下两类：

-   实时数据：日志中时间点为服务器当前时间点 \(-180秒，900秒\]。例如，日志时间为 UTC 2014-09-25 12:03:00，服务器收到时为 UTC 2014-09-25 12:05:00，则该日志被作为实时数据处理，一般出现在正常场景下。
-   历史数据：日志中时间点为服务器当前时间点 \[-7 x 86400秒, -180秒\)。例如，日志时间为 UTC 2014-09-25 12:00:00，服务器收到时为 UTC 2014-09-25 12:05:00，则该日志被作为历史数据处理，一般出现在补数据场景下。

其中，实时数据写入至可查询的延时为3秒。

## 请求语法 {#section_j5v_14t_12b .section}

``` {#codeblock_x9a_lv5_ukj}
GET /logstores/<logstorename>?type=histogram&topic=<logtopic>&from=<starttime>&to=<endtime>&query=<querystring> HTTP/1.1
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
|logstorename|string|是|需要查询日志的 logstore 名称。|
|type|string|是|查询 logstore 数据的类型，在 GetHistograms 接口中该参数必须为 histogram。|
|from|int|是|查询开始时间点（精度为秒，从 1970-1-1 00:00:00 UTC 计算起的秒数）。|
|to|int|是|查询结束时间点（精度为秒，从 1970-1-1 00:00:00 UTC 计算起的秒数）。|
|topic|string|否|查询日志主题。|
|query|string|否|查询表达式。关于查询表达式的详细语法，请参考[查询语法](../../../../intl.zh-CN/用户指南/查询与分析/查询语法与功能/查询语法.md)。|

 **请求头** 

GetHistograms 接口无特有请求头。关于 Log Service API 的公共请求头，请参考 [公共请求头](intl.zh-CN/API 参考/公共请求头.md)。

 **响应头** 

GetHistograms 接口无特有响应头。关于 Log Service API 的公共响应头，请参考 [公共响应头](intl.zh-CN/API 参考/公共响应头.md)。

 **响应元素** 

GetHistograms 请求成功，其响应 Body 会包括查询命中的日志数量在时间轴上的分布情况。响应结果会把您查询的时间范围进行均匀分割成若干个（1 到 60 个不等）子时间区间，并返回每个子时间区间内命中的日志条数。由于日志服务需要在限定时间内返回结果以保证实时性，每次查询只能扫描指定条数的日志量。当需要查询日志数据量非常大时，该接口的响应结果可能并不完整。所以响应元素中有专门成员表示请求返回结果是否完整。具体响应元素格式如下：

|名称|类型|描述|
|:-|:-|:-|
|progress|string|查询结果的状态。可以有 Incomplete 和 Complete 两个选值，表示结果是否完整。|
|count|int|当前查询结果中所有日志总数。|
|histograms|array|当前查询结果在划分的子时间区间上的分布状况，具体结构见下面的描述。|

上表中的 histograms 数组中的每个元素结构如下：

|名称|类型|描述|
|:-|:-|:-|
|from|int|子时间区间的开始时间点（精度为秒，从 1970-1-1 00:00:00 UTC 计算起的秒数）|
|to|int|子时间区间的结束时间点（精度为秒，从 1970-1-1 00:00:00 UTC 计算起的秒数）|
|count|int|当前查询结果在该子时间区间内命中的日志条数|
|progress|int|当前查询结果在该子时间区间内的结果是否完整，可以有 Incomplete 和 Complete 两个选值。|

 **细节描述** 

-   该接口中涉及的时间区间（无论是请求参数 from 和 to 定义的时间区间，还是返回结果中各个子时间区间）都遵循“左闭右开”原则，即该时间区间包括区间开始时间点，但不包括区间结束时间点。如果 from 和 to 的值相同，则为无效区间，函数直接返回错误。
-   该接口的响应中子区间划分方式是一致稳定的。如果您在请求查询的时间区间不变，则响应中子区间划分结果也不会改变。
-   如上所述，该接口一次调用必须要在限定时间内返回结果，每次查询只能扫描指定条数的日志量。如果一次请求需要处理的数据量非常大的时候，该请求会返回不完整的结果（并在返回结果中的 progress 成员标示是否完整）。与此同时，服务端会缓存 15 分钟内的查询结果。当查询请求的结果有部分被缓存命中，则服务端会在这次请求中继续扫描未被缓存命中的日志数据。为了减少您合并多次查询结果的工作量，服务端会把缓存命中的查询结果与本次查询新命中的结果合并返回给您。因此，日志服务可以让您通过以相同参数反复调用该接口来获取最终完整结果。因为您的查询涉及的日志数据量变化非常大，日志服务 API 无法预测需要调用多少次该接口而获取完整结果。所以需要您通过检查每次请求的返回结果中的 progress 成员状态值来确定是否需要继续。需要注意的是，每次重复调用该接口都会重新消耗相同数量的查询 CU。

 **错误码** 

除了返回 Log Service API 的 [通用错误码](intl.zh-CN/API 参考/通用错误码.md)，还可能返回如下特有错误码：

|HTTP 状态码（Status Code）|错误码（Error Code）|错误消息（Error Message）|描述（Description）|
|:--------------------|:--------------|:------------------|:--------------|
|404|LogStoreNotExist|logstore \{Name\} does not exist.|日志库（logstore）不存在。|
|400|InvalidTimeRange|request time range is invalid.|请求的时间区间无效。|
|400|InvalidQueryString|query string is invalided|请求的查询字符串无效。|

**说明：** 上表错误消息中 \{name\} 表示该部分会被具体的 LogstoreName 来替换。

## 示例 {#section_e5p_d4t_12b .section}

以杭州地域内名为 big-game 的 project 为例，查询该 project 内名为 app\_log 的 Logstore 中，主题为 groupA 的日志分布情况。查询区间为 2014-09-01 00:00:00 到 2014-09-01 22:00:00，查询关键字是 error。

 **请求示例：** 

``` {#codeblock_mnr_e1r_95j}
GET /logstores/app_log?type=histogram&topic=groupA&from=1409529600&to=1409608800&query=error HTTP/1.1
Authorization: LOG <yourAccessKeyId>:<yourSignature>
Date: Wed, 3 Sept. 2014 08:33:46 GMT
Host: big-game.cn-hangzhou.log.aliyuncs.com
x-log-bodyrawsize: 0
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

 **响应示例：** 

``` {#codeblock_vx5_ga3_5nc}
HTTP/1.1 200 OK
Content-Type: application/json
Content-MD5: E6AD9C21204868C2DE84EE3808AAA8C8
Content-Type: application/json
Date: Wed, 3 Sept. 2014 08:33:47 GMT
Content-Length: 232
x-log-requestid: efag01234-12341-15432f
[
        {
            "from": 1409529600,
            "to": 1409569200,
            "count": 2,
            "progress": "Complete"
        },
        {
            "from": 1409569200,
            "to": 1409608800,
            "count": 1,
            "progress": "Incomplete"
        }
]
```

在这个响应示例中，服务端把整个 Histogram 划分到两个均等的时间区间，分别为 \[2014-09-01 00:00:00, 2014-09-01 11:00:00） 和 \[2014-09-01 11:00:00, 2014-09-01 22:00:00）。由于您的数据量过大，第一次查询会返回不完整数据。响应结果告知您命中日志条数为 3，但整体结果不完整，仅在时间段 \[2014-09-01 00:00:00 2014-09-01 11:00:00）结果完整且命中 2 条，而在另外一个时间段结果不完整但已经命中 1 条。在这个情况下，如果您希望得到完整结果，则需要重复调用上面的请求示例若干次直到整个响应中的 progress 成员值变成 Complete （例如下响应）：

``` {#codeblock_lmo_y7z_5qf}
HTTP/1.1 200 OK
Content-Type: application/json
Content-MD5: E6AD9C21204868C2DE84EE3808AAA8C8
Content-Type: application/json
Date: Wed, 3 Sept. 2014 08:33:48 GMT
Content-Length: 232
x-log-requestid: afag01322-1e241-25432e
[
        {
            "from": 1409529600,
            "to": 1409569200,
            "count": 2,
            "progress": "Complete"
        },
        {
            "from": 1409569200,
            "to": 1409608800,
            "count": 2,
            "progress": "complete"
        }
]
```

