# GetCursorTime {#concept_fky_k44_hhb .concept}

[GetCursor](reseller.en-US/API Reference/Logstore related APIs/GetCursor.md#)You can call the GetCursor operation to query a cursor based on the server time. In contrast, you can call the GetCursorTime operation to query the server time based on a cursor.

## Request syntax {#section_ipt_vp4_hhb .section}

```
GET /logstores/{logstoreName}/shards/{shard}? cursor={cursor}&type=cursor_time HTTP/1.1 

Authorization: <AuthorizationString>
Date: <GMT Date> 
Host: <Project Endpoint> 
x-log-apiversion: 0.6.0 
x-log-signaturemethod: hmac-sha1
```

## Request parameters {#section_smp_cq4_hhb .section}

|Parameter|Type|Required|Description|
|---------|----|--------|-----------|
|logstoreName|String|Yes|The name of the Logstore.|
|shard|Integer|Yes|The ID of the shard.|
|cursor|String|Yes|The cursor used to query a timestamp.|
|type|String|Yes|The type of data to be queried. Set this parameter to cursor\_time.|

## Request header fields {#section_wm3_mq4_hhb .section}

This operation does not require special request header fields. For more information about the common request header fields of Log Service operations, see [Common request header fields](reseller.en-US/API Reference/Public request header.md).

## Response header fields {#section_g3m_nq4_hhb .section}

This operation does not return special response header fields. For more information about the common response header fields of Log Service operations, see [Common response header fields](reseller.en-US/API Reference/Public response header.md).

## Response parameters {#section_rp4_4q4_hhb .section}

```
{ 
   "cursor_time": 1554260243 
}
```

## Error codes {#section_mxc_rq4_hhb .section}

In addition to the [common errors](reseller.en-US/API Reference/Common error codes.md) of Log Service operations, this operation also returns some specific errors, as listed in the following table.

|HTTP status code|Error code|Error message|
|----------------|----------|-------------|
|404|LogStoreNotExist|Logstore \{Name\} does not exist|
|400|ParameterInvalid|Parameter From is not valid|
|400|ShardNotExist|Shard \{ShardID\} does not exist|
|500|InternalServerError|Specified Server Error Message|
|400|LogStoreWithoutShard|the logstore has no shard|

## Examples {#section_vtk_br4_hhb .section}

**Sample request**

```
GET /logstores/internal-operation_log/shards/0? cursor=MTU0NzQ3MDY4MjM3NjUxMzQ0Ng%3D%3D&type=cursor_time HTTP/1.1 

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

**Sample success response**

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

