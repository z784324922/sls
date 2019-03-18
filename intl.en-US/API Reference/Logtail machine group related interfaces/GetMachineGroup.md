# GetMachineGroup {#reference_swr_d5q_12b .reference}

View details of a machine group.

Example:

```
GET /machinegroups/{groupName}
```

## Request syntax {#section_j5v_14t_12b .section}

```
GET /machinegroups/{groupName} HTTP/1.1
Authorization: <AuthorizationString> 
Date: <GMT Date>
Host: <Project Endpoint>
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

## Request parameters {#section_mx5_b4t_12b .section}

URL parameters:

|Parameter Name|Type|Required|Description|
|:-------------|:---|:-------|:----------|
|groupName|string|Yes|The machine group name.|

**Request Header**

The GetMachineGroup API does not have a special request header.  For more information about the public request headers of Log Service APIs, see [Public request header](reseller.en-US/API Reference/Public request header.md).

**Response header**

The GetMachineGroup API does not have a special response header.  For more information about the public response headers of Log Service APIs, see [Public response header](reseller.en-US/API Reference/Public response header.md).

**Response element**

|Attribute name |Type|Description|
|---------------|----|-----------|
|groupName|string|The machine group name, which is unique in the same project.|
|groupType|string|The machine group type \(empty or Armory\), which is empty by default.|
|machineIdentifyType|string|The machine group type \(empty or Armory\), which is empty by default.|
|groupAttribute|4\) JSON object|The machine identification type, including IP and user-defined identity.|
|machineList|json array|The specific machine identification, which can be an IP address or user-defined identity.|
|createTime|int|The created time of the machine group.|
|lastModifyTime|int|The last updated time of the machine group.|

groupAttribute description

|Attribute name|Type|Required|Description|
|:-------------|:---|:-------|:----------|
|groupTopic|string|No|The topic of a machine group, which is generally not configured.|
|externalName|string|No| The external system \(Armory\) identification that the machine group depends. External Management System \(armory\) Identification on which Machine grouping is dependent|

```
{
    "groupName": "test-machine-group",
    "groupType": "",
    "groupAttribute": {
        "externalName": "testgroup",
        "groupTopic": "testtopic"
    },
    "machineIdentifyType": "ip",
    "machineList": [
        \"127.0.0.1\
        \"127.0.0.2\"
    ],
    "createTime": 1447178253,
    "lastModifyTime": 1447178253
}
```

**Error code**

Besides the common error codes of Log Service APIs,  the GetMachineGroup API may return the following  [special error codes](reseller.en-US/API Reference/Common error codes.md).

|HTTP status code |Error code|Error message|
|:----------------|:---------|:------------|
|404|GroupNotExist|group \{GroupName\} does not exist|
|500|InternalServerError|internal server error|

## Example {#section_e5p_d4t_12b .section}

**Request example:**

```
GET /machinegroups/test-machine-group HTTP/1.1
Header :
{
    "x-log-apiversion": "0.6.0",
    "Authorization": "LOG AK\_ID:Signature",
    "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com",
    "Date": "Tue, 10 Nov 2015 18:15:24 GMT",
    "Content-Length": "0",
    "x-log-signaturemethod": "hmac-sha1",
    "User-Agent": "sls-java-sdk-v-0.6.0",
    "Content-Type": "application/x-protobuf",
    "x-log-bodyrawsize": "0"
}
```

**Response example:**

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
    "groupAttribute": {
        "externalName": "testgroup",
        "groupTopic": "testtopic"
    },
    "machineIdentifyType": "ip",
    "machineList": [
        \"127.0.0.1\
        \"127.0.0.2\"
    ],
    "createTime": 1447178253,
    "lastModifyTime": 1447178253
}
```

