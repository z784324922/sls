# DeleteConfig {#reference_nt5_j5q_12b .reference}

删除特定 config，如果 config 已被[应用到机器组](intl.zh-CN/API 参考/Logtail机器组相关接口/ApplyConfigToMachineGroup.md)，则 Logtail 配置也会被删除。

示例：

```
DELETE /configs/{configName}
```

## 请求语法 {#section_j5v_14t_12b .section}

```
DELETE /configs/<configName> HTTP/1.1
Authorization: <AuthorizationString> 
Date: <GMT Date>
Host: <Project Endpoint>
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

## 请求参数 {#section_mx5_b4t_12b .section}

URL参数：

|参数名称|类型|是否必须|描述|
|----|--|----|--|
|ConfigName|String|是|日志配置名称|

**请求头**

无特有请求头。关于 Log Service API 的公共请求头，请参考 [公共请求头](intl.zh-CN/API 参考/公共请求头.md)。

**响应头**

无特有响应头。请参考 [公共响应头](intl.zh-CN/API 参考/公共响应头.md)。

**响应元素**

HTTP 状态码返回 200。

**错误码**

除了返回 Log Service API 的 [通用错误码](intl.zh-CN/API 参考/通用错误码.md)，还可能返回如下特有错误码：

|HTTP 状态码|ErrorCode|ErrorMessage|
|:-------|:--------|:-----------|
|404|ConfigNotExist|config \{Configname\} does not exist|
|400|InvalidParameter|invalid config resource json|
|500|InternalServerError|internal server error|

## 示例 {#section_e5p_d4t_12b .section}

**请求示例：**

```
DELETE /configs/logtail-config-sample
Header :
{
    "Content-Length": 0, 
    "x-log-signaturemethod": "hmac-sha1", 
    "x-log-bodyrawsize": 0, 
    "User-Agent": "log-python-sdk-v-0.6.0", 
    "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com", 
    "Date": "Mon, 09 Nov 2015 09:28:21 GMT", 
    "x-log-apiversion": "0.6.0", 
    "Authorization": "LOG 94to3z418yupi6ikawqqd370:utd/O1JNCYvcRGiSXHsjKhTzJDI="
}
```

**响应示例：**

```
Header : 
{
    "date": "Mon, 09 Nov 2015 09:28:21 GMT", 
    "connection": "close", 
    "x-log-requestid": "5640673599248CAA230836C6", 
    "content-length": "0", 
    "server": "nginx/1.6.1"
}
```

