# GetLogstore  {#reference_nsj_vtq_12b .reference}

View Logstore attributes.

## Request syntax  {#section_j5v_14t_12b .section}

```
GET/logstores/{logstorename} HTTP/1.1
Authorization: <AuthorizationString> 
Date: <GMT Date>
Host: <Project Endpoint>
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

## Request parameters {#section_mx5_b4t_12b .section}

|Parameter name |Type |Required|Description |
|:--------------|:----|:-------|:-----------|
|logstoreName |string |Yes |The Logstore name, which must be unique in the same project.|

**Request header**

The GetLogstore API does not have a special request header.  For more information about the public request headers of Log Service APIs, see [Public request header](reseller.en-US/API Reference/Public request header.md).

**Response header **

The GetLogstore API does not have a special response header.  For more information about   the public response headers of Log Service APIs, see [Public response header](reseller.en-US/API Reference/Public response header.md).

**Response element **

The returned HTTP status code is  200.

```
{
    "logstoreName": <logstoreName>,
    "ttl": <ttl>,
    "shardCount": <shardCount>,
    "createTime": <createTime>,
    "lastModifyTime": <lastModifyTime>
}
```

**Error code **

Besides the common error codes of Log Service APIs, the GetLogstore API may return the following special error codes. [common error codes](reseller.en-US/API Reference/Common error codes.md) of Log Service APIs, the GetLogstore API may return the following special error codes.

|HTTP status code |Error code |Error message|
|:----------------|:----------|:------------|
|404 |ProjectNotExist |Project \{ProjectName\} does not exist|
|404 |LogstoreNotExist |logstore \{logstoreName\} does not exist|
|500 |InternalServerError|Specified Server Error Message|

## Example {#section_e5p_d4t_12b .section}

**Request example**

```
GET /logstores/test-logstore HTTP/1.1
Header :
{
x-log-apiversion=0.6.0,
Authorization=LOG 94to3z418yupi6ikawqqd370:6ga/Cvj51rFatX/DtTkcQB/CALk=,
Host=ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com,
Date=Wed, 11 Nov 2015 07:53:29 GMT,
Content-Length=0,
x-log-signaturemethod=hmac-sha1,
User-Agent=sls-java-sdk-v-0.6.0,
Content-Type=application/json
}
```

**Response example**

```
HTTP/1.1 200 OK
Header :
{
Date=Wed, 11 Nov 2015 07:53:30 GMT,
Content-Length=107,
x-log-requestid=5642F3FA99248C817B01352D,
Connection=close,
Content-Type=application/json,
Server=nginx/1.6.1
}
Body :
{
    "logstoreName": test-logstore,
    "ttl": 1,
    "shardCount": 2,
    "createTime": 1447833064,
    "lastModifyTime": 1447833064
}
```

