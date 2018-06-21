# ListConsumerGroup {#reference_ns3_mgh_f2b .reference}

查询指定 Logstore 的所有消费组。

示例：

```
GET /logstores/{logstoreName}/consumergroups
```

## 请求语法 {#section_o54_dhh_f2b .section}

```
GET /logstores/<logstoreName>/consumergroups HTTP/1.1
Authorization: <AuthorizationString>
x-log-bodyrawsize: 0
User-Agent: <UserAgent>
x-log-apiversion: 0.6.0
Host: <Project Endpoint>
x-log-signaturemethod: hmac-sha1
Date: <GMT Date>
Content-Type: application/x-protobuf
Connection: Keep-Alive
```

## 请求参数 {#section_lvh_2hh_f2b .section}

|属性名称|类型|是否必须|描述|
|:---|:-|:---|:-|
|logstoreName|string|是|Logstore 的名称。|

**请求头**

ListConsumerGroup接口无特有请求头，关于 Log Service API 的公共请求头请参考[公共请求头](cn.zh-CN/API 参考/公共请求头.md)。

**响应头**

ListConsumerGroup接口无特有响应头，关于 Log Service API 的公共响应头请参考[公共响应头](cn.zh-CN/API 参考/公共响应头.md)。

**响应元素**

ListConsumerGroup 请求成功，其响应 Body 会包括指定 Project 和Logstore下的所有消费组列表，具体格式如下：

|属性名称|类型|描述|
|:---|:-|:-|
|consumerGroup|string|消费组名称。|
|timeout|integer|超时时间，在超时时间段内没有收到心跳，消费组将被删除。|
|order|bool|是否按顺序消费。|

**错误码**

除了返回 Log Service API 的[通用错误码](cn.zh-CN/API 参考/通用错误码.md)，还可能返回如下特有错误码：

|HTTP状态码|ErrorCode|ErrorMessage|
|:------|:--------|:-----------|
|404|ProjectNotExist|The Project does not exist : \{Project\}|
|404|LogStoreNotExist|logstore \{logstoreName\} dose not exist|
|500|InternalServerError|Specified Server Error Message|

## 示例 {#section_p5z_ghh_f2b .section}

**请求示例：**

```
GET /logstores/my-logstore/consumergroups HTTP/1.1
Authorization: LOG LTRTfdR7fbosJYad:OK7Sldsxcv/8gz6YtrrmzR19Tgh=
x-log-bodyrawsize: 0
User-Agent: sls-java-sdk-v-0.6.1
x-log-apiversion: 0.6.0
Host: my-project.cn-shanghai.log.aliyuncs.com
x-log-signaturemethod: hmac-sha1
Date: Fri, 04 May 2018 08:47:30 GMT
Content-Type: application/x-protobuf
Connection: Keep-Alive
```

**响应示例：**

```
HTTP/1.1 200
Server: nginx/1.12.1
Content-Type: application/json
Connection: close
Access-Control-Allow-Origin: *
Date: Fri, 04 May 2018 08:47:31 GMT
x-log-requestid: 5AEC1E23048191954B42EAB9
[
  {
    "name": "test-consumer-group",
    "timeout": 30,
    "order": true
  }
]
```

