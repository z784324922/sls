# UpdateLogstore {#reference_zgr_5tq_12b .reference}

更新 Logstore 的属性。目前只支持更新 TTL和shard 属性。

## 请求语法 {#section_j5v_14t_12b .section}

```
PUT /logstores/{logstoreName} HTTP/1.1
Authorization: <AuthorizationString> 
Date: <GMT Date>
Host: <Project Endpoint>
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
{
    "logstoreName": <logstoreName>,
    "ttl": <ttl>,
    "shardCount": <shardCount>
}
```

## 请求参数 {#section_mx5_b4t_12b .section}

|参数名称|类型|是否必须|描述|
|:---|:-|:---|:-|
|logstoreName|string|是|日志库名称，同一 Project 下唯一。|
|ttl|integer|是|日志数据生命周期（TTL），单位为天，范围1~365（额外需求请提交工单）。|
|shardCount|integer|是|Shard 个数，单位为个，范围为 1~10。|

**请求头**

UpdateLogstore 接口无特有请求头。关于 Log Service API 的公共请求头，请参考 [公共请求头](intl.zh-CN/API 参考/公共请求头.md)。

**响应头**

UpdateLogstore 接口无特有响应头。关于 Log Service API 的公共响应头，请参考 [公共响应头](intl.zh-CN/API 参考/公共响应头.md)。

**响应元素**

HTTP 状态码返回 200。

**错误码**

除了返回 Log Service API 的 [通用错误码](intl.zh-CN/API 参考/通用错误码.md)，还可能返回如下特有错误码：

|HTTP 状态码|ErrorCode|ErrorMessage|
|:-------|:--------|:-----------|
|404|ProjectNotExist|Project \{ProjectName\} does not exist|
|404|LogStoreNotExist|logstore \{logstoreName\} does not exist|
|400|LogStoreAlreadyExist|logstore \{logstoreName\} already exists|
|500|InternalServerError|Specified Server Error Message|
|400|ParameterInvalid|invalid shard count, you can only increase the count|

**细节描述**

Shard 的数量目前只能增加不能减少。

## 示例 {#section_e5p_d4t_12b .section}

**请求示例：**

```
PUT /logstores/test-logstore HTTP/1.1
Header:
{
x-log-apiversion=0.6.0, 
Authorization=LOG 94to3z418yupi6ikawqqd370:wFcl3ohVJupCi0ZFxRD0x4IA68A=, 
Host=ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com, 
Date=Wed, 11 Nov 2015 08:28:19 GMT, 
Content-Length=55, 
x-log-signaturemethod=hmac-sha1, 
Content-MD5=757C60FC41CC7D3F60B88E0D916D051E, 
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
Date=Wed, 11 Nov 2015 08:28:20 GMT, 
Content-Length=0, 
x-log-requestid=5642FC2399248C8F7B0145FD, 
Connection=close, 
Server=nginx/1.6.1
}
```

