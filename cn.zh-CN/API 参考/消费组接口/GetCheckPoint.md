# GetCheckPoint {#reference_xbw_fgh_f2b .reference}

获取指定消费组的消费的某个或者所有 Shard 的checkpoint。

示例：

```
GET /logstores/{logstoreName}/consumergroups/{consumerGroupName}?shard={shardId}
```

## 请求语法 {#section_o54_dhh_f2b .section}

```
GET /logstores/<logstoreName>/consumergroups/<consumerGroup>?shard=<shardId> HTTP/1.1
Authorization: <AuthorizationString> 
x-log-bodyrawsize: 0
User-Agent: <UserAgent>
x-log-apiversion: 0.6.0
Host: <Project Endpoint>
x-log-signaturemethod: hmac-sha1
Date: <GMT Date>
Content-Type: application/json
Connection: Keep-Alive
```

## 请求参数 {#section_lvh_2hh_f2b .section}

|属性名称|类型|是否必须|描述|
|:---|:-|:---|:-|
|logstoreName|string|是|消费组所属 Logstore 的名称。|
|consumerGroup|string|是|消费组名称。|
|shardId|integer|否|Shard Id。如果不指定则返回所有shard的checkpoint。|

**请求头**

GetCheckPoint接口无特有请求头，关于 Log Service API 的公共请求头请参考[公共请求头](intl.zh-CN/API 参考/公共请求头.md)。

**响应头**

GetCheckPoint接口无特有响应头，关于 Log Service API 的公共响应头请参考[公共响应头](intl.zh-CN/API 参考/公共响应头.md)。

**响应元素**

GetCheckPoint请求成功，其响应 Body 会包括指定 ConsumerGroup 消费的 Shard 的checkpoint，具体格式如下。

|属性名称|类型|描述|
|:---|:-|:-|
|shard|integer|Shard ID。|
|checkpoint|string|checkpoint的值。|
|updateTime|integer|checkpoint最后的更新时间。|
|consumer|string|Shard所属的消费者。|

**错误码**

除了返回 Log Service API 的[通用错误码](intl.zh-CN/API 参考/通用错误码.md)，还可能返回如下特有错误码：

|HTTP状态码|ErrorCode|ErrorMessage|
|:------|:--------|:-----------|
|404|ProjectNotExist|The Project does not exist : \{Project\}|
|404|LogStoreNotExist|logstore \{logstoreName\} dose not exist|
|404|ConsumerGroupNotExist|consumer group not exist|
|500|InternalServerError|Specified Server Error Message|

**细节描述**

如果指定的Shard不存在，返回空列表。如果 Shard不指定，返回所有shard的checkpoint。

## 示例 {#section_p5z_ghh_f2b .section}

**请求示例：**

```
GET /logstores/my-logstore/consumergroups/consumer_group_test?shard=0
Authorization: LOG <yourAccessKeyId>:<yourSignature>
x-log-bodyrawsize: 0
User-Agent: sls-java-sdk-v-0.6.1
x-log-apiversion: 0.6.0
Host: my-project.cn-shanghai.log.aliyuncs.com
x-log-signaturemethod: hmac-sha1
Date: Fri, 04 May 2018 09:26:53 GMT
Content-Type: application/x-protobuf
Connection: Keep-Alive
```

**响应示例：**

```
HTTP/1.1 200
Server: nginx/1.12.1
Content-Type: application/json
Content-Length: 111
Connection: close
Access-Control-Allow-Origin: *
Date: Fri, 04 May 2018 09:26:53 GMT
x-log-requestid: 5AEC275D1FFC036AB254B20C
[
  {
    "shard": 0,
    "checkpoint": "MTUyNDE1NTM3OTM3MzkwODQ5Ng==",
    "updateTime": 1524224984800922,
    "consumer": "consumer_1"
  }
]
```

