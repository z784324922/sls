# GetCursor {#concept_ffg_3dm_12b .concept}

The GetCursor API is used to get the cursor based on the time. The following figure shows the relationship among the project, Logstore, shard, and cursor.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13233/15529199656710_en-US.png)

-   A project has multiple Logstores.
-   Each Logstore has multiple shards.
-   You can get the location of a specified log by using the cursor.

## Request syntax {#section_j5v_14t_12b .section}

```
GET /logstores/ay42/shards/2? type=cursor&from=1402341900 HTTP/1.1
Authorization: <AuthorizationString>
Date: <GMT Date>
Host: <Project Endpoint>
x-log-apiversion: 0.6.0
```

## Request parameter {#section_mx5_b4t_12b .section}

|Parameter Name|Type|Required |Description|
|:-------------|:---|:--------|:----------|
|shard|string|Yes|The Shard.|
|type|string|Yes| The cursor.|
|from|string|Yes|The time point \(in UNIX format and measured in seconds\). The from\_time, begin\_time, or end\_time.|

**Logstore lifecycle**

The lifecycle of a Logstore is specified by the lifeCycle field in the attribute.  For example, the current time is 2015-11-11 09:00:00  and lifeCycle=24.  Then, the data time period that can be consumed in each shard is \[2015-11-10 09:00:00,2015-11-11  09:00:00\) and the time here is the server time.

By using the parameter from, you can locate the logs within the lifecycle in the shard. Assume that the Logstore lifecycle is \[begin\_time,end\_time\) and the parameter from is set to from\_time, then: \[begin\_time,end\_time\), from=from\_time.

```
  from_time <= begin_time or from_time == "begin" : Returns the cursor location corresponding to begin_time.
 from_time >= end_time or from_time == "end" : Returns the cursor location for writing the next entry at the current time point (no data at this cursor location currently).
  from_time > begin_time and from_time < end_time : Returns the cursor location for the first data packet whose receipt time at the server is >= from_time.
```

**Request header**

The GetCursor API does not have a special request header.  For more information about  the public request headers of Log Service APIs, see  [Public request header](reseller.en-US/API Reference/Public request header.md).

**Response header**

The GetCursor API does not have a special response header.  For more information about the public response headers of Log Service APIs, see [Public response header](reseller.en-US/API Reference/Public response header.md).

**Response element**

```
{
    "cursor": "MTQ0NzI5OTYwNjg5NjYzMjM1Ng=="
}
```

**Error code**

Besides  the  [common error codes](reseller.en-US/API Reference/Common error codes.md) of Log Service APIs, the GetCursor API may return the following special error codes.

|HTTP status code|ErrorCode|ErrorMessage|
|:---------------|:--------|:-----------|
|404|LogStoreNotExist|Logstore \{Name\} does not exist|
|400|ParameterInvalid|Parameter From is not valid|
|400|ShardNotExist|Shard \{ShardID\} does not exist|
|500|InternalServerError|Specified Server Error Message|
|400|LogStoreWithoutShard|the logstore has no shard|

## Example {#section_e5p_d4t_12b .section}

**Request example:**

```
GET /logstores/sls-test-logstore/shards/0? type=cursor&from=begin
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
    "Authorization": "LOG AK\_ID:Signature"
}
```

**Response example:**

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

