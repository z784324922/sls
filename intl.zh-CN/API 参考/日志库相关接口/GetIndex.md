# GetIndex {#reference_j3p_cch_f2b .reference}

查询指定 Logstore 的索引。

示例：

```
GET /logstores/{logstoreName}/index
```

## 请求语法 {#section_qwn_nch_f2b .section}

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

## 请求参数 {#section_m3m_vch_f2b .section}

|属性名称|类型|是否必须|描述|
|:---|:-|:---|:-|
|logstoreName|string|是|Logstore 的名称。|

**请求头**

GetIndex 接口无特有请求头，关于 Log Service API 的公共请求头请参考 [公共请求头](intl.zh-CN/API 参考/公共请求头.md)。

**响应头**

GetIndex 接口无特有响应头，关于 Log Service API 的公共响应头请参考[公共响应头](intl.zh-CN/API 参考/公共响应头.md)。

**响应元素**

GetIndex 请求成功，其响应 Body 会包括指定 Project和Logstore的Index，具体格式如下。

|属性名称|类型|描述|
|:---|:-|:-|
|index\_mode|string|Index 类型。|
|keys|dict|字段索引配置，key为字段名称，value为字段索引配置。|
|line|object|全文索引配置。|
|storage|string|存储类型，目前为pg。|
|ttl|integer|索引文件生命周期，支持7天，30天和90天。|
|lastModifyTime|integer|Index 最后更新时间，UNIX时间戳。|

全文索引配置包含如下属性：

|属性名称|类型|描述|
|:---|:-|:-|
|caseSensitive|bool|是否大小写敏感。|
|chn|bool|是否包含中文。|
|token|array|分词符列表。|
|include\_keys|array|包含的字段列表。|
|exclude\_keys|array|排除的字段列表。|

字段索引每个字段对应的配置包含如下属性：

|属性名称|类型|描述|
|:---|:-|:-|
|type|string|字段类型。|
|alias|string|字段别名。|
|chn|bool|是否包含中文，只有type为text时才存在。|
|token|array|分词符列表，只有type为text时才存在。|
|caseSensitive|bool|是否大小写敏感，只有type为text时才存在。|
|doc\_value|bool|字段是否开启统计|

**错误码**

除了返回 Log Service API 的[通用错误码](intl.zh-CN/API 参考/通用错误码.md)，还可能返回如下特有错误码：

|HTTP状态码|ErrorCode|ErrorMessage|
|:------|:--------|:-----------|
|400|IndexConfigNotExist|index config doesn’t exist|
|404|ProjectNotExist|The Project does not exist : \{Project\}|
|404|LogStoreNotExist|logstore \{logstoreName\} dose not exist|
|500|InternalServerError|Specified Server Error Message|

## 示例 {#section_oht_5fh_f2b .section}

**请求示例**

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

**响应示例**

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

