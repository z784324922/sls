# GetHistograms {#reference_oxy_xv5_zdb .reference}

Query the log distribution in a Logstore of a specific project. You can also query the distribution of logs that meet the specific conditions by specifying the relevant parameters.

When a log is written to the Logstore, the latency of querying this log by using Log Service query APIs \(GetHistograms and GetLogs\) varies according to the log type. Log Service classifies logs based on the log timestamp into the following two types:

-   Real-time data: The time point in a log is the current time point on the server \(-180 seconds, 900 seconds\]. For example, if the log time is UTC 2014-09-25 12:03:00 and the time when the server receives the log is UTC 2014-09-25 12:05:00, the log is processed as the real-time data, which usually appears in normal scenarios.
-   Historical data: The time point in a log is the current time point on the server \(-7 x 86400 seconds, -180 seconds\].  For example, if the log time is UTC 2014-09-25 12:00:00 and the time when the server receives the log is UTC 2014-09-25 12:05:00, the log is processed as the historical data, which usually appears in the supplementary data scenario.

The latency between real-time data writing and query is 3 seconds.

## Request syntax {#section_j5v_14t_12b .section}

```
GET /logstores/<logstorename>? type=histogram&topic=<logtopic>&from=<starttime>&to=<endtime>&query=<querystring> HTTP/1.1
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
|logstorename|string|Yes|The name of the Logstore where the log to be queried belongs.|
|type|string|Yes|The type of Logstore data to be queried. This parameter must be histogram in GetHistograms API.|
|from|integer|Yes|The query start time \(the number of seconds since 1970-1-1 00:00:00 UTC\).|
|to|integer|Yes|The query end time \(the number of seconds since 1970-1-1 00:00:00 UTC\).|
|topic|string|No|The topic of the log to be queried.|
|query|string|No|The query expression. For more information about the query expression syntax, see [Query syntax](../../../../intl.en-US/User Guide/Index and query/Query syntax.md).|

**Request header**

The GetHistograms API does not have a special request header. For more information about the public request headers of Log Service APIs, see [Public request header](intl.en-US/API Reference/Public request header.md).

**Response header**

The GetHistograms API does not have a special response header.  For more information about the public response headers of Log Service APIs, see [Public response header](intl.en-US/API Reference/Public response header.md).

**Response element**

After the successful request, the response body contains the distribution of logs that meet the query conditions on the timeline. The response results evenly divide the time range into several \(1–60\) subintervals and return the number of logs that meet the query conditions in each subinterval. Log Service must return results within a specified time to guarantee the timeliness. Therefore, each query can only scan a specified number of logs. The response results of this API may be incomplete when the log volume to be queried is large. A special response element is used to indicate whether or not the returned results of the request is complete. See the following specific response element formats.

|Name|Type|Description|
|:---|:---|:----------|
|progress|string|The status of the query results. The two optional values Incomplete and Complete indicate whether or not the results are complete.|
|count|integer|The total number of logs in the current query results.|
|histograms|array|The distribution of the current query results in the subintervals. For more information about the structure, see the following table.|

The structure of each element in the histograms array is as follows.

|Name|Type|Description|
|:---|:---|:----------|
|from|integer|The start time for the subinterval \(the number of seconds since 1970-1-1 00:00:00 UTC\).|
|to|integer|The end time for the subinterval \(the number of seconds since 1970-1-1 00:00:00 UTC\).|
|count|integer|The number of logs that meet the query conditions for this subinterval in the current query results.|
|progress|string|Whether or not the current query results in this subinterval are complete. Optional values: Incomplete and Complete.|

**Detailed description**

-   All the time intervals in this API, whether the time intervals defined by the request parameters from and to, or subintervals in the returned results, follow the left-closed and right-opened principle, that is, the time interval includes the start time, but not the end time. If the from and to values are the same, the time interval is invalid and the function returns an error directly.
-   The subinterval division method in the response of this API is consistent and unchanging. The subinterval division in the response does not change if the time interval of your request does not change.
-   Each call to this API must return results within a specified time, and each query can only scan a specified number of logs. The results returned from this request are incomplete if the log volume to be processed for this request is large \(whether or not the results are complete is indicated by using the progress in the returned results\). At the same time, Log Service caches the query results within 15 minutes. If some query request results are the same as those in the cache, Log Service continues to scan the logs that are not in the cache for this request. To reduce the workload of merging multiple query results, Log Service merges the query results that are the same as those in the cache and the results newly scanned in this query, and then returns them to you. Therefore, Log Service allows you to call the API multiple times with the same parameter to obtain the final complete results. Log Service API cannot predict how many times the API must be called before obtaining the complete results because the log volume to be queried changes massively. Therefore, you must check the progress value in the returned results of each request to determine whether or not to continue the query. You must note that each call to this API consumes the same number of query CUs again.

**Error code**

Besides the [common error codes](intl.en-US/API Reference/Common error codes.md) of Log Service APIs, the GetHistograms API may return the following special error codes.

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|:----------|
|404|LogStoreNotExist|logstore \{Name\} does not exist.|The Logstore does not exist.|
|400|InvalidTimeRange|request time range is invalid.|The time interval of the request is invalid.|
|400|InvalidQueryString|query string is invalided|The query string of the request is invalid.|

**Note:** The \{name\} in the preceding error message is replaced by a specific Logstore name.

## Example {#section_e5p_d4t_12b .section}

ake a project named big-game in the region Hangzhou as an example. Query the distribution of logs whose topic is groupA in the app\_log Logstore of the big-game project. The time interval for this query is 2014-09-01 00:00:00–2014-09-01 22:00:00. The keyword for this query is error.

**Request example**

```
GET /logstores/app_log? type=histogram&topic=groupA&from=1409529600&to=1409608800&query=error HTTP/1.1
Authorization: <AuthorizationString>
Date: Wed, 3 Sept. 2014 08:33:46 GMT
Host: big-game.cn-hangzhou.log.aliyuncs.com
x-log-bodyrawsize: 0
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

**响应示例：**

```
HTTP/1.1 200 OK
Content-Type: application/json
Content-MD5: E6AD9C21204868C2DE84EE3808AAA8C8
Content-Type: application/json
Date: Wed, 3 Sept. 2014 08:33:47 GMT
Content-Length: 232
x-log-requestid: efag01234-12341-15432f
{
    "progress": "Incomplete",
    "count": 3,
    "histograms": [
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
}
```

In this response example, Log Service divides the entire Histogram into two equal time intervals: \[2014-09-01 00:00:00, 2014-09-01 11:00:00\) and \[2014-09-01 11:00:00, 2014-09-01 22:00:00\). The returned results of the first query are incomplete because your log volume to be queried is large. The response results indicate that three logs meet the query conditions, but the overall results are incomplete. Only the results in the time interval \[2014-09-01 00:00:00, 2014-09-01 11:00:00\) are complete, with two logs meeting the query conditions. However, the results in the other time interval are incomplete, with one log meeting the query conditions. In this situation, to obtain the complete results, you must call the preceding request example multiple times until the progress value in the response changes to Complete as follows.

```
HTTP/1.1 200 OK
Content-Type: application/json
Content-MD5: E6AD9C21204868C2DE84EE3808AAA8C8
Content-Type: application/json
Date: Wed, 3 Sept. 2014 08:33:48 GMT
Content-Length: 232
x-log-requestid: afag01322-1e241-25432e
{
    "progress": "Incomplete",
    "count": 4,
    "histograms": [
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
}
```

