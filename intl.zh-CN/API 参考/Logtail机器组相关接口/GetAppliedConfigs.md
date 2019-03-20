# GetAppliedConfigs {#reference_i1s_f5q_12b .reference}

获得 MachineGroup 上已经被应用的配置名称。

示例：

```
GET /machinegroups/{groupName}/configs
```

## 请求语法 {#section_j5v_14t_12b .section}

```
GET /machinegroups/{groupName}/configs HTTP/1.1
Authorization: <AuthorizationString> 
Date: <GMT Date>
Host: <Project Endpoint>
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

## 请求参数 {#section_mx5_b4t_12b .section}

URL 参数：

|参数名称|类型|是否必须|描述|
|:---|:-|:---|:-|
|groupName|string|是|机器分组名称|

**请求头**

无特有请求头。关于 Log Service API 的公共请求头，请参考 [公共请求头](intl.zh-CN/API 参考/公共请求头.md)。

**响应头**

无特有响应头。请参考 [公共响应头](intl.zh-CN/API 参考/公共响应头.md)。

**响应元素**

请求成功，其响应 Body 会包括指定 machinegroup 下的所有 machine 列表，具体格式如下：

|名称|类型|描述|
|:-|:-|:-|
|count|整型|返回的 config 数目。|
|configs|字符串数组|返回的 config 名称列表。|

```
{
    "count":2,
    "configs":
    ["config1","config2"]
}
```

**错误码**

除了返回 Log Service API 的 [通用错误码](intl.zh-CN/API 参考/通用错误码.md)，还可能返回如下特有错误码：

|HTTP 状态码|ErrorCode|ErrorMessage|
|:-------|:--------|:-----------|
|404|GroupNotExist|group \{GroupName\} does not exist|
|500|InternalServerError|internal server error|

## 示例 {#section_e5p_d4t_12b .section}

**请求示例：**

```
GET /machinegroups/test-machine-group/configs HTTP/1.1
Header :
{
    "x-log-apiversion": "0.6.0",
    "Authorization": "LOG <yourAccessKeyId>:<yourSignature>",
    "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com",
    "Date": "Tue, 10 Nov 2015 19:45:48 GMT",
    "Content-Length": "0",
    "x-log-signaturemethod": "hmac-sha1",
    "User-Agent": "sls-java-sdk-v-0.6.0",
    "Content-Type": "application/x-protobuf",
    "x-log-bodyrawsize": "0"
}
```

**响应示例：**

```
HTTP/1.1 200 OK
Header :
{
    "Date": "Tue, 10 Nov 2015 19:45:48 GMT",
    "Content-Length": "53",
    "x-log-requestid": "5642496C99248C8C7B00173F",
    "Connection": "close",
    "Content-Type": "application/json",
    "Server": "nginx/1.6.1"
}
Body :
{
    "configs":     [
        "two",
        "three",
        "test_logstore"
    ],
    "count": 3
}
```

