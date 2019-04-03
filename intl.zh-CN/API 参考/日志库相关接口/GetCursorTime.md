# GetCursorTime {#concept_fky_k44_hhb .concept}

[GetCursor](intl.zh-CN/API 参考/日志库相关接口/GetCursor.md#)API可以帮助我们根据服务端时间来查询Cursor的接口，而GetCursorTime API则提供了根据Cursor获取服务端时间的功能。

## 请求语法 {#section_ipt_vp4_hhb .section}

```
GET /logstores/{logstoreName}/shards/{shard}?cursor={cursor}&type=cursor_time HTTP/1.1 

Authorization: <AuthorizationString>
Date: <GMT Date> 
Host: <Project Endpoint> 
x-log-apiversion: 0.6.0 
x-log-signaturemethod: hmac-sha1
```

## 请求参数 {#section_smp_cq4_hhb .section}

|参数名称|参数类型|是否必须|参数说明|
|----|----|----|----|
|logstoreName|string|是|Logstore名称。|
|shard|int|是|shard ID。|
|cursor|string|是|希望获取时间戳的Cursor。|
|type|string|是|固定为cursor\_time。|

## 请求头 {#section_wm3_mq4_hhb .section}

无特有请求头。关于 Log Service API 的公共请求头，请参考 [公共请求头](intl.zh-CN/API 参考/公共请求头.md)。

## 响应头 {#section_g3m_nq4_hhb .section}

无特有响应头。关于 Log Service API 的公共响应头，请参考 [公共响应头](intl.zh-CN/API 参考/公共响应头.md)。

## 响应元素 {#section_rp4_4q4_hhb .section}

```
{ 
   "cursor_time": 1554260243 
}
```

## 错误码 {#section_mxc_rq4_hhb .section}

除了返回 Log Service API 的 [通用错误码](intl.zh-CN/API 参考/通用错误码.md)，还可能返回如下特有错误码：

|HTTP 状态码|ErrorCode|ErrorMessage|
|--------|---------|------------|
|404|LogStoreNotExist|Logstore \{Name\} does not exist|
|400|ParameterInvalid|Parameter From is not valid|
|400|ShardNotExist|Shard \{ShardID\} does not exist|
|500|InternalServerError|Specified Server Error Message|
|400|LogStoreWithoutShard|the logstore has no shard|

## 示例 {#section_vtk_br4_hhb .section}

**请求示例：**

```
GET /logstores/internal-operation_log/shards/0?cursor=MTU0NzQ3MDY4MjM3NjUxMzQ0Ng%3D%3D&type=cursor_time HTTP/1.1 

Authorization: LOG <yourAccessKeyId>:<yourSignature> 
x-log-bodyrawsize: 0 
User-Agent: sls-java-sdk-v-0.6.1 
x-log-apiversion: 0.6.0 
Host: my-project.cn-hangzhou.sls.aliyuncs.com 
x-log-signaturemethod: hmac-sha1 
Date: Wed, 03 Apr 2019 02:57:23 GMT 
Content-Type: application/x-protobuf 
Connection: Keep-Alive
```

**响应示例：**

```
HTTP/1.1 200 
Server: nginx 
Content-Type: application/json 
Content-Length: 26 
Connection: close 
Access-Control-Allow-Origin: * 
Date: Wed, 03 Apr 2019 02:57:23 GMT 
x-log-requestid: 5CA42113D2F00378A74B9051 
{ 
  "cursor_time": 1554260243 
}
```

