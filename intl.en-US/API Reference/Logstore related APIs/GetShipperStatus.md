# GetShipperStatus {#reference_eg2_ztq_12b .reference}

Query the LogShipper task status.

## Request syntax {#section_j5v_14t_12b .section}

```
GET /logstores/{logstoreName}/shipper/{shipperName}/tasks? from=1448748198&to=1448948198&status=success&offset=0&size=100 HTTP/1.1
Authorization: <AuthorizationString> 
Date: <GMT Date>
Host: <Project Endpoint>
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

## Request Parameters {#section_mx5_b4t_12b .section}

|Parameter name|Type| Required| Description|
|:-------------|:---|:--------|:-----------|
|logstoreName|string|Yes|The Logstore name, which is unique in the same project.|
|shipperName|String|Yes|The name of the log shipping rule, which is unique in the same Logstore.|
|from|integer|Yes|The start time of a LogShipper task.|
|to|integer|Yes|The end time of a LogShipper task.|
|status|string|No|The default value is empty, indicating that tasks of any status are returned. Currently, tasks in the successful, running, or failed status are returned.|
|offset|integer|No|The starting number of LogShipper tasks within a specified time range. The default value is 0.|
|size|integer|No|The number of LogShipper tasks within a specified time range. The default value is 100. The maximum value is 500.|

**Request header**

The GetShipperStatus API does not have a special request header.  For more information about the public request headers of Log Service APIs, see [Public request header](intl.en-US/API Reference/Public request header.md).

**Response header**

The GetShipperStatus API does not have a special response header. For more information about the public response headers of Log Service APIs,  see [Public response header](intl.en-US/API Reference/Public response header.md).

**Response element**

After the successful request, the response body contains a list of specified LogShipper tasks.

```
{
    "count" : 10,
    "total" : 20,
    "statistics" : {
        "running" : 0,
        "success" : 20,
        "fail" : 0 
    }
    "tasks" : [
        {
            "id" : "abcdefghijk",
            "taskStatus" : "success",
            "taskMessage" : "",
            "taskCreateTime" : 1448925013,
            "taskLastDataReceiveTime" : 1448915013,
            "taskFinishTime" : 1448926013
        }
    ]
}
```

|Name|Type|Description|
|:---|:---|:----------|
|count|integer|The number of returned tasks.|
|total|integer|The total number of tasks within a specified range.|
|statistics|json|The statistics of task status within a specified range. For more information, see the following table.|
|tasks|array|The details of a LogShipper task within a specified range. For more information, see the following table.|

**Statistics of task status**

|Name|Type|Description|
|:---|:---|:----------|
|running|integer|The number of running tasks within a specified range.|
|success|integer|The number of successful tasks within a specified range.|
|fail|integer|The number of failed tasks within a specified range.|

**Task details**

|Name|Type|Description|
|:---|:---|:----------|
|id|String|The unique ID of a LogShipper task.|
|taskStatus|string|The LogShipper task status, which may be running, successful, and failed.|
|taskMessage|string|The error message appeared when a LogShipper task fails.|
|taskCreateTime|integer|The created time of a LogShipper task.|
|taskLastDataReceiveTime|integer|The time when the server receives the last log of a LogShipper task \(the receipt time on the server, not the log time\).|
|taskFinishTime|integer|The end time of a LogShipper task.|

**Error code**

Besides the [common error codes of Log Service APIs](intl.en-US/API Reference/Common error codes.md), the GetShipperStatus API may return the following special error codes.

|HTTP status code|ErrorCode|ErrorMessage|
|:---------------|:--------|:-----------|
|404|ProjectNotExist|Project \{ProjectName\} does not exist|
|404|LogStoreNotExist|logstore \{logstoreName\} does not exist|
|400|ShipperNotExist|shipper \{logstoreName\} does not exist|
|500|InternalServerError|internal server error|
|400|ParameterInvalid|start time must be earlier than end time|
|400|ParameterInvalid|only support query last 48 hours task status|
|400|ParameterInvalid|status only contains success/running/fail|

**Detailed description**

You can only query LogShipper task status within the last 24 hours.

## Example {#section_e5p_d4t_12b .section}

**Request example**

```
GET /logstores/test-logstore/shipper/test-shipper/tasks? from=1448748198&to=1448948198&status=success&offset=0&size=100 HTTP/1.1
Header:
{
x-log-apiversion=0.6.0, 
Authorization=LOG 94to3z418yupi6ikawqqd370:wFcl3ohVJupCi0ZFxRD0x4IA68A=, 
Host=ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com, 
Date=Wed, 11 Nov 2015 08:28:19 GMT, 
Content-Length=55, 
x-log-signaturemethod=hmac-sha1, 
Content-MD5=757C60FC41CC7D3F60B88E0D916D051E, 
User-Agent=sls-java-sdk-v-0.6.0, 
Content-Type=application/json
}
```

**Response example**

```
HTTP/1.1 200 OK
Header:
{
Date=Wed, 11 Nov 2015 08:28:20 GMT, 
Content-Length=0, 
x-log-requestid=5642FC2399248C8F7B0145FD, 
Connection=close, 
Server=nginx/1.6.1
}
Body:
{
    "count" : 10,
    "total" : 20,
    "statistics" : {
        "running" : 0,
        "success" : 20,
        "fail" : 0 
    }
    "tasks" : [
        {
            "id" : "abcdefghijk",
            "taskStatus" : "success",
            "taskMessage" : "",
            "taskCreateTime" : 1448925013,
            "taskLastDataReceiveTime" : 1448915013,
            "taskFinishTime" : 1448926013
        }
    ]
}
```

