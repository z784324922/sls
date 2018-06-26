# UpdateMachineGroup {#reference_ok3_c5q_12b .reference}

Update the machine group information. If the machine group has applied a configuration, the configuration is automatically added or removed when machines are added to or removed from the machine group.

Example:

```
PUT /machinegroups/{groupName}
```

## Request syntax {#section_j5v_14t_12b .section}

```
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

## Request Parameters {#section_mx5_b4t_12b .section}

URL parameters:

|Parameter Name|Type|Required|Description|
|:-------------|:---|:-------|:----------|
|groupName|string|Yes|The machine group name.|

Body parameters:

|Attribute name |Type|Required|Description|
|---------------|----|--------|-----------|
|groupName|String|Yes|The machine group name, which is unique in the same project.|
|groupType|string|No|The machine group type, which is empty by default.|
|machineIdentifyType|string|Yes|The machine identification type, including IP and user-defined identity.|
|groupAttribute|object|Yes|The machine group attribute, which is empty by default.|
|machineList|array|Yes|The specific machine identification, which can be an IP address or user-defined identity.|

groupAttribute description

|Attribute name|Type|类型|Description|
|:-------------|:---|:-|:----------|
|groupTopic|string|No|The topic of a machine group, which is empty by default.|
|externalName|string|No|The external identification that the machine group depends, which is empty by default.|

**Request Header**

The UpdateMachineGroup API does not have a special request header.  For more information about the public request headers of Log Service APIs, see [Public request header](intl.en-US/API Reference/Public request header.md).

**Response header**

The UpdateMachineGroup API does not have a special response header.  For more information about the public response headers of Log Service APIs, see [Public response header](intl.en-US/API Reference/Public response header.md).

**Response element**

The returned HTTP status code is 200.

**Error code**

Besides  [the common error codes](intl.en-US/API Reference/Common error codes.md) of Log Service APIs, the UpdateMachineGroup API may return the following special error codes.

|HTTP 状态码|ErrorCode|ErrorMessage|
|:-------|:--------|:-----------|
|404|GroupNotExist|group \{GroupName\} does not exist|
|400|InvalidParameter|invalid group resource json|
|500|InternalServerError|internal server error|

## 示例 {#section_e5p_d4t_12b .section}

**请求示例：**

```
PUT /machinegroups/test-machine-group HTTP/1.1
Header :
{
    "x-log-apiversion": "0.6.0",
    "Authorization": "LOG 94to3z418yupi6ikawqqd370:ZJmBDS+LjRCzgSLuo21vFh6o7CE=",
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
    "groupAttribute": {
        "groupTopic": "testtopic2",
        "externalName": "testgroup2"
    },
    "machineList": [
        "uu_id_1",
        "uu_id_2"
    ]
}
```

**Response example:**

```
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

