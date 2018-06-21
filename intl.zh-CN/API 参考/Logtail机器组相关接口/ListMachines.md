# ListMachines {#reference_fnf_f5q_12b .reference}

获得 machinegroup 下属于用户并与 Server 端连接的机器状态信息。

示例：

```
GET /machinegroups/{groupName}/machines?offset=1&size=10
```

## 请求语法 {#section_j5v_14t_12b .section}

```
GET /machinegroups/{groupName}/machines HTTP/1.1
Authorization: <AuthorizationString> 
Date: <GMT Date>
Host: <Project Endpoint>
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

## 请求参数 {#section_mx5_b4t_12b .section}

URL 参数：

|名称|类型|必须|描述|
|:-|:-|:-|:-|
|groupName|string|是|机器分组名称|
|offset|int|否|返回记录的起始位置，默认为 0|
|size|int|否|每页返回最大条目，默认 500（最大值）|

**请求头**

无特有请求头。关于 Log Service API 的公共请求头，请参考 [公共请求头](intl.zh-CN/API 参考/公共请求头.md)。

**响应头**

无特有响应头。请参考 [公共响应头](intl.zh-CN/API 参考/公共响应头.md)。

**响应元素**

|名称|类型|描述|
|:-|:-|:-|
|count|int|返回的 machine 数目|
|total|int|machine 总数|
|machines|json array|返回的 machine 名称列表|

machines 说明如下：

|名称|类型|描述|
|:-|:-|:-|
|ip|string|机器的 IP|
|machine-uniqueid|string|机器 DMI UUID|
|userdefined-id|string|机器的用户自定义标识|

```
{
    "count":10,
    "total":100,
    "machines":
    [{
        "ip" : "testip1",
        "machine-uniqueid" : "testuuid1",
        "userdefined-id" : "testuserdefinedid1",
        "lastHeartbeatTime" : 1447182247
    },
    {
        "ip" : "testip1",
        "machine-uniqueid" : "testuuid2",
        "userdefined-id" : "testuserdefinedid2",
        "lastHeartbeatTime" : 1447182247
    },
    {
        "ip" : "testip2",
        "machine-uniqueid" : "testuuid",
        "userdefined-id" : "testuserdefinedid"
        "lastHeartbeatTime" : 1447182247
    }]
}
```

**错误码**

除了返回 Log Service API 的 [通用错误码](intl.zh-CN/API 参考/通用错误码.md)，还可能返回如下特有错误码：

|HTTP 状态码|ErrorCode|ErrorMessage|
|:-------|:--------|:-----------|
|404|GroupNotExist|group \{GroupName\} does not exist|
|500|InternalServerError|internal server error|

**细节描述**

该接口只获取与 Server 端连接正常的机器列表。

## 示例 {#section_e5p_d4t_12b .section}

**请求示例：**

```
GET /machinegroups/test-machine-group-5/machines?offset=0&size=3 HTTP/1.1
Header :
{
    "x-log-apiversion": "0.6.0",
    "Authorization": "LOG 94to3z418yupi6ikawqqd370:9yoK0iJPxr0RrWf/wW9NJYXu4zo=",
    "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com",
    "Date": "Tue, 10 Nov 2015 19:04:57 GMT",
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
    "Date": "Tue, 10 Nov 2015 19:04:58 GMT",
    "Content-Length": "324",
    "x-log-requestid": "56423FD999248C827B000A57",
    "Connection": "close",
    "Content-Type": "application/json",
    "Server": "nginx/1.6.1"
}
Body :
{
    "machines":     [
        {
            "ip": "10.101.166.116",
            "machine-uniqueid": "",
            "userdefined-id": "",
            "lastHeartbeatTime": 1447182247
        },
        {
            "ip": "10.101.165.193",
            "machine-uniqueid": "",
            "userdefined-id": "",
            "lastHeartbeatTime": 1447182246
        },
        {
            "ip": "10.101.166.91",
            "machine-uniqueid": "",
            "userdefined-id": "",
            "lastHeartbeatTime": 1447182248
        }
    ],
    "count": 3,
    "total": 8
}
```

