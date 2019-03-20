# GetMachineGroup {#reference_swr_d5q_12b .reference}

查看具体的 MachineGroup 信息。

示例：

```
GET /machinegroups/{groupName}
```

## 请求语法 {#section_j5v_14t_12b .section}

```
GET /machinegroups/{groupName} HTTP/1.1
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

|属性名称|类型|描述|
|----|--|--|
|groupName|string|机器分组名称，Project 下唯一|
|groupType|string|分组类型（空或者 Armory\)，默认为空|
|machineIdentifyType|string|机器标识类型，分为 IP 和 userdefined 两种|
|groupAttribute|json object|机器分组的属性，默认为空|
|machineList|json array|具体的机器标识，可以是 IP 或 userdefined-id|
|createTime|int|机器分组创建时间|
|lastModifyTime|int|机器分组最近更新时间|

groupAttribute 说明如下：

|属性名称|类型|是否必须|描述|
|:---|:-|:---|:-|
|groupTopic|string|否|机器分组的 topic，一般不设置|
|externalName|string|否|机器分组所依赖的外部管理系统（Armory）标识|

```
{
    "groupName": "test-machine-group",
    "groupType": "",
    "groupAttribute":     {
        "externalName": "testgroup",
        "groupTopic": "testtopic"
    },
    "machineIdentifyType": "ip",
    "machineList":     [
        "127.0.0.1",
        "127.0.0.2"
    ],
    "createTime": 1447178253,
    "lastModifyTime": 1447178253
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
GET /machinegroups/test-machine-group HTTP/1.1
Header :
{
    "x-log-apiversion": "0.6.0",
    "Authorization": "LOG <yourAccessKeyId>:<yourSignature>",
    "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com",
    "Date": "Tue, 10 Nov 2015 18:15:24 GMT",
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
    "Date": "Tue, 10 Nov 2015 18:15:23 GMT",
    "Content-Length": "239",
    "x-log-requestid": "5642343B99248CB36D0060B8",
    "Connection": "close",
    "Content-Type": "application/json",
    "Server": "nginx/1.6.1"
}
Body :
{
    "groupName": "test-machine-group",
    "groupType": "",
    "groupAttribute":     {
        "externalName": "testgroup",
        "groupTopic": "testtopic"
    },
    "machineIdentifyType": "ip",
    "machineList":     [
        "127.0.0.1",
        "127.0.0.2"
    ],
    "createTime": 1447178253,
    "lastModifyTime": 1447178253
}
```

