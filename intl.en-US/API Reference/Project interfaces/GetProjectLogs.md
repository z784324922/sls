# GetProjectLogs {#reference_krh_v5q_12b .reference}

GetProjectLogs is the SQL query interface of the Project level.

## Request syntax {#section_x1q_tdf_jdb .section}

```
GET /logs/? query=SELECT * FROM <logStoreName> where __line__ = 'abc' and __date__ >'2017-09-01 00:00:00' and __date__ < '2017-09-02 00:00:00'&line=20&offset=0 HTTP/1.1
Authorization: <AuthorizationString>
Date: Wed, 3 Sept. 2014 08:33:46 GMT
Host: big-game.cn-hangzhou.log.aliyuncs.com
x-log-bodyrawsize: 0
x-log-apiversion: 0.4.0
x-log-signaturemethod: hmac-sha1
```

## Request parameters {#section_ghf_vdf_jdb .section}

|Attribute name|Type |Required or not|Description|
|--------------|-----|---------------|-----------|
|query|string|Yes|The SQL query condition.|

## Request header {#section_l3t_b2f_jdb .section}

The GetProjectLogs API does not have a special request header. For more information about the public request headers of Log Service APIs, see [Public request header](reseller.en-US/API Reference/Public request header.md).

## Response header {#section_whj_d2f_jdb .section}

For more information about the public response headers of Log Service APIs, see [Public response header](reseller.en-US/API Reference/Public response header.md).

The response header has a special element to indicate whether the returned results of the request are complete.  The specific response element format is as follows.

|Name |类型|Description|
|-----|--|-----------|
|x-log-progress|String|The status of the query results.  The two values, Incomplete and Complete, indicate whether the results are complete.|
|x-log-count|Integer|The total number of logs in the current query results.|
|x-log-processed-rows|Integer|The number of rows processed in this calculation.|
|x-log-elapsed-millisecond|Integer|The time \(in milliseconds\) spent in this calculation.|

## Response element {#section_zsm_k2f_jdb .section}

After the successful request, the response body contains the computing results. The response body of GetProjectLogs is an array, and each element in the array is a log. The element formats are as follows.

|Parameter|类型|Required or not|
|---------|--|---------------|
|\_\_time\_\_|Integer|The log timestamp \(the number of seconds since 1970-1-1 00:00:00 UTC\).|
|\_\_source\_\_|String|The log source, which is specified when logs are written.|
|\[content\]|Key-Value pair|The original content of the log, which is organized in Key-Value pairs.|

## Detailed description {#section_plj_42f_jdb .section}

-   The query of this API is a standard SQL query statement.
-   Specify the project you want to query in the domain name of the request.
-   Specify the Logstore you want to query in the FROM condition of the query statement.  Logstore is equivalent to the table in SQL.
-   You must specify the time range you want to query in the SQL query condition. The time range is specified by \_\_date\_\_ \(timestamp type\) or \_\_time\_\_ \(integer  type, the unit is in UNIX time\).
-   Each call to this API must return results within a specified time, and each query can only scan a specified number of logs.  The results returned from this request are incomplete if the log volume to be processed for this request is large \(whether the results are complete is indicated by using the  x-log-progress in the response header\). At the same time, Log Service caches the query results within 15 minutes. When a portion of the query request results are cache hits, the server continues to scan the log data that are not cache hits for this request. To reduce the workload of merging multiple query results, the server cache hit query results and the new hits of the current query are merged and returned to you. Therefore, the Log Service allows you to call the interface multiple times with the same parameters to finally obtain the complete results. Log Service API cannot predict how many times the  APIs must be called before obtaining the complete results because the log volume to be queried changes massively.  Therefore, you must check the x-log-progress status in the returned results of each request to determine whether to continue the query.  You must note that each call to this API consumes the same number of query CUs.

## Error code {#section_uhx_p2f_jdb .section}

Besides the [common error codes](reseller.en-US/API Reference/Common error codes.md) of Log Service APIs, the GetProjectLogs API may return the following special error codes.

|HTTP status code  |Error code|Error message|Description|
|------------------|----------|-------------|-----------|
|400 |ParameterInvalid|Parameter is invalid|The request parameter is invalid. For more information, see the detailed error message.|

## Samples {#section_pyd_v2f_jdb .section}

Take a project named big-game in the region Hangzhou as an example. Query the logs whose topic is groupA in the app\_log Logstore of the big-game project.  The time interval for this query is  2014-09-01 00:00:00–2014-09-01 22:00:00. The keyword for this query is error. The query starts from the beginning of the time interval, and a maximum of 20 logs are returned.

## Request example {#section_cpt_w2f_jdb .section}

```
GET /logs/? query=SELECT * FROM <logStoreName> where __line__ = 'abc' and __date__ >'2017-09-01 00:00:00' and __date__ < '2017-09-02 00:00:00'&line=20&offset=0 HTTP/1.1
Authorization: <AuthorizationString>
Date: Wed, 3 Sept. 2014 08:33:46 GMT
Host: big-game.cn-hangzhou.log.aliyuncs.com
x-log-bodyrawsize: 0
x-log-apiversion: 0.4.0
x-log-signaturemethod: hmac-sha1
```

## Response example {#section_mvw_x2f_jdb .section}

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

In this response example, the x-log-progress status is Complete, which indicates the log query is completed and the returned results are complete.  For this request, two logs meet the query condition and are displayed as the  values of logs.  If the x-log-progress status is Incomplete in the response result, you must repeat the request to obtain the complete results. 

