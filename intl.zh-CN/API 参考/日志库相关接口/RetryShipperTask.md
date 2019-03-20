# RetryShipperTask {#reference_its_ztq_12b .reference}

重新执行失败的日志投递任务。

## 请求语法 {#section_j5v_14t_12b .section}

```
PUT /logstores/{logstoreName}/shipper/{shipperName}/tasks HTTP/1.1
Authorization: <AuthorizationString> 
Date: <GMT Date>
Host: <Project Endpoint>
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
["task-id-1", "task-id-2", "task-id-2"]
```

## 请求参数 {#section_mx5_b4t_12b .section}

|参数名称|类型|是否必须|描述|
|:---|:-|:---|:-|
|logstoreName|string|是|日志库名称，同一 Project 下唯一。|
|shipperName|string|是|日志投递规则名称，同一 Logstore 下唯一。|

**请求头**

RetryShipperTask 接口无特有请求头。关于 Log Service API 的公共请求头，请参考 [公共请求头](intl.zh-CN/API 参考/公共请求头.md)。

**响应头**

RetryShipperTask 接口无特有响应头。关于 Log Service API 的公共响应头，请参考 [公共响应头](intl.zh-CN/API 参考/公共响应头.md)。

**响应元素**

HTTP 状态码返回 200。

**错误码**

除了返回 Log Service API 的 [通用错误码](intl.zh-CN/API 参考/通用错误码.md)，还可能返回如下特有错误码：

|HTTP 状态码|ErrorCode|ErrorMessage|
|:-------|:--------|:-----------|
|404|ProjectNotExist|Project \{ProjectName\} does not exist|
|404|LogStoreNotExist|logstore \{logstoreName\} does not exist|
|400|ShipperNotExist|shipper \{logstoreName\} does not exist|
|500|InternalServerError|Specified Server Error Message|
|400|ParameterInvalid|Each time allows 10 task retries only|

**细节描述**

每次最多只能重新执行 10 个失败的投递任务。

## 示例 {#section_e5p_d4t_12b .section}

**请求示例：**

```
PUT /logstores/test-logstore/shipper/test-shipper/tasks HTTP/1.1
Header:
{
x-log-apiversion=0.6.0, 
Authorization=LOG <yourAccessKeyId>:<yourSignature>, 
Host=ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com, 
Date=Wed, 11 Nov 2015 08:28:19 GMT, 
Content-Length=55, 
x-log-signaturemethod=hmac-sha1, 
Content-MD5=757C60FC41CC7D3F60B88E0D916D051E, 
User-Agent=sls-java-sdk-v-0.6.0, 
Content-Type=application/json
}
Body : 
["task-id-1", "task-id-2", "task-id-2"]
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
```

