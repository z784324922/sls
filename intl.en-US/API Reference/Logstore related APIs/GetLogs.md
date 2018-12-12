# GetLogs {#reference_bqw_wv5_zdb .reference}

Query logs in a Logstore of a specific project. You can also query the logs that meet the specific condition by specifying the relevant parameters.

When a log is written to the Logstore, the latency of querying this log by using Log Service query APIs \(GetHistograms and GetLogs\) varies according to the log type. Log Service classifies logs based on the log timestamp into the following two types:

-   Real-time data: The time point in a log is the current time point on the server \(-180 seconds, 900 seconds\]. For example, if the log time is UTC 2014-09-25 12:03:00 and the time when the server receives the log is UTC 2014-09-25 12:05:00, the log is processed as the real-time data, which usually appears in normal scenarios.
-   Historical data: The time point in a log is the current time point on the server \(-7 x 86400 seconds, -180 seconds\]. For example, if the log time is UTC 2014-09-25 12:00:00 and the time when the server receives the log is UTC 2014-09-25 12:05:00, the log is processed as the historical data,  which usually appears in the supplementary data scenario.

The maximum latency between real-time data writing and query is 3 seconds. \(data can be queried within one second in 99.9% cases\).

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

## Request parameters {#section_mx5_b4t_12b .section}

|Parameter name|Type|Required|Description|
|:-------------|:---|:-------|:----------|
|logstorename|string|Yes|The name of the Logstore where the log to be queried belongs. |
|type|string|Yes|The type of Logstore data to be queried. This parameter must be log in GetLogs API.|
|from|整型|Yes| The query start time \(the number of seconds since 1970-1-1 00:00:00 UTC\). |
|to|整型|Yes|The query end time \(the number of seconds since 1970-1-1 00:00:00 UTC\).|
|topic|string|No |The topic of the log to be queried.|
|query|string|No |The query expression. For more information about the query expression syntax, see  [Query syntax](https://help.aliyun.com/document_detail/29060.html).|
|line|整型|No |The maximum number of log lines returned by the request. The maximum number of logs returned from the request.|
|The value range is 0–100 and the default value is 100.|整型|No | The returned log start point of the request. The value can be 0 or a positive integer. The default value is 0. |
|reverse|boolean|No |Whether or not logs are returned in reverse order according to the log timestamp.  true indicates reverse order and false indicates sequent order. The default value is false .|

**Request header**

The GetLogs API does not have a special request header. For more information about the public request headers of Log Service APIs, see [Public request header](reseller.en-US/API Reference/Public request header.md).

**Response header**

For more information about the public response headers of Log Service APIs, see [Public response header](reseller.en-US/API Reference/Public response header.md).

The response header has special elements to indicate whether or not the returned results of the request is complete.  See the following specific response element formats.

|Parameter name|Type|Description|
|:-------------|:---|:----------|
|x-log-progress|string|The status of the query results.  The status of the query results. The two optional values Incomplete and Complete indicate whether or not the results are complete.|
|x-log-count|整型|The total number of logs in the current query results.|

**Response element**

After the successful request, the response body contains the logs that meet the query conditions. The response results of this API may be incomplete when the log volume to be queried is large \(T-level\). The response body of GetLogs API is an array, and each element in this array is a log. The structure of each element in this array is as follows.

|Parameter name|Type|Description|
|:-------------|:---|:----------|
|\_\_time\_\_|整型|The log timestamp \(the number of seconds since 1970-1-1 00:00:00 UTC\).|
|\_\_source\_\_|string|The log source, which is specified when logs are written.|
|\[content\]|key-value pair|The original content of the log, which is organized in key-value pairs.|

**Detailed description**

-   The time interval defined by the request parameters from and to in this API follows the left-closed and right-opened principle, that is, the time interval includes the start time, but not the end time. If the from and to values are the same, the time interval is invalid and the function returns an error directly.
-   Each call to this API must return results within a specified time, and each query can only scan a specified number of logs. The results returned from this request are incomplete if the log volume to be processed for this request is large \(whether or not the results are complete is indicated by using the x-log-progress in the response header\).  At the same time, Log Service caches the query results within 15 minutes. If some query request results are the same as those in the cache, Log Service continues to scan the logs that are not in the cache for this request. To reduce the workload of merging multiple query results, Log Service merges the query results that are the same as those in the cache and the results newly scanned in this query, and then returns them to you. Therefore, Log Service allows you to call the API multiple times with the same parameter to obtain the final complete results. Log Service API cannot predict how many times the API must be called before obtaining the complete results  The API cannot predict how many times it needs to call the interface to get the full result. because the log volume to be queried changes massively. Therefore, you must check the x-log-progress status in the returned results of each request to determine whether or not to continue the query. You must note that each call to this API consumes the same number of query CUs again.

**Error code**

Besides the [common error codes](reseller.en-US/API Reference/Common error codes.md) of Log Service APIs, the GetLogs API may return the following special error codes.

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|:----------|
|404|LogStoreNotExist|logstore \{Name\} does not exist.|The Logstore does not exist.|
|400|InvalidTimeRange|request time range is invalid|The time interval of the request is invalid.|
|400|InvalidQueryString|query string is invalid|The query string of the request is invalid.|
|400|InvalidOffset|offset is invalid|The offset parameter of the request is invalid.|
|400|InvalidLine|line is invalid|The line parameter of the request is invalid.|
|400|InvalidReverse|Reverse value is invalid|The Reverse parameter value is invalid.|
|400|IndexConfigNotExist|logstore without index config|The Logstore does not enable the index.|

**Note:** The \{name\} in the preceding error message is replaced by a specific Logstore name.

## Example {#section_e5p_d4t_12b .section}

Take a project named big-game in the region Hangzhou as an example. Query the logs whose topic is groupA in the app\_log Logstore of the big-game project. The time interval for this query is 2014-09-01 00:00:00–2014-09-01 22:00:00. The keyword for this query is error. The query starts from the beginning of the time interval, and a maximum of 20 logs are returned.

**Request example**

```
GET /logstores/app_log？ type=log&topic=groupA&from=1409529600&to=1409608800&query=error&line=20&offset=0 HTTP/1.1
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

In this response example, x-log-progress In this response example, the x-log-progress status is Complete , which indicates the log query is completed and the returned results are complete. For this request, two logs meet the query condition and are displayed as the values of logs. If the x-log-progress status is Incomplete in the response result, you must repeat the request to obtain the complete results.

