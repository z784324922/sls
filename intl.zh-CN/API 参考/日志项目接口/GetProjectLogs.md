# GetProjectLogs {#reference_krh_v5q_12b .reference}

统计Project下所有日志。

## 请求语法 {#section_x1q_tdf_jdb .section}

```
GET /logs/?query=SELECT * FROM <logStoreName> where __line__ = 'abc' and __date__ >'2017-09-01 00:00:00' and __date__ < '2017-09-02 00:00:00'&line=20&offset=0 HTTP/1.1
Authorization: <AuthorizationString>
Date: Wed, 3 Sept. 2014 08:33:46 GMT
Host: big-game.cn-hangzhou.log.aliyuncs.com
x-log-bodyrawsize: 0
x-log-apiversion: 0.4.0
x-log-signaturemethod: hmac-sha1
```

## 请求参数 {#section_ghf_vdf_jdb .section}

|属性名称|类型|是否必须|描述|
|----|--|----|--|
|query|string|是|查询SQL条件|

## 请求头 {#section_l3t_b2f_jdb .section}

GetProjectLogs接口无特有请求头。关于 API 的公共请求头，请参考 [公共请求头](intl.zh-CN/API 参考/公共请求头.md)。

## 响应头 {#section_whj_d2f_jdb .section}

关于 API 的公共响应头，请参考 [公共响应头](intl.zh-CN/API 参考/公共响应头.md)。

响应头中有专门成员表示请求返回结果是否完整。具体响应元素格式如下：

|名称|类型|描述|
|--|--|--|
|x-log-progress|字符串|查询结果的状态。可以有 Incomplete 和 Complete 两个选值，表示本次是否完整。|
|x-log-count|整型|当前查询结果的日志总数。|
|x-log-processed-rows|整型|本次计算处理的行数。|
|x-log-elapsed-millisecond|整型|本次计算消耗的毫秒时间。|

## 响应元素 {#section_zsm_k2f_jdb .section}

请求成功，其响应 Body 会包括计算结果，GetProjectLogs的响应body是一个数组，数组中每个元素是一条日志结果。数组中的每个元素结构如下：

|名称|类型|描述|
|--|--|--|
|\_\_time\_\_|整型|日志的时间戳（精度为秒，从 1970-1-1 00:00:00 UTC 计算起的秒数）。|
|\_\_source\_\_|字符串|日志的来源，由写入日志时指定。|
|\[content\]|Key-Value对|日志原始内容，以 Key-value 对的形式组织。|

## 细节描述 {#section_plj_42f_jdb .section}

-   该接口的query是一个标准的SQL查询语句。
-   查询的Project，在请求的域名中指定。
-   查询的logstore，在查询语句的from条件中指定。logstore相当于SQL中的表。
-   在查询的SQL条件中，必须指定要查询的时间范围，时间范围由\_\_date\_\_\(timestamp类型\)来指定，或\_\_time\_\_\(int 类型，单位是unix\_time\)来指定。
-   如上所述，该接口一次调用必须要在限定时间内返回结果，每次查询只能扫描指定条数的日志量。如果一次请求需要处理的数据量非常大的时候，该请求会返回不完整的结果（并在返回结果中的 x-log-progress 成员标示是否完整）。如此同时，服务端会缓存 15 分钟内的查询结果。当查询请求的结果有部分被缓存命中，则服务端会在这次请求中继续扫描未被缓存命中的日志数据。为了减少您合并多次查询结果的工作量，服务端会把缓存命中的查询结果与本次查询新命中的结果合并返回给您。因此，日志服务可以让您通过以相同参数反复调用该接口来获取最终完整结果。因为您的查询涉及的日志数据量变化非常大，日志服务 API 无法预测需要调用多少次该接口而获取完整结果。所以需要用户通过检查每次请求的返回结果中的x-log-progress成员状态值来确定是否需要继续。需要注意的是，每次重复调用该接口都会重新消耗相同数量的查询 CU。

## 特有错误码 {#section_uhx_p2f_jdb .section}

GetProjectLogs 接口除了返回 API 的 [通用错误码](intl.zh-CN/API 参考/通用错误码.md)，还可能返回如下特有错误码：

|HTTP状态码（Status Code）|错误码（Error Code）|错误消息（Error Message）|描述（Description）|
|--------------------|---------------|-------------------|---------------|
|400|ParameterInvalid|parameter is invalid|请求的参数错误，具体错误参考错误的message。|

## 示例 {#section_pyd_v2f_jdb .section}

以杭州地域内名为 big-game 的 Project 为例，查询该 Project 内名为 app\_log 的 Logstore 中，主题为 groupA 的日志数据。查询区间为 2014-09-01 00:00:00 到 2014-09-01 22:00:00，查询关键字为 error，且从时间区间头开始查询，最多返回 20 条日志数据。

## 请求示例 {#section_cpt_w2f_jdb .section}

```
GET /logs/?query=SELECT * FROM <logStoreName> where __line__ = 'abc' and __date__ >'2017-09-01 00:00:00' and __date__ < '2017-09-02 00:00:00'&line=20&offset=0 HTTP/1.1
Authorization: <AuthorizationString>
Date: Wed, 3 Sept. 2014 08:33:46 GMT
Host: big-game.cn-hangzhou.log.aliyuncs.com
x-log-bodyrawsize: 0
x-log-apiversion: 0.4.0
x-log-signaturemethod: hmac-sha1
```

## 响应示例 {#section_mvw_x2f_jdb .section}

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

