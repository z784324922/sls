# DeleteIndex {#reference_ivq_pvz_c2b .reference}

Deletes an index for a specified Logstore.

Example:

```
POST /logstores/{logstoreName}/index
```

## Request syntax {#section_o54_dhh_f2b .section}

```
POST /logstores/<logstoreName>/index HTTP/1.1
Authorization: <AuthorizationString>
x-log-bodyrawsize: <x-log-bodyrawsize>
User-Agent: <User-Agent>
x-log-apiversion: <APIVersion>
Host: <Project Endpoint>
x-log-signaturemethod: hmac-sha1
Date: <GMT Date>
Content-Type: application/json
Content-MD5: <Content-MD5>
Content-Length: <Content-Length>
{
  "line": <full text index>,
  "keys": <key-value index>,
  "ttl": <ttl>
}
```

## Request parameters: {#section_lvh_2hh_f2b .section}

|**Attribute name**|**Type**|**Required or not**|**Description**|
|:-----------------|:-------|:------------------|:--------------|
|logstoreName|string|Yes|Logstore name.|
|ttl|integer|No|Index file lifecycle. The lifecycle can be 7 days, 30 days or 90 days.|
|keys|object|No |Index configuration of the key value. Key indicates the field name, and value indicates the index configuration.|
|line|object|No|Full text index configuration.|

Specify either keys or line. The full text index configuration includes the following attributes:

|**Attribute name**|**Type **|**Required or not**|**Description**|
|:-----------------|:--------|:------------------|:--------------|
|caseSensitive|bool|No |Case sensitivity.|
|chn|bool|No |Include Chinese characters or not.|
|token|array|Yes |Word divider list.|
|include\_keys|array|No |List of included fields.|
|exclude\_keys|array|No |List of excluded fields.|

The field index configuration includes the following attributes:

|**Attribute name**|**Type **|**Required or not**|**Description**|
|:-----------------|:--------|:------------------|:--------------|
|type |string|Yes|Field type.|
|alias|string|No |Field alias.|
|chn|bool|No |Include Chinese characters or not. This field makes sense only when the value of type is text.|
|token|array|This field is required only when the value of type is text.|Word divider list. This field makes sense only when the value of type is text.|
|caseSensitive|bool|No |Case sensitivity. This field makes sense only when the value of type is text.|
|doc\_value|bool |No |Enable or disable statistics analysis on the field.|

**Request header**

The CreateIndex interface does not have a specific request header. For details about public request headers of Log Service APIs, see [Public request header](reseller.en-US/API Reference/Public request header.md).

**Response header**

The CreateIndex interface does not have a specific response header. For details about public response headers of Log Service APIs, see [Public response header](reseller.en-US/API Reference/Public response header.md).

**Response element **

The returned HTTP status code is 200.

**Error code**

The interface may return the following error codes in addition to Log Service API [Common error codes](reseller.en-US/API Reference/Common error codes.md):

|**HTTP status code**|**ErrorCode**|**ErrorMessage**|
|:-------------------|:------------|:---------------|
|400|IndexInfoInvalid|required field token is lacking or of error format|
|400|IndexAlreadyExist|log store index is already created|
|404|ProjectNotExist|The Project does not exist : \{Project\}|
|404|LogStoreNotExist|logstore \{logstoreName\} dose not exist|
|500|InternalServerError|Specified Server Error Message|

**Note:** In the error message in the above table, \{Project\} and \{logstoreName\} indicate that this section will be replaced by a specific project name and Logstore name.

## Example {#section_p5z_ghh_f2b .section}

**Request example**

```
DELETE /logstores/my-logstore/index HTTP/1.1

Authorization: LOG <yourAccessKeyId>:<yourSignature>
x-log-bodyrawsize: 0
User-Agent: sls-java-sdk-v-0.6.1
x-log-apiversion: 0.6.0
Host: my-project.cn-shanghai.log.aliyuncs.com
x-log-signaturemethod: hmac-sha1
Date: Sun, 06 May 2018 13:22:57 GMT
Content-Type: application/x-protobuf
Connection: Keep-Alive
```

**Response example:**

```
HTTP/1.1 200
Server: nginx/1.12.1
Content-Length: 0
Connection: close
Access-Control-Allow-Origin: *
Date: Mon, 07 May 2018 09:43:16 GMT
x-log-requestid: 5AF01FB4826CF43228691C93
```

