# CreateConsumerGroup {#reference_hq4_cgh_f2b .reference}

在指定的 Logstore 上创建一个消费组。

示例：

``` {#codeblock_kbu_uk0_q4h}
POST /logstores/{logstoreName}/consumergroups
```

## 请求语法 {#section_o54_dhh_f2b .section}

``` {#codeblock_aqh_v5t_wxp}
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

## 请求参数 {#section_lvh_2hh_f2b .section}

|属性名称|类型|是否必须|描述|
|:---|:-|:---|:-|
|logstoreName|string|是|Logstore 的名称。|
|consumerGroup|string|是|消费组名称，在Project 下必须唯一。|
|timeout|integer|是|超时时间，在超时时间段内没有收到心跳，消费者将被删除。|
|order|bool|是|在单个shard中是否按顺序消费。 -   true：表示在单个shard中按顺序消费。shard分裂后，先消费原shard数据，然后并列消费两个新shard的数据。
-   false：表示不按顺序消费。

 |

 **请求头** 

CreateConsumerGroup 接口无特有请求头，关于 Log Service API 的公共请求头，请参考[公共请求头](intl.zh-CN/API 参考/公共请求头.md)。

 **响应头** 

CreateConsumerGroup 接口无特有响应头，关于 Log Service API 的公共响应头，请参考[公共响应头](intl.zh-CN/API 参考/公共响应头.md)。

 **响应元素** 

HTTP 状态码返回 200。

 **错误码** 

除了返回 Log Service API 的[通用错误码](intl.zh-CN/API 参考/通用错误码.md)，还可能返回如下特有错误码：

|**HTTP状态码**|**ErrorCode**|**ErrorMessage**|
|:----------|:------------|:---------------|
|400|ConsumerGroupAlreadyExist|consumer group already exist|
|400|JsonInfoInvalid|consumerGroup or timeout is of error format|
|404|ProjectNotExist|The Project does not exist : \{Project\}|
|404|LogStoreNotExist|logstore \{logstoreName\} dose not exist|
|500|InternalServerError|Specified Server Error Message|

## 示例 {#section_p5z_ghh_f2b .section}

**请求示例：** 

``` {#codeblock_wv4_48f_tcz}
POST /logstores/my-logstore/consumergroups HTTP/1.1
Header:
Authorization: LOG <yourAccessKeyId>:<yourSignature>
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

**响应示例：** 

``` {#codeblock_cld_waw_t8z}
HTTP/1.1 200
Server: nginx/1.12.1
Content-Length: 0
Connection: close
Access-Control-Allow-Origin: *
Date: Fri, 04 May 2018 08:15:11 GMT
x-log-requestid: 5AEC168FA796F4195BF404CB
```

