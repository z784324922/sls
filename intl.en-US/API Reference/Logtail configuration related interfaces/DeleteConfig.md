# DeleteConfig {#reference_nt5_j5q_12b .reference}

Delete a specific configuration. If the configuration has been [applied to the machine group](reseller.en-US/API Reference/Logtail machine group related interfaces/ApplyConfigToMachineGroup.md), logs cannot be collected based  on this configuration.

Example:

```
DELETE /configs/{configName}
```

## Request syntax {#section_j5v_14t_12b .section}

```
DELETE /configs/<configName> HTTP/1.1
Authorization: <AuthorizationString> 
Date: <GMT Date>
Host: <Project Endpoint>
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

## Request parameters {#section_mx5_b4t_12b .section}

URL parameters:

|Parameter|Type|Required |Description|
|---------|----|---------|-----------|
|ConfigName|String|Yes|The configuration name.|

**Request header**

No special request header is available. For more information about the public request headers of Log Service APIs, see [Public request header](reseller.en-US/API Reference/Public request header.md).

**Response header**

No special response header is available. For more information about the public response headers of Log Service APIs, see [Public response header](reseller.en-US/API Reference/Public response header.md).

**Response element**

The system returns the HTTP status code 200.

**Error codes**

Besides the common error codes of Log Service APIs, the GetLogs API may return the following special error codes.

|HTTP status code|ErrorCode|ErrorMessage|
|:---------------|:--------|:-----------|
|404|ConfigNotExist|config \{Configname\} does not exist|
|400|InvalidParameter|invalid config resource json|
|500|InternalServerError|internal server error|

## Example {#section_e5p_d4t_12b .section}

**Request example**

```
DELETE /configs/logtail-config-sample
Header :
{
    "Content-Length": 0, 
    "x-log-signaturemethod": "hmac-sha1", 
    "x-log-bodyrawsize": 0, 
    "User-Agent": "log-python-sdk-v-0.6.0", 
    "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com", 
    "Date": "Mon, 09 Nov 2015 09:28:21 GMT", 
    "x-log-apiversion": "0.6.0", 
    "Authorization": "LOG 94to3z418yupi6ikawqqd370:utd/O1JNCYvcRGiSXHsjKhTzJDI="
}
```

**Response example**

```
Header : 
{
    "date": "Mon, 09 Nov 2015 09:28:21 GMT", 
    "connection": "close", 
    "x-log-requestid": "5640673599248CAA230836C6", 
    "content-length": "0", 
    "server": "nginx/1.6.1"
}
```

