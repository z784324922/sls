# GetAppliedMachineGroups {#reference_cld_35q_12b .reference}

List the machines that apply the configuration.

Example:

```
GET /configs/{configName}/machinegroups
```

## Request syntax {#section_j5v_14t_12b .section}

```
GET /configs/{configName}/machinegroups HTTP/1.1
Authorization: <AuthorizationString> 
Date: <GMT Date>
Host: <Project Endpoint>
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

## Request Parameters  {#section_mx5_b4t_12b .section}

URL parameters:

|Parameter Name|Type|Required|Description|
|:-------------|:---|:-------|:----------|
|ConfigName|string|Yes|Configuration name|

**Request Header**

The GetAppliedMachineGroups API does not have a special request header.  For more information about the public request headers of Log Service APIs, see [Public request header](intl.en-US/API Reference/Public request header.md).

**Response header**

The GetAppliedMachineGroups API does not have a special response header. For more information about the public response headers of Log Service APIs, see [Public response header](intl.en-US/API Reference/Public response header.md).

**Response Element**

After the successful request, the response body contains a list of all machines in a specific machine group. The  List, in the following format:

|Name|Type|Description|
|:---|:---|:----------|
|count|Integer|The number of returned machine groups.The number of machineggroup returned.|
|machinegroups|String Array|The name list of returned machine groups.|

```
{
    "count":2,
    "machinegroups":
    ["group1","group2"]
}
```

**Error code**

Besides the   [common error codes](intl.en-US/API Reference/Common error codes.md) of Log Service APIs, the GetAppliedMachineGroups API may return the following special error codes.

|HTTP status code|Error Code|Error Message|
|:---------------|:---------|:------------|
|404|GroupNotExist|group \{GroupName\} does not exist|
|500|InternalServerError|internal server error|

## Example {#section_e5p_d4t_12b .section}

**Request example:**

```
GET /configs/logtail-config-sample/machinegroups
Header:
{
    "Content-Length": 0, 
    "x-log-signaturemethod": "hmac-sha1", 
    "x-log-bodyrawsize": 0, 
    "User-Agent": "log-python-sdk-v-0.6.0", 
    "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com", 
    "Date": "Mon, 09 Nov 2015 09:51:38 GMT", 
    "x-log-apiversion": "0.6.0", 
    "Authorization": "LOG 94to3z418yupi6ikawqqd370:+6bo4MSUt/dyNa72kXeGCkVOi+4="
}
```

**Response example**

```
Header : 
{
    "content-length": "44", 
    "server": "nginx/1.6.1", 
    "connection": "close", 
    "date": "Mon, 09 Nov 2015 09:51:38 GMT", 
    "content-type": "application/json", 
    "x-log-requestid": "56406CAA99248CAA230BE828"
}
Body:
{
    "count": 1, 
    "machinegroups": 
    [
        "sample-group"
    ]
}
```

