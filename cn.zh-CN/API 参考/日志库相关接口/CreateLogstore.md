# CreateLogstore {#reference_xl4_ttq_12b .reference}

在 Project 下创建 Logstore。

示例：

```
POST /logstores
```

## 请求语法 {#section_it1_kl2_sy .section}

```
POST /logstores HTTP/1.1
Authorization: <AuthorizationString> 
Date: <GMT Date>
Host: <Project Endpoint>
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
{
    "logstoreName" : <logStoreName>,
    "ttl": <ttl>,
    "shardCount": <shardCount>
}
```

## 请求参数 {#section_d4g_ll2_sy .section}

|属性名称|类型|是否必须|描述|
|:---|:-|:---|:-|
|logstoreName|string|是|Logstore 的名称，在 Project 下必须唯一。|
|ttl|integer|是|数据的保存时间，单位为天，范围1~3600。|
|shardCount|integer|是|Shard 个数，单位为个，范围为 1~100。|
|enable\_tracking|bool|否|是否开启WebTracking。|
|autoSplit|bool|否|是否自动分裂 shard。|
|maxSplitShard|int|否|自动分裂时最大的shard个数 ，范围为1~64。当autoSplit为true时必须指定。|

## 请求头 {#section_nqn_nl2_sy .section}

CreateLogstore 接口无特有请求头，关于 Log Service API 的公共请求头请参考 [公共请求头](intl.zh-CN/API 参考/公共请求头.md)。

## 响应头 {#section_of3_4l2_sy .section}

CreateLogstore 接口无特有响应头，关于 Log Service API 的公共响应头请参考 [公共响应头](intl.zh-CN/API 参考/公共响应头.md)。

## 响应元素 {#section_hy4_pl2_sy .section}

HTTP 状态码返回 200。

## 错误码 {#section_evd_ql2_sy .section}

除了返回 Log Service API 的 [通用错误码](intl.zh-CN/API 参考/通用错误码.md)，还可能返回特有错误码。

|HTTP状态码|ErrorCode|ErrorMessage|
|:------|:--------|:-----------|
|400|LogstoreAlreadyExist|logstore \{logstoreName\} already exists|
|500|InternalServerError|Specified Server Error Message|
|400|LogstoreInfoInvalid|logstore info is invalid|
|400|ProjectQuotaExceed|Project Quota Exceed|

## 细节描述 {#section_rhb_tl2_sy .section}

创建过程中 quota 如果非法，则会创建不成功。

## 示例 {#section_ml2_5l2_sy .section}

**请求示例：**

```
POST /logstores HTTP/1.1
Header :
{
x-log-apiversion=0.6.0, 
Authorization=LOG 94to3z418yupi6ikawqqd370:8IwDTWugRK1AZAo0dWQYpffhy48=, 
Host=ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com, 
Date=Wed, 11 Nov 2015 07:35:00 GMT, 
Content-Length=55,
x-log-signaturemethod=hmac-sha1, 
Content-MD5=7EF43D0B8F4A807B95E775048C911C72, 
User-Agent=sls-java-sdk-v-0.6.0, 
Content-Type=application/json
}
Body : 
{
    "logstoreName": "test-logstore",
    "ttl": 1,
    "shardCount": 2
}
```

**响应示例：**

```
HTTP/1.1 200 OK
Header:
{
Date=Wed, 11 Nov 2015 07:35:00 GMT, 
Content-Length=0, 
x-log-requestid=5642EFA499248C827B012B39, 
Connection=close, 
Server=nginx/1.6.1
}
```

