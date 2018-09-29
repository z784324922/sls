# CreateConsumerGroup {#reference_hq4_cgh_f2b .reference}

Create a consumer group in a specified Logstore.

Example:

```
POST /logstores/{logstoreName}/consumergroups
```

## Request syntax {#section_o54_dhh_f2b .section}

```
POST /logstores/<logstoreName>/consumergroups HTTP/1.1
Authorization: <AuthorizationString>
x-log-bodyrawsize: 0
User-Agent: <UserAgent>
x-log-apiversion: 0.6.0
Host: <Project Endpoint>
x-log-signaturemethod: hmac-sha1
Date: <GMT Date>
Content-Type: application/json
Content-MD5: F58544E4D022CC28A93D0B7CC208A5AA
Content-Length: <ContentLength>
{
  "consumerGroup": <consumerGroup>,
  "timeout": <timeout>,
  "order": <order>
}
```

## Request parameters {#section_lvh_2hh_f2b .section}

|Attribute name|Type|Required or not|Description|
|:-------------|:---|:--------------|:----------|
|logstoreName|string|Yes|Logstore name.|
|consumerGroup|string|Yes|Consumer group name, which must be unique under a project.|
|timeout |integer|Yes|Timeout period. If no heartbeat is received in the timeout period, the consumer group is deleted.|
|order|bool|Yes|Sequential consumption or not.|

**Request header**

The CreateConsumerGroup interface does not have a specific request header. For details about public request headers of Log Service APIs, see [Public request header](reseller.en-US/API Reference/Public request header.md).

**Response header**

The CreateConsumerGroup interface does not have a specific response header. For details about public response headers of Log Service APIs, see [Public response header](reseller.en-US/API Reference/Public response header.md).

**Response element **

The returned HTTP status code is 200.

**Error code**

The interface may return the following error codes in addition to Log Service API [Common error codes](reseller.en-US/API Reference/Common error codes.md):

|**HTTP status code**|**ErrorCode**|**ErrorMessage**|
|:-------------------|:------------|:---------------|
|400|ConsumerGroupAlreadyExist|consumer group already exist|
|400|JsonInfoInvalid|consumerGroup or timeout is of error format|
|404|ProjectNotExist|The Project does not exist : \{Project\}|
|404|LogStoreNotExist|logstore \{logstoreName\} dose not exist|
|500 |InternalServerError|Specified Server Error Message|

## Example {#section_p5z_ghh_f2b .section}

**Request example:**

```
POST /logstores/my-logstore/consumergroups HTTP/1.1
Header:
Authorization: LOG LTRTfdR7fbosJYad:OK7Sldsxcv/8gz6YtrrmzR19Tgh=
x-log-bodyrawsize: 0
User-Agent: sls-java-sdk-v-0.6.1
x-log-apiversion: 0.6.0
Host: my-project.cn-shanghai.log.aliyuncs.com
x-log-signaturemethod: hmac-sha1
Date: Fri, 04 May 2018 08:02:22 GMT
Content-Type: application/json
Content-MD5: F58544E4D022CC28A93D0B7CC208A5AA
Content-Length: 65
Connection: Keep-Alive
Body:
{
  "consumerGroup": "consumer-group-1",
  "timeout": 300,
  "order": true
}
```

**Response example:**

```
HTTP/1.1 200
Server: nginx/1.12.1
Content-Length: 0
Connection: close
Access-Control-Allow-Origin: *
Date: Fri, 04 May 2018 08:15:11 GMT
x-log-requestid: 5AEC168FA796F4195BF404CB
```

