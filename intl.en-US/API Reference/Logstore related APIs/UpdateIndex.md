# UpdateIndex {#reference_cmx_pdh_f2b .reference}

Update an index for a specified Logstore.

Example:

```
PUT /logstores/{logstoreName}/index
```

## Request syntax {#section_qwn_nch_f2b .section}

```
PUT /logstores/<logstoreName>/index HTTP/1.1
Authorization: <AuthorizationString>
x-log-bodyrawsize: 0
User-Agent: <UserAgent>
x-log-apiversion: 0.6.0
Host: <Project Endpoint>
x-log-signaturemethod: hmac-sha1
Date: <GMT Date>
Content-Type: application/json
Content-MD5: <Content-MD5>
Content-Length: <ContentLength>
{
  "line": <full text index>,
  "keys": <key-value index>
}
```

## Request parameters {#section_m3m_vch_f2b .section}

|Attribute name|Type |Required or not|Description|
|:-------------|:----|:--------------|:----------|
|logstoreName|string|Yes|Logstore name.|
|keys|object|No |Index configuration of the key value. Key indicates the field name, and value indicates the index configuration.|
|line|object|No |Full text index configuration.|

Specify either keys or line. The full text index configuration includes the following attributes:

|Attribute name|Type |Required or not|Description|
|:-------------|:----|:--------------|:----------|
|token|array|Yes |Word divider list.|
|caseSensitive|bool |No |Case sensitivity.|
|chn|bool|No |Include Chinese characters or not.|
|include\_keys|array|No|List of included fields, and the attribute cannot be specified with exclued\_keys at the same time.|
|exclude\_keys|array|No|List of excluded fields, and the attribute cannot be specified with exclued\_keys at the same time.|

The field index configuration includes the following attributes:

|Attribute name|Type |Required or not|Description|
|:-------------|:----|:--------------|:----------|
|type |string|Yes|Field type.|
|alias|string|No|Alias.|
|chn|bool|No|Include Chinese characters or not. This field makes sense only when the value of type is text.|
|token|array|No. This field is required when the value of type is text.|Word divider list. This field makes sense only when the value of type is text.|
|caseSensitive|bool|No|Case sensitivity. This field makes sense only when the value of type is text.|
|doc\_value|bool |No |Enable statistics or not.|

**Request header**

The UpdateIndex interface does not have a specific request header. For details about public request headers of Log Service APIs, see [Public request header](reseller.en-US/API Reference/Public request header.md).

**Response header**

UpdateIndex interface does not have a specific response header. For more information about public request headers of Log Service APIs, see [Public response header](reseller.en-US/API Reference/Public response header.md).

**Response element**

The system returns the HTTP status code 200.

**Error code**

The interface may return the following error codes in addition to Log Service API [Common error codes](reseller.en-US/API Reference/Common error codes.md):

|HTTP status code|ErrorCode|ErrorMessage|
|:---------------|:--------|:-----------|
|400|ParameterInvalid|log store index is not created|
|400 |ParameterInvalid|key config must has type|
|400|IndexInfoInvalid|required field token is lacking or of error format|
|400|IndexInfoInvalid|required fields line/keys are lacking or of error format|
|404|ProjectNotExist|The Project does not exist : \{Project\}|
|404|LogStoreNotExist|logstore \{logstoreName\} dose not exist|
|500|InternalServerError|Specified Server Error Message|

## Example {#section_t3z_rfh_f2b .section}

-   Request example

    ```
    PUT /logstores/logstore-4/index HTTP/1.1
    Header:
    Authorization: LOG <yourAccessKeyId>:<yourSignature>
    x-log-bodyrawsize: 0
    User-Agent: sls-java-sdk-v-0.6.1
    x-log-apiversion: 0.6.0
    Host: my-project.cn-shanghai.log.aliyuncs.com
    x-log-signaturemethod: hmac-sha1
    Date: Mon, 07 May 2018 09:47:20 GMT
    Content-Type: application/json
    Content-MD5: 1860B805A6AA5288B97B32CF3B519112
    Content-Length: 316
    Connection: Keep-Alive
    Body:
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

-   Response example

    ```
    HTTP/1.1 200
    Server: nginx/1.12.1
    Content-Length: 0
    Connection: close
    Access-Control-Allow-Origin: *
    Date: Mon, 07 May 2018 09:47:21 GMT
    x-log-requestid: 5AF020A98CBAA2EF52AB66C9
    ```


