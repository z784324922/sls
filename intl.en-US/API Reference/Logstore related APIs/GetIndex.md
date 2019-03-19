# GetIndex {#reference_j3p_cch_f2b .reference}

Query the index of a specified Logstore.

Example:

```
GET /logstores/{logstoreName}/index
```

## Request syntax {#section_qwn_nch_f2b .section}

```
GET /logstores/<logstoreName>/index HTTP/1.1
Authorization: <AuthorizationString>
x-log-bodyrawsize: 0
User-Agent: <UserAgent>
x-log-apiversion: 0.6.0
Host: <Project Endpoint>
x-log-signaturemethod: hmac-sha1
Date: <GMT Date>
Content-Type: application/x-protobuf
Connection: Keep-Alive
```

## Request parameters {#section_m3m_vch_f2b .section}

|Attribute name|Type|Required or not|Description|
|:-------------|:---|:--------------|:----------|
|logstoreName|string|Yes|Logstore name.|

**Request header**

The GetIndex interface does not have a specific request header. For details about public request headers of Log Service APIs, see [Public request header](reseller.en-US/API Reference/Public request header.md).

**Response header**

The GetIndex interface does not have a specific response header. For details about public response headers of Log Service APIs, see [Public response header](reseller.en-US/API Reference/Public response header.md).

**Response element**

When the GetIndex request is sent successfully, the response body contains the indexes of a specified project and Logstore in the following format:

|Attribute name|Type|Required or not|
|:-------------|:---|:--------------|
|index\_mode|string|Index type.|
|keys|dict|Index configuration of the key value. Key indicates the field name, and value indicates the index configuration.|
|line|object|Full text index configuration.|
|storage|string|Storage type, which is pg at present.|
|ttl|integer|Index file lifecycle. The lifecycle can be 7 days, 30 days or 90 days.|
|lastModifyTime|integer|Last update time of the index in the format of Unix timestamp.|

The full text index configuration includes the following attributes:

|Attribute name|Type|Required or not|
|:-------------|:---|:--------------|
|caseSensitive|bool|Case sensitivity.|
|chn|bool|Include Chinese characters or not.|
|token|array|Word divider list.|
|include\_keys|array|List of included fields.|
|exclude\_keys|array|List of excluded fields.|

Configuration corresponding to each field in the key value index contains the following attributes:

|Attribute name|Type|Required or not|
|:-------------|:---|:--------------|
|type |string|Field type.|
|alias|string|Field alias.|
|chn|bool|Include Chinese characters or not. The field is available only when the value of type is text.|
|token|array|Word divider list. The field is available only when the value of type is text.|
|caseSensitive|bool|Case sensitivity. The field is available only when the value of type is text.|
|doc\_value|bool|Enable or disable statistics analysis on the field.|

**Error code**

The interface may return the following error codes in addition to Log Service API [Common error codes](reseller.en-US/API Reference/Common error codes.md):

|HTTP status code|ErrorCode|ErrorMessage|
|:---------------|:--------|:-----------|
|400|IndexConfigNotExist|index config doesn’t exist|
|404|ProjectNotExist|The Project does not exist : \{Project\}|
|404|LogStoreNotExist|logstore \{logstoreName\} dose not exist|
|500|InternalServerError|Specified Server Error Message|

## Example {#section_oht_5fh_f2b .section}

**Request example**

```
GET /logstores/logstore-4/index HTTP/1.1
Authorization: LOG <yourAccessKeyId>:<yourSignature>
x-log-bodyrawsize: 0
User-Agent: sls-java-sdk-v-0.6.1
x-log-apiversion: 0.6.0
Host: my-project.cn-shanghai.log.aliyuncs.com
x-log-signaturemethod: hmac-sha1
Date: Sun, 06 May 2018 13:08:42 GMT
Content-Type: application/x-protobuf
Connection: Keep-Alive
```

**Response example**

```
HTTP/1.1 200
Server: nginx/1.12.1
Content-Type: application/json
Content-Length: 712
Connection: close
Access-Control-Allow-Origin: *
Date: Sun, 06 May 2018 13:08:42 GMT
x-log-requestid: 5AEEFE5A8B8AEB5E6C82B395
{
  "index_mode": "v2",
  "keys": {
    "agent": {
      "alias": "",
      "caseSensitive": false,
      "chn": false,
      "doc_value": true,
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
      ],
      "type": "text"
    },
    "bytes": {
      "alias": "",
      "doc_value": true,
      "type": "long"
    },
    "remote_ip": {
      "alias": "",
      "caseSensitive": false,
      "chn": false,
      "doc_value": true,
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
      ],
      "type": "text"
    },
    "response": {
      "alias": "",
      "doc_value": true,
      "type": "long"
    }
  },
  "line": {
    "caseSensitive": false,
    "chn": false,
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
  "storage": "pg",
  "ttl": 30,
  "lastModifyTime": 1524155379
}
```

