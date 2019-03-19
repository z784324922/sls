# ListProject {#reference_wdt_tgh_f2s .reference}

Query the list of all projects.

Example

```
GET /? offset={offset}&size={size}
```

## Request syntax {#section_o54_dhh_f2b .section}

```
GET /? offset={offset}&size={size} HTTP/1.1
Authorization: <AuthorizationString>
x-log-bodyrawsize: 0
User-Agent: <UserAgent>
x-log-apiversion: 0.6.0
Host: <Endpoint>
x-log-signaturemethod: hmac-sha1
Date: <GMT Date>
Content-Type: application/x-protobuf
Connection: Keep-Alive
```

## Request parameters {#section_lvh_2hh_f2b .section}

|Attribute name|Type |Required|Description|
|:-------------|:----|:-------|:----------|
|offset|integer|No |Start position of a returned record. The default value is 0.|
|size|integer|No |Maximum number of entries returned per page. The default value is 500 \(maximum\).|

**Request header**

The ListProject interface does not have a specific request header. For details about public request headers of Log Service APIs, see [Public request header](reseller.en-US/API Reference/Public request header.md).

**Response header**

The ListProject interface does not have a specific response header. For details about public response headers of Log Service APIs, see [Public response header](reseller.en-US/API Reference/Public response header.md).

**Response element**

If the ListProject request is sent successfully, the response body contains a project list in the following format:

|Attribute name| Type|Description|
|:-------------|:----|:----------|
|count|integer|Number of projects returned.|
|total|integer|Total number of projects.|
|projects|array|Project list.|

Each element of projects is shown in the following format:

|Attribute name|Type|Description|
|:-------------|:---|:----------|
|createTime|string|Creation time.|
|description|string|Project description.|
|lastModifyTime|string|Last update time.|
|owner|string|Account ID of the project creator.|
|projectName|string|Project name.|
|status|string|Project status.|
|region|string|List of excluded fields.|

**Error code**

The interface may return the following error codes in addition to Log Service API [Common error codes](reseller.en-US/API Reference/Common error codes.md):

|**HTTP status code**|**ErrorCode**|**Error message**|
|:-------------------|:------------|:----------------|
|500 |InternalServerError|Specified Server Error Message|

## Example {#section_p5z_ghh_f2b .section}

**Request example**

```
GET /? offset=0&size=2&projectName= HTTP/1.1
Authorization: LOG <yourAccessKeyId>:<yourSignature>
x-log-bodyrawsize: 0
User-Agent: sls-java-sdk-v-0.6.1
x-log-apiversion: 0.6.0
Host: cn-shanghai.log.aliyuncs.com
x-log-signaturemethod: hmac-sha1
Date: Sun, 27 May 2018 09:03:33 GMT
Content-Type: application/x-protobuf
Connection: Keep-Alive
```

**Response example:**

```
HTTP/1.1 200
Server: nginx
Content-Type: application/json
Content-Length: 345
Connection: close
Access-Control-Allow-Origin: *
Date: Sun, 27 May 2018 09:03:33 GMT
x-log-requestid: 5B0A7465AAEA20CA70DE3064
{
  "count": 2,
  "total": 11,
  "projects": [
    {
      "projectName": "project1",
      "status": "Normal",
      "owner": "",
      "description": "",
      "region": "cn-shanghai",
      "createTime": "1524222931",
      "lastModifyTime": "1524539357"
    },
    {
      "projectName": "project123456",
      "status": "Normal",
      "owner": "",
      "description": "",
      "region": "cn-shanghai",
      "createTime": "1471963876",
      "lastModifyTime": "1524539357"
    }
  ]
}
```

