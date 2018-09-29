# GetShipperStatus {#reference_eg2_ztq_12b .reference}

查询日志投递任务状态。

## 请求语法 {#section_j5v_14t_12b .section}

```
GET /logstores/{logstoreName}/shipper/{shipperName}/tasks?from=1448748198&to=1448948198&status=success&offset=0&size=100 HTTP/1.1
Authorization: <AuthorizationString> 
Date: <GMT Date>
Host: <Project Endpoint>
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

## 请求参数 {#section_mx5_b4t_12b .section}

|参数名称|类型|是否必须|描述|
|:---|:-|:---|:-|
|logstoreName|string|是|日志库名称，同一 Project 下唯一|
|shipperName|string|是|日志投递规则名称，同一 Logstore 下唯一|
|from|integer|是|日志投递任务创建时间区间|
|to|integer|是|日志投递任务创建时间区间|
|status|string|否|默认为空，表示返回所有状态的任务，目前支持 success/fail/running 等状态|
|offset|integer|否|返回指定时间区间内投递任务的起始数目，默认值为 0|
|size|integer|否|返回指定时间区间内投递任务的数目，默认值为 100，最大为 500|

**请求头**

GetShipperStatus 接口无特有请求头。关于 Log Service API 的公共请求头，请参考 [公共请求头](intl.zh-CN/API 参考/公共请求头.md)。

**响应头**

GetShipperStatus 接口无特有响应头。关于 Log Service API 的公共响应头，请参考 [公共响应头](intl.zh-CN/API 参考/公共响应头.md)。

**响应元素**

请求成功，其响应 Body 会包括指定的日志投递任务列表：

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

|名称|类型|描述|
|:-|:-|:-|
|count|integer|返回的任务个数。|
|total|integer|指定范围内任务总数。|
|statistics|json|指定范围内任务汇总状态，具体请参考下表。|
|tasks|array|指定范围内投递任务具体详情，具体请参考下表。|

**statistics 内容：**

|名称|类型|描述|
|:-|:-|:-|
|running|integer|指定范围内状态为 running 的任务个数。|
|success|integer|指定范围内状态为 success 的任务个数。|
|fail|integer|指定范围内状态为 fail 的任务个数。|

**tasks 内容：**

|名称|类型|描述|
|:-|:-|:-|
|id|string|具体投递任务的任务唯一 ID。|
|taskStatus|string|投递任务的具体状态，可能为 running/success/fail 中的一种。|
|taskMessage|string|投递任务失败时的具体错误信息。|
|taskCreateTime|integer|投递任务创建时间。|
|taskLastDataReceiveTime|integer|投递任务中的最近一条日志到达服务端时间（非日志时间，是服务端接收时间）。|
|taskFinishTime|integer|投递任务结束时间。|

**错误码**

除了返回 Log Service API 的 [通用错误码](intl.zh-CN/API 参考/通用错误码.md)，还可能返回如下特有错误码：

|HTTP 状态码|ErrorCode|ErrorMessage|
|:-------|:--------|:-----------|
|404|ProjectNotExist|Project \{ProjectName\} does not exist|
|404|LogStoreNotExist|logstore \{logstoreName\} does not exist|
|400|ShipperNotExist|shipper \{logstoreName\} does not exist|
|500|InternalServerError|internal server error|
|400|ParameterInvalid|start time must be earlier than end time|
|400|ParameterInvalid|only support query last 48 hours task status|
|400|ParameterInvalid|status only contains success/running/fail|

**细节描述**

投递任务状态查询时间区间只能指定为最近 24 小时之内。

## 示例 {#section_e5p_d4t_12b .section}

**请求示例：**

```
GET /logstores/test-logstore/shipper/test-shipper/tasks?from=1448748198&to=1448948198&status=success&offset=0&size=100 HTTP/1.1
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

**响应示例：**

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

