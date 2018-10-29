# CreateIndex {#reference_cqb_mvz_c2b .reference}

Create an index for a specified Logstore.

**Example**:

```
POST /logstores/{logstoreName}/index
```

## Request syntax {#section_r1f_5vz_c2b .section}

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
  "keys": <key-value index>
}
```

## Request parameters {#section_nyl_yvz_c2b .section}

|Attribute name|Type |Required or not|Description|
|:-------------|:----|:--------------|:----------|
|logstoreName|string|Yes|Logstore name.|
|keys|object|No |Index configuration of the key value. Key indicates the field name, and value indicates the index configuration.|
|line|object|No |Full text index configuration.|

Specify either keys or line. The full text index configuration includes the following attributes:

|Attribute name|Type |Required or not|Description|
|:-------------|:----|:--------------|:----------|
|caseSensitive|bool|No |Case sensitivity.|
|chn|bool|No |Include Chinese characters or not.|
|token|array|是|Word divider list.|
|include\_keys|array|No |List of included fields, and the attribute cannot be specified with exclude\_keys at the same time.|
|exclude\_keys|array|No |List of excluded fields, and the attribute cannot be specified with exclude\_keys at the same time.|

The field index configuration includes the following attributes:

|Attribute name|Type |Required|Description|
|:-------------|:----|:-------|:----------|
|type |string|Yes|Field type.|
|alias|string|No |Field alias.|
|chn|bool|No |Include Chinese characters or not. This field makes sense only when the value of type is text.|
|token|array|This field is required only when the value of type is text.|Word divider list. This field makes sense only when the value of type is text.|
|caseSensitive|bool |No |Case sensitivity. This field makes sense only when the value of type is text.|
|doc\_value|bool|No |Enable or disable statistics analysis on the field.|

**Request header**

The CreateIndex interface does not have a specific request header. For details about public request headers of Log Service APIs, see  [Public request header](reseller.en-US/API Reference/Public request header.md).

**Response header**

The CreateIndex interface does not have a specific response header. For details about public response headers of Log Service APIs, see[Public response header](reseller.en-US/API Reference/Public response header.md) .

**Response element **

The returned HTTP status code is 200.

**Error code**

The interface may return the following error codes in addition to Log Service API [Common error codes](reseller.en-US/API Reference/Common error codes.md):

|HTTP status code|ErrorCode|ErrorMessage|
|:---------------|:--------|:-----------|
|400|IndexInfoInvalid|required field token is lacking or of error format|
|400|IndexAlreadyExist|log store index is already created|
|404|ProjectNotExist|The Project does not exist : \{Project\}|
|404|LogStoreNotExist|logstore \{logstoreName\} dose not exist|
|500|InternalServerError|Specified Server Error Message|

**Note:** In the error message in the above table, \{Project\} and \{logstoreName\} indicate that this section will be replaced by a specific project name and Logstore name.

## Example {#section_sdw_lwz_c2b .section}

**Request example**

```
POST /logstores/my-logstore/index HTTP/1.1
Authorization: LOG LTRTfdR7fbosJYad:OK7Sldsxcv/8gz6YtrrmzR19Tgh=
x-log-bodyrawsize: 0
User-Agent: sls-java-sdk-v-0.6.1
x-log-apiversion: 0.6.0
Host: my-project.cn-shanghai.log.aliyuncs.com
x-log-signaturemethod: hmac-sha1
Date: Mon, 07 May 2018 09:43:16 GMT
Content-Type: application/json
Content-MD5: 22876515FC311F857AD6C7902F6A0148
Content-Length: 316
Connection: Keep-Alive
{
  "line": {
    "token": [
      ",",
      " ",
      "'",
      "\"",
      ";",
      "=",
      "(",
      ")",
      "[",
      "]",
      "{",
      "}",
      "?",
      "@",
      "&",
      "<",
      ">",
      "/",
      ":",
      "\n",
      "\t",
      "\r"
    ]
  },
  "keys": {
    "agent": {
      "doc_value": true,
      "caseSensitive": true,
      "alias": "agent_alias",
      "type": "text",
      "token": [
        ",",
        " ",
        "'",
        "\"",
        ";",
        "=",
        "(",
        ")",
        "[",
        "]",
        "{",
        "}",
        "?",
        "@",
        "&",
        "<",
        ">",
        "/",
        ":",
        "\n",
        "\t",
        "\r"
      ]
    }
  }
}
```

