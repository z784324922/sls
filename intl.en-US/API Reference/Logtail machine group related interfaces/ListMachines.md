# ListMachines {#reference_fnf_f5q_12b .reference}

Obtain the status of your machine that is in the machine group and connected to the server.

Example:

```
GET /machinegroups/{groupName}/machines? offset=1&size=10
```

## Request syntax {#section_j5v_14t_12b .section}

```
GET /machinegroups/{groupName}/machines HTTP/1.1
Authorization: <AuthorizationString> 
Date: <GMT Date>
Host: <Project Endpoint>
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

## Request Parameters {#section_mx5_b4t_12b .section}

URL parameters:

|Name|Type|Required|Description|
|:---|:---|:-------|:----------|
|groupName|string|Yes|The machine group name.|
|offset|int|No|The starting position of the returned records. The default value is 0.|
|size|int|No|The maximum number of entries returned on each page. The default value is 500 \(maximum\).|

**Request Header**

The ListMachines API does not have a special request header.  For more information about the public request headers of Log Service APIs, see [Public request header](reseller.en-US/API Reference/Public request header.md).

**Response header**

The ListMachines API does not have a special response header. For more information about the public response headers of Log Service APIs, see [Public response header](reseller.en-US/API Reference/Public response header.md).

**Response Element**

|Name|Type|Description|
|:---|:---|:----------|
|Count|Int| The number of returned machines.|
|total|int| The total number of machines.|
|machines|json array|The name list of returned machines.|

Machine description

|Name|Type|Description|
|:---|:---|:----------|
|ip|string|The IP address of the machine.|
|machine-uniqueid|string|The DMI UUID of the machine.|
|userdefined-id|String|The user-defined identity of the machine.|

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

**Error Code**

Besides the [common error codes](reseller.en-US/API Reference/Common error codes.md) of Log Service APIs, the ListMachines API may return the following special error codes.

|HTTP status code|ErrorCode|Error Message|
|:---------------|:--------|:------------|
|404|GroupNotExist|group \{GroupName\} does not exist|
|500|InternalServerError|internal server error|

**Note:** This API only obtains the list of machines that are normally connected to the server.

## Example {#section_e5p_d4t_12b .section}

**Request example:**

```
GET /machinegroups/test-machine-group-5/machines? offset=0&size=3 HTTP/1.1
Header :
{
    "x-log-apiversion": "0.6.0",
    "Authorization": "LOG <yourAccessKeyId>:<yourSignature>",
    "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com",
    "Date": "Tue, 10 Nov 2015 19:04:57 GMT",
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
    "Date": "Tue, 10 Nov 2015 19:04:58 GMT",
    "Content-Length": "324",
    "x-log-requestid": "56423FD999248C827B000A57",
    "Connection": "close",
    "Content-Type": "application/json",
    "Server": "nginx/1.6.1"
}
Body :
{
    "machines": [
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

