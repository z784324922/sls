# UpdateCheckPoint {#reference_wzw_ngh_f2b .reference}

The UpdateCheckpoint interface updates the checkpoints of a shard of consumer groups under a specified project and Logstore.

Example:

```
POST /logstores/{logstoreName}/consumergroups/{consumerGroupName}? forceSuccess={forceSuccess}&type=checkpoint&consumer={consumer}
```

## Request syntax {#section_o54_dhh_f2b .section}

```
POST /logstores/<logstoreName>/consumergroups/<consumerGroupName>? forceSuccess=<forceSuccess>&type=checkpoint&consumer=<consumer> HTTP/1.1
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

## Request parameters {#section_lvh_2hh_f2b .section}

|Name |Parameters|Required or not|Description|
|:----|:---------|:--------------|:----------|
|logstoreName|string|Yes|Name of the Logstore of the consumer group.|
|consumerGroup|string|Yes|Consumer group name.|
|forceSuccess|bool|No|Force update or not.|
|consumer|string|No|Consumer.|
|shard|integer|Yes|The shard ID.|
|checkpoint|string|Yes|Checkpoint value.|

**Request header**

The UpdateCheckpoint interface does not have a specific request header. For details about public request headers of Log Service APIs, see [Public request header](intl.en-US/API Reference/Public request header.md).

**Response header**

The UpdateCheckpoint interface does not have a specific response header. For details about public response headers of Log Service APIs, see [Public response header](intl.en-US/API Reference/Public response header.md).

**Response element**

The system returns the HTTP status code 200.

**Error code**

The interface may return the following error codes in addition to Log Service API [Common error codes](intl.en-US/API Reference/Common error codes.md):

|HTTP status code|ErrorCode|ErrorMessage|
|:---------------|:--------|:-----------|
|400|InvalidShardCheckPoint|shard checkpoint not encoded by base64|
|404|ProjectNotExist|The Project does not exist : \{Project\}|
|404|LogStoreNotExist|logstore \{logstoreName\} dose not exist|
|404|ConsumerGroupNotExist|consumer group not exist|
|404|ShardNotExist|shard not exist|
|500|InternalServerError|Specified Server Error Message|

**Detailed description**

If the consumer is not specified, checkpoints can be updated only when forceSuccess is set to true.

## Example {#section_p5z_ghh_f2b .section}

**Request example:**

```
POST /logstores/logstore-4/consumergroups/consumer_group_test? forceSuccess=false&type=checkpoint&consumer=consumer_1 HTTP/1.1
Authorization: LOG LTRTfdR7fbosJYad:OK7Sldsxcv/8gz6YtrrmzR19Tgh=
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

**Response example:**

```
HTTP/1.1 200
Server: nginx/1.12.1
Content-Length: 0
Connection: close
Access-Control-Allow-Origin: *
Date: Sun, 06 May 2018 09:14:54 GMT
x-log-requestid: 5AEEC78E1FFC0366B24F1C63
```

