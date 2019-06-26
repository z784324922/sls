# UpdateMachineGroup {#reference_ok3_c5q_12b .reference}

更新机器组信息，如果机器组已应用配置，则新加入、减少机器会自动增加、移除配置。

示例：

``` {#codeblock_ffn_3se_6c1}
PUT /machinegroups/{groupName}
```

## 请求语法 {#section_j5v_14t_12b .section}

``` {#codeblock_fpx_65j_b2j}
PUT /machinegroups/{groupName} HTTP/1.1
Authorization: <AuthorizationString> 
Content-Type:application/json
Content-Length:<Content Length>
Content-MD5<:<Content MD5>
Date: <GMT Date>
Host: <Project Endpoint>
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
{
    "groupName": "test-machine-group",
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

URL 参数：

|参数名称|类型|是否必须|描述|
|:---|:-|:---|:-|
|groupName|string|是|机器分组名称|

Body 参数：

|属性名称|类型|是否必须|描述|
|----|--|----|--|
|groupName|string|是|机器分组名称，Project 下唯一|
|groupType|string|否|机器分组类型，默认为空|
|machineIdentifyType|string|是|机器标识类型，分为 ip 和 userdefined 两种|
|groupAttribute|object|是|机器分组的属性，默认为空|
|machineList|array|是|具体的机器标识，可以是 ip 或 userdefined-id|

groupAttribute 说明如下：

|属性名称|类型|是否必须|描述|
|:---|:-|:---|:-|
|groupTopic|string|否|机器分组的 topic，默认为空|
|externalName|string|否|机器分组所依赖的外部管理标识，默认为空|

 **请求头** 

无特有请求头。关于 Log Service API 的公共请求头，请参考 [公共请求头](intl.zh-CN/API 参考/公共请求头.md)。

 **响应头** 

无特有响应头。关于 Log Service API 的公共请求头，请参考 [公共响应头](intl.zh-CN/API 参考/公共响应头.md)。

 **响应元素** 

HTTP 状态码返回 200。

 **错误码** 

除了返回 Log Service API 的 [通用错误码](intl.zh-CN/API 参考/通用错误码.md)，还可能返回如下特有错误码：

|HTTP 状态码|ErrorCode|ErrorMessage|
|:-------|:--------|:-----------|
|404|GroupNotExist|group \{GroupName\} does not exist|
|400|InvalidParameter|invalid group resource json|
|500|InternalServerError|internal server error|

## 示例 {#section_e5p_d4t_12b .section}

 **请求示例：** 

``` {#codeblock_w6k_4co_say}
PUT /machinegroups/test-machine-group HTTP/1.1
Header :
{
    "x-log-apiversion": "0.6.0",
    "Authorization": "LOG <yourAccessKeyId>:<yourSignature>",
    "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com",
    "Date": "Tue, 10 Nov 2015 18:41:43 GMT",
    "Content-Length": "194",
    "x-log-signaturemethod": "hmac-sha1",
    "Content-MD5": "2CEBAEBE53C078891527CB70A855BAF4",
    "User-Agent": "sls-java-sdk-v-0.6.0",
    "Content-Type": "application/json",
    "x-log-bodyrawsize": "0"
}
Body :
{
    "groupName": "test-machine-group",
    "groupType": "",
    "machineIdentifyType": "userdefined",
    "groupAttribute":     {
        "groupTopic": "testtopic2",
        "externalName": "testgroup2"
    },
    "machineList":     [
        "uu_id_1",
        "uu_id_2"
    ]
}
```

 **响应示例：** 

``` {#codeblock_1qe_q82_l3p}
HTTP/1.1 200 OK
Header :
{
    "Date": "Tue, 10 Nov 2015 18:41:43 GMT",
    "Content-Length": "0",
    "x-log-requestid": "56423A6799248CA57B00035C",
    "Connection": "close",
    "Server": "nginx/1.6.1"
}
```

