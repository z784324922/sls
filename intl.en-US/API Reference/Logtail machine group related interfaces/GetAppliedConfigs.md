# GetAppliedConfigs {#reference_i1s_f5q_12b .reference}

Obtain the name of the configuration applied to a machine group.

Example:

```
GET /machinegroups/{groupName}/configs
```

## Request syntax {#section_j5v_14t_12b .section}

```
GET /machinegroups/{groupName}/configs HTTP/1.1
Authorization: <AuthorizationString> 
Date: <GMT Date>
Host: <Project Endpoint>
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

## Request parameters {#section_mx5_b4t_12b .section}

URL parameters

|Parameter name|Type|Required|Description|
|:-------------|:---|:-------|:----------|
|groupName|string|Yes|Machine grouping name|

**Request header**

The GetAppliedConfigs API does not have a special request header. For more information about the public request headers of Log Service APIs, see  [Public request header](intl.en-US/API Reference/Public request header.md).

**Response header**

The GetAppliedConfigs API does not have a special response header. see  [Public response header](intl.en-US/API Reference/Public response header.md).

**Responsee element**

After the successful request, the response body contains a list of all machines in a specific machine group.  The specific formats are as follows.

|Name|Type|Description|
|:---|:---|:----------|
|count|Integer|The number of returned configurations.|
|configs|string array|The name list of returned configurations.|

```
{
    "count":2,
    "configs":
    ["config1","config2"]
}
```

**Error code**

Besides the  [common error codes](intl.en-US/API Reference/Common error codes.md) of Log Service APIs, the GetAppliedConfigs API may return the following special error codes.

|HTTP status code|ErrorCode|ErrorMessage|
|:---------------|:--------|:-----------|
|404|GroupNotExist|group \{GroupName\} does not exist|
|500|InternalServerError|internal server error|

## Example {#section_e5p_d4t_12b .section}

**Request example**

```
GET /machinegroups/test-machine-group/configs HTTP/1.1
Header :
{
    "x-log-apiversion": "0.6.0",
    "Authorization": "LOG 94to3z418yupi6ikawqqd370:/Ntg290OaJ8JfInmhzYTG/GJwbE=",
    "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com",
    "Date": "Tue, 10 Nov 2015 19:45:48 GMT",
    "Content-Length": "0",
    "x-log-signaturemethod": "hmac-sha1",
    "User-Agent": "sls-java-sdk-v-0.6.0",
    "Content-Type": "application/x-protobuf",
    "x-log-bodyrawsize": "0"
}
```

**Response example**

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
    "configs": [
        "two",
        "three",
        "test_logstore"
    ],
    "count": 3
}
```

