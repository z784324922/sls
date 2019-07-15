# GetLogs {#reference_bqw_wv5_zdb .reference}

You can call this operation to query logs in a Logstore of a specific project. You can also specify relevant parameters to query logs that meet the specified conditions.

When a log is written to a Logstore, the latency of querying this log by using the GetHistograms and GetLogs operations of Log Service varies with the log type. Based on the log timestamp, Log Service classifies logs into the following types:

-   Real-time data: Logs of this type are generated within the interval of \(-180 seconds, 900 seconds\] based on the current server time. For example, if a log was generated at 2014-09-25 12:03:00 UTC and the server received the log at 2014-09-25 12:05:00 UTC, the server processes the log as real-time data. Such logs are usually generated in normal scenarios.
-   Historical data: Logs of this type are generated within the interval of \[-7 × 86400 seconds, -180 seconds\) based on the current server time. For example, if a log was generated at 2014-09-25 12:00:00 UTC and the server received the log at 2014-09-25 12:05:00 UTC, the server processes the log as historical data. Such logs are usually generated in data supplement scenarios.

After real-time data is written, it can be queried with a maximum latency of 3 seconds. Data can be queried within 1 second in 99.9% of cases.

## Request syntax {#section_j5v_14t_12b .section}

```
GET /logstores/<logstorename>? type=log&topic=<logtopic>&from=<starttime>&to=<endtime>&query=<querystring>&line=<linenum>&offset=<startindex>&reverse=<true|false> HTTP/1.1
Authorization: <AuthorizationString>
Date: <GMT Date>
Host: <Project Endpoint>
x-log-bodyrawsize: 0
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

## Request parameters {#section_mx5_b4t_12b .section}

|Parameter|Type|Required|Description|
|:--------|:---|:-------|:----------|
|logstorename|String|Yes|The name of the Logstore where logs are to be queried.|
|type|String|Yes|The type of data to be queried in the Logstore. Set this parameter to log for the GetLogs operation.|
|from|Integer|Yes|The start time of queried data, in seconds. Set this parameter to the number of seconds that have elapsed since 1970-1-1 00:00:00 UTC.|
|to|Integer|Yes|The end time of queried data, in seconds. Set this parameter to the number of seconds that have elapsed since 1970-1-1 00:00:00 UTC.|
|topic|String|No|The topic of logs to be queried.|
|query|String|No|The query statement. For more information about the query syntax, see [Query syntax](../../../../reseller.en-US/User Guide/Index and query/Query/Query syntax.md).|
|line|Integer|No|The maximum number of logs to return for the request. Valid values: \[0, 100\]. Default value: 100.|
|offset|Integer|No|The start point of the specified time interval for the request. Valid values: 0 and positive integers. Default value: 0.|
|reverse|Boolean|No|Specifies whether to return logs in reverse order based on the log timestamp, in minutes. A value of true indicates that logs are returned in reverse order. A value of false indicates that logs are returned in regular order. Default value: false.|

**Request header fields**

The GetLogs operation does not require special request header fields. For more information about the common request header fields of Log Service operations, see [Common request header fields](reseller.en-US/API Reference/Public request header.md).

**Response header fields**

For more information about the common response header fields of Log Service operations, see [Common response header fields](reseller.en-US/API Reference/Public response header.md).

The response header contains specific fields that indicate whether the query results returned for a request are complete. The following table describes these response header fields.

|Field|Type|Description|
|:----|:---|:----------|
|x-log-progress|String|The status of the query results. The value can be Incomplete or Complete, which indicates whether the results are complete.|
|x-log-count|Integer|The total number of logs in the current query results.|

**Response parameters**

After a request is sent for the GetLogs operation, the response body contains logs that meet the specified query conditions. If a large number of logs \(in units of TB\) need to be queried, the response of the GetLogs operation may be incomplete. The response body of the GetLogs operation is an array, and each element in this array is a log. The following table describes the structure of each element in the array.

|Parameter|Type|Description|
|:--------|:---|:----------|
|\_\_time\_\_|Integer|The timestamp of the log, in seconds. The value indicates the number of seconds that have elapsed since 1970-1-1 00:00:00 UTC.|
|\_\_source\_\_|String|The source of the log, which is specified when the log is written.|
|\[content\]|Key-value pair|The original content of the log, which is organized in key-value pairs.|

**Detailed description**

-   The time interval defined by the request parameters from and to for this operation is left-closed and right-open. That is, the time interval includes the start time but excludes the end time. If the values of the from and to parameters are the same, the time interval is invalid and the server directly returns an error.
-   The server must return a response to each request for this operation within a specified time. For each query, the server can scan only a specified number of logs. If the server needs to process a large number of logs for a request, it may return incomplete query results. Whether the results are complete is indicated by the x-log-progress field in the response header. At the same time, the server caches the query results within 15 minutes. If some query results of a request hit the data in the cache, the server continues to scan the logs that do not hit any data in the cache for this request. To reduce your workload of merging multiple query results, the server merges the query results that hit the data in the cache with the results newly scanned in the current query, and then returns the merged results to you. In this manner, Log Service allows you to call the GetLogs operation multiple times with the same parameters to obtain the final complete results. Log Service cannot predict how many times it must call the GetLogs operation before obtaining the complete results because the volume of logs to be queried changes dramatically. Therefore, you must check the value of the x-log-progress field in the returned results of each request to determine whether to continue the query. You must note that each call of this operation consumes the same number of compute units \(CUs\).

**Error codes**

In addition to the [common errors](reseller.en-US/API Reference/Common error codes.md) of Log Service operations, this operation also returns some specific errors, as listed in the following table.

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|:----------|
|404|LogStoreNotExist|logstore \{Name\} does not exist.|The error message returned because the specified Logstore does not exist.|
|400|InvalidTimeRange|request time range is invalid|The error message returned because the specified time interval of the request is invalid.|
|400|InvalidQueryString|query string is invalid|The error message returned because the specified query string of the request is invalid.|
|400|InvalidOffset|offset is invalid|The error message returned because the specified offset parameter of the request is invalid.|
|400|InvalidLine|line is invalid|The error message returned because the specified line parameter of the request is invalid.|
|400|InvalidReverse|Reverse value is invalid|The error message returned because the specified reverse parameter is invalid.|
|400|IndexConfigNotExist|logstore without index config|The error message returned because the index feature has not been enabled for the Logstore.|

**Note:** The \{name\} variable in the preceding error message is replaced with a specific Logstore name.

## Examples {#section_e5p_d4t_12b .section}

Take a project named big-game in China \(Hangzhou\) as an example. Query the logs whose topic is groupA in the app\_log Logstore of the big-game project. The time interval for this query is 2014-09-01 00:00:00 to 2014-09-01 22:00:00, and the query keyword is error. The query starts from the start time of the time interval, and a maximum of 20 logs are returned.

**Sample request**

```
GET /logstores/app_log? type=log&topic=groupA&from=1409529600&to=1409608800&query=error&line=20&offset=0 HTTP/1.1
Authorization: <AuthorizationString>
Date: Wed, 3 Sept. 2014 08:33:46 GMT
Host: big-game.cn-hangzhou.log.aliyuncs.com
x-log-bodyrawsize: 0
x-log-apiversion: 0.4.0
x-log-signaturemethod: hmac-sha1
```

**Sample success response**

```
HTTP/1.1 200 OK
Content-MD5: 36F9F7F0339BEAF571581AF1B0AAAFB5
Content-Type: application/json
Content-Length: 269
Date: Wed, 3 Sept. 2014 08:33:47 GMT
x-log-requestid: efag01234-12341-15432f
x-log-progress: Complete
x-log-count: 10000
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

In this sample response, the value of the x-log-progress field is Complete, which indicates that the log query is completed and the returned results are complete. For this request, two logs meet the specified query conditions and are displayed as the value of the logs parameter. If the value of the x-log-progress field is Incomplete in the response, you must repeat the request to obtain the complete results.

