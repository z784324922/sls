# ListProject {#reference_wdt_tgh_f2s .reference}

查询所有 Project 列表。

示例：

```
GET /?offset={offset}&size={size}
```

## 请求语法 {#section_o54_dhh_f2b .section}

```
GET /?offset={offset}&size={size} HTTP/1.1
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

## 请求参数 {#section_lvh_2hh_f2b .section}

|属性名称|类型|是否必须|描述|
|:---|:-|:---|:-|
|offset|integer|否|返回记录的起始位置，默认值为 0。|
|size|integer|否|每页返回最大条目，默认 500（最大值）。|

**请求头**

ListProject接口无特有请求头，关于 Log Service API 的公共请求头请参考[公共请求头](intl.zh-CN/API 参考/公共请求头.md)。

**响应头**

ListProject接口无特有响应头，关于 Log Service API 的公共响应头请参考[公共响应头](intl.zh-CN/API 参考/公共响应头.md)。

**响应元素**

ListProject请求成功，其响应 Body 中包含 Project 列表，具体格式如下：

|属性名称|类型|描述|
|:---|:-|:-|
|count|integer|返回的 Project 个数。|
|total|integer|Project 总数。|
|projects|array|Project 列表。|

projects 中每个元素的格式为：

|属性名称|类型|描述|
|:---|:-|:-|
|createTime|string|创建时间。|
|description|string|Project 描述|
|lastModifyTime|string|最后一次更新时间。|
|owner|string|创建人的账户Id。|
|projectName|string|Project名称。|
|status|string|Project 状态。|
|region|string|排除的字段列表。|

**错误码**

除了返回 Log Service API 的[通用错误码](intl.zh-CN/API 参考/通用错误码.md)，还可能返回如下特有错误码：

|**HTTP状态码**|**ErrorCode**|**ErrorMessage**|
|:----------|:------------|:---------------|
|500|InternalServerError|Specified Server Error Message|

## 示例 {#section_p5z_ghh_f2b .section}

**请求示例：**

```
GET /?offset=0&size=2&projectName= HTTP/1.1
Authorization: LOG LTRTfdR7fbosJYad:OK7Sldsxcv/8gz6YtrrmzR19Tgh=
x-log-bodyrawsize: 0
User-Agent: sls-java-sdk-v-0.6.1
x-log-apiversion: 0.6.0
Host: cn-shanghai.log.aliyuncs.com
x-log-signaturemethod: hmac-sha1
Date: Sun, 27 May 2018 09:03:33 GMT
Content-Type: application/x-protobuf
Connection: Keep-Alive
```

**响应示例：**

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

