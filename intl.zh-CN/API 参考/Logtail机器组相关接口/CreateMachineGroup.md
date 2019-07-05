# CreateMachineGroup {#reference_xnh_b5q_12b .reference}

您可以根据需求创建一组机器，用以日志收集下发配置。

示例：

``` {#codeblock_d07_a0n_iik}
POST /machinegroups
```

## 请求语法 {#section_j5v_14t_12b .section}

``` {#codeblock_f6r_nk4_5wu}
POST /machinegroups HTTP/1.1
Authorization: <AuthorizationString> 
Content-Type:application/json
Content-Length:<Content Length>
Content-MD5<:<Content MD5>
Date: <GMT Date>
Host: <Project Endpoint>
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
{
    "groupName" : "testgroup",
    "groupType" : "",
    "groupAttribute" : {
        "externalName" : "testgroup",
        "groupTopic": "testgrouptopic"
    },
    "machineIdentifyType" : "ip",
    "machineList" : [
        "test-ip1",
        "test-ip2"
    ]
}
```

## 请求参数 {#section_mx5_b4t_12b .section}

Body 参数：

|属性名称|类型|是否必须|描述|
|:---|:-|:---|:-|
|groupName|string|是|机器分组名称，Project 下唯一|
|groupType|string|否|机器分组类型，默认为空|
|machineIdentifyType|string|是|机器标识类型，分为 IP 和 userdefined 两种|
|groupAttribute|object|是|机器分组的属性，默认为空|
|machineList|array|是|具体的机器标识，可以是 IP 或 userdefined-id|

groupAttribute 说明如下：

|属性名称|类型|是否必须|描述|
|:---|:-|:---|:-|
|groupTopic|string|否|机器分组的 topic，默认为空|
|externalName|string|否|机器分组所依赖的外部管理标识，默认为空|

 **请求头** 

CreateMachineGroup接口无特有请求头。关于 Log Service API 的公共请求头，请参考 [公共请求头](intl.zh-CN/API 参考/公共请求头.md)。

 **响应头** 

CreateMachineGroup接口无特有响应头。关于 Log Service API 的公共响应头，请参考 [公共响应头](intl.zh-CN/API 参考/公共响应头.md)。

 **响应元素** 

HTTP 状态码返回 200。

 **错误码** 

除了返回 Log Service API 的 [通用错误码](intl.zh-CN/API 参考/通用错误码.md)，还可能返回如下特有错误码：

|HTTP 状态码|ErrorCode|ErrorMessage|
|:-------|:--------|:-----------|
|400|MachineGroupAlreadyExist|group \{GroupName\} already exists|
|400|InvalidParameter|invalid group resource json|
|500|InternalServerError|Internal server error|

## 示例 {#section_e5p_d4t_12b .section}

**请求示例：** 

``` {#codeblock_vsd_oav_bf9}
POST /machinegroups HTTP/1.1
Header :
{
    "x-log-apiversion": "0.6.0",
    "Authorization": "LOG <yourAccessKeyId>:<yourSignature>",
    "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com",
    "Date": "Tue, 10 Nov 2015 17:57:33 GMT",
    "Content-Length": "187",
    "x-log-signaturemethod": "hmac-sha1",
    "Content-MD5": "82033D507DEAAD72067BB58DFDCB590D",
    "User-Agent": "sls-java-sdk-v-0.6.0",
    "Content-Type": "application/json",
    "x-log-bodyrawsize": "0"
}
Body :
{
    "groupName": "test-machine-group",
    "groupType": "",
    "machineIdentifyType": "ip",
    "groupAttribute":     {
        "groupTopic": "testtopic",
        "externalName": "testgroup"
    },
    "machineList":     [
        "127.0.0.1",
        "127.0.0.2"
    ]
}
```

 **响应示例：** 

``` {#codeblock_a7o_gbv_sqr}
HTTP/1.1 200 OK
Header :
{
    "Date": "Tue, 10 Nov 2015 17:57:33 GMT",
    "Content-Length": "0",
    "x-log-requestid": "5642300D99248CB76D005D36",
    "Connection": "close",
    "Server": "nginx/1.6.1"
}
```

