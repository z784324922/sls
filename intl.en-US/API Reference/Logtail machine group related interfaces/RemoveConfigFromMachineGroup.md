# RemoveConfigFromMachineGroup {#reference_p1t_25q_12b .reference}

Remove a configuration from a machine group.

Example:

```
DELETE /machinegroups/{GroupName}/configs/{ConfigName}
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

|Parameter Name|Type|Required or not|Description|
|:-------------|:---|:--------------|:----------|
|GroupName|string|Yes|The machine group name.|
|ConfigName|string|Yes.|The Logtail configuration name.|

**Request Header**

The RemoveConfigFromMachineGroup API does not have a special request header. For more information about the public request headers of Log Service APIs, see [Public request header](reseller.en-US/API Reference/Public request header.md).

**Response header**

The RemoveConfigFromMachineGroup API does not have a special response header. For more information about the public response headers of Log Service APIs, see [Public response header](reseller.en-US/API Reference/Public response header.md).

**Response Element**

The returned HTTP status code is 200.

**Error code**

Besides the   [common error codes](reseller.en-US/API Reference/Common error codes.md) of Log Service APIs, the RemoveConfigFromMachineGroup API may return the following special error codes.

|HTTP status code|Error Code|ErrorMessage|
|:---------------|:---------|:-----------|
|404|GroupNotExist|group \{GroupName\} does not exist|
|404|ConfigNotExist|config \{ConfigName\} does not exist|
|500|InternalServerError|internal server error|

## Example {#section_e5p_d4t_12b .section}

**Request example:**

```
DELETE /machinegroups/sample-group/configs/logtail-config-sample
{
    "Content-Length": 0, 
    "x-log-signaturemethod": "hmac-sha1", 
    "x-log-bodyrawsize": 0, 
    "User-Agent": "log-python-sdk-v-0.6.0", 
    "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com", 
    "Date": "Mon, 09 Nov 2015 09:48:48 GMT", 
    "x-log-apiversion": "0.6.0", 
    "Authorization": "LOG 94to3z418yupi6ikawqqd370:t8v8y+zqOz3ZiqLDIkb6JQ8FUAU="
}
```

**Response example:**

```
{
    "date": "Mon, 09 Nov 2015 09:48:48 GMT", 
    "connection": "close", 
    "x-log-requestid": "56406C0099248CAA230BE135", 
    "content-length": "0", 
    "server": "nginx/1.6.1"
}
```

