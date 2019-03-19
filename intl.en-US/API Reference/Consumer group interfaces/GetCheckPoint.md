# GetCheckPoint {#reference_xbw_fgh_f2b .reference}

Obtain the checkpoints of one or all shards consumed by a specified consumer group.

Example:

```
GET /logstores/{logstoreName}/consumergroups/{consumerGroupName}? shard={shardId}
```

## Request syntax {#section_o54_dhh_f2b .section}

```
GET /logstores/<logstoreName>/consumergroups/<consumerGroup>? shard=<shardId> HTTP/1.1
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

## Request parameters {#section_lvh_2hh_f2b .section}

|Attribute name|Type|Required or not|Description|
|:-------------|:---|:--------------|:----------|
|logstoreName|string|Yes|Name of the Logstore of the consumer group.|
|consumerGroup|string|Yes|Consumer group name.|
|shardId|integer|No|The shard ID. If the Shard ID is not specified, checkpoints of all shards are returned.|

**Request header**

The GetCheckpoint interface does not have a specific request header. For details about public request headers of Log Service APIs, see [Public request header](reseller.en-US/API Reference/Public request header.md).

**Response header**

The GetCheckpoint interface does not have a specific response header. For details about public response headers of Log Service APIs, see [Public response header](reseller.en-US/API Reference/Public response header.md).

**Response element**

If the GetCheckPoint request is sent successfully, the response body contains the checkpoint of the shard consumed by the specified consumer group in the following format:

|Attribute name|Type|Description|
|:-------------|:---|:----------|
|shard|integer|The shard ID.|
|checkpoint|string|Checkpoint value.|
|updateTime|integer|Last update time of the checkpoint.|
|consumer|string|Consumer of the shard.|

**Error code**

The interface may return the following error codes in addition to Log Service API [Common error codes](reseller.en-US/API Reference/Common error codes.md):

|HTTP status code|ErrorCode|ErrorMessage|
|:---------------|:--------|:-----------|
|404|ProjectNotExist|The Project does not exist : \{Project\}|
|404|LogStoreNotExist|logstore \{logstoreName\} dose not exist|
|404|ConsumerGroupNotExist|consumer group not exist|
|500|InternalServerError|Specified Server Error Message|

**Detailed description**

If the specified shard does not exist, an empty list is returned. If the shard ID is not specified, checkpoints of all shards are returned.

## Example {#section_p5z_ghh_f2b .section}

**Request example:**

```
GET /logstores/my-logstore/consumergroups/consumer_group_test? shard=0
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

**Response example:**

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

