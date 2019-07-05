# UpdateCheckPoint {#reference_wzw_ngh_f2b .reference}

UpdateCheckpoint接口更新指定Project和Logstore下的Consumer Group的某个Shard的checkpoint。

示例：

``` {#codeblock_i06_417_ds0}
POST /logstores/{logstoreName}/consumergroups/{consumerGroupName}?forceSuccess={forceSuccess}&type=checkpoint&consumer={consumer}
```

## 请求语法 {#section_o54_dhh_f2b .section}

``` {#codeblock_dvy_4rp_m98}
POST /logstores/<logstoreName>/consumergroups/<consumerGroupName>?forceSuccess=<forceSuccess>&type=checkpoint&consumer=<consumer> HTTP/1.1
Authorization: <AuthorizationString>
x-log-bodyrawsize: 0
User-Agent: <UserAgent>
x-log-apiversion: 0.6.0
Host: <Project Endpoint>
x-log-signaturemethod: hmac-sha1
Date: <GMT Date>
Content-Type: application/json
Content-Length: <ContentLength>
Connection: Keep-Alive
{
  "shard": <shardID>,
  "checkpoint": <checkpoint>
}
```

## 请求参数 {#section_lvh_2hh_f2b .section}

|名称|类型|是否必须|描述|
|:-|:-|:---|:-|
|logstoreName|string|是|消费组所属Logstore 的名称。|
|consumerGroup|string|是|消费组名称。|
|forceSuccess|bool|否|是否强制更新。|
|consumer|string|否|消费者。|
|shard|integer|是|Shard ID。|
|checkpoint|string|是|checkpoint值。|

 **请求头** 

UpdateCheckpoint接口无特有请求头，关于 Log Service API 的公共请求头，请参考[公共请求头](intl.zh-CN/API 参考/公共请求头.md)。

 **响应头** 

UpdateCheckpoint接口无特有响应头，关于 Log Service API 的公共响应头，请参考[公共响应头](intl.zh-CN/API 参考/公共响应头.md)。

 **响应元素** 

HTTP 状态码返回 200。

 **错误码** 

除了返回 Log Service API 的[通用错误码](intl.zh-CN/API 参考/通用错误码.md)，还可能返回如下特有错误码：

|HTTP状态码|ErrorCode|ErrorMessage|
|:------|:--------|:-----------|
|400|InvalidShardCheckPoint|shard checkpoint not encoded by base64|
|404|ProjectNotExist|The Project does not exist : \{Project\}|
|404|LogStoreNotExist|logstore \{logstoreName\} dose not exist|
|404|ConsumerGroupNotExist|consumer group not exist|
|404|ShardNotExist|shard not exist|
|500|InternalServerError|Specified Server Error Message|

 **细节描述** 

当不指定消费者时，必须指定 forceSuccess 为 true 才能更新 checkpoint。

## 示例 {#section_p5z_ghh_f2b .section}

**请求示例：** 

``` {#codeblock_tjo_cnj_uhh}
POST /logstores/logstore-4/consumergroups/consumer_group_test?forceSuccess=false&type=checkpoint&consumer=consumer_1 HTTP/1.1
Authorization: LOG <yourAccessKeyId>:<yourSignature>
x-log-bodyrawsize: 0
User-Agent: sls-java-sdk-v-0.6.1
x-log-apiversion: 0.6.0
Host: my-project.cn-shanghai.log.aliyuncs.com
x-log-signaturemethod: hmac-sha1
Date: Sun, 06 May 2018 09:14:53 GMT
Content-Type: application/json
Content-MD5: 59457BAFB3F85A5117BDE243947583B0
Content-Length: 55
Connection: Keep-Alive
{
  "shard": 0,
  "checkpoint": "MTUyNDE1NTM3OTM3MzkwODQ5Ng=="
}
```

**响应示例：** 

``` {#codeblock_hk9_yhg_zhi}
HTTP/1.1 200
Server: nginx/1.12.1
Content-Length: 0
Connection: close
Access-Control-Allow-Origin: *
Date: Sun, 06 May 2018 09:14:54 GMT
x-log-requestid: 5AEEC78E1FFC0366B24F1C63
```

