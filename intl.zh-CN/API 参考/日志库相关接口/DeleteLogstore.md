# DeleteLogstore {#reference_xzb_5tq_12b .reference}

删除 Logstore，包括所有 Shard 数据，以及索引等。

## 请求语法 {#section_vzt_dm2_sy .section}

``` {#codeblock_yox_zdr_kip}
DELETE /logstores/{logstoreName} HTTP/1.1
Authorization: <AuthorizationString> 
Date: <GMT Date>
Host: <Project Endpoint>
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

## 请求参数 {#section_j3x_2m2_sy .section}

|参数名称|类型|是否必须|描述|
|:---|:-|:---|:-|
|logstoreName|string|是|日志库名称，同一 Project 下唯一。|

## 请求头 {#section_nqn_nl2_sy .section}

DeleteLogstore接口无特有请求头，关于 Log Service API 的公共请求头，请参考 [公共请求头](intl.zh-CN/API 参考/公共请求头.md)。

## 响应头 {#section_of3_4l2_sy .section}

DeleteLogstore接口无特有响应头，关于 Log Service API 的公共响应头，请参考 [公共响应头](intl.zh-CN/API 参考/公共响应头.md)。

## 响应元素 {#section_hy4_pl2_sy .section}

HTTP 状态码返回 200。

## 错误码 {#section_izy_km2_sy .section}

除了返回 Log Service API 的 [通用错误码](intl.zh-CN/API 参考/通用错误码.md)，还可能返回如下特有错误码。

|HTTP 状态码|ErrorCode|ErrorMessage|
|:-------|:--------|:-----------|
|404|LogStoreNotExist|logstore \{logstoreName\} does not exist|
|500|InternalServerError|Specified Server Error Message|

## 示例 {#section_ayr_nm2_sy .section}

**请求示例：**

``` {#codeblock_9mi_127_c0w}
DELETE /logstores/test_logstore HTTP/1.1
Header :
{
x-log-apiversion=0.6.0, 
Authorization=LOG <yourAccessKeyId>:<yourSignature>, 
Host=ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com, 
Date=Wed, 11 Nov 2015 08:09:38 GMT, 
Content-Length=0, 
x-log-signaturemethod=hmac-sha1, 
User-Agent=sls-java-sdk-v-0.6.0, 
Content-Type=application/json
}
```

**响应示例：**

``` {#codeblock_q3g_tro_k8h}
HTTP/1.1 200 OK
Header:
{
Date=Wed, 11 Nov 2015 08:09:39 GMT, 
Content-Length=0, 
x-log-requestid=5642F7C399248C817B013A07, 
Connection=close, 
Server=nginx/1.6.1
}
```

