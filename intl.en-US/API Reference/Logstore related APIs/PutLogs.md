# PutLogs {#reference_a5t_kxr_zdb .reference}

You can call this operation to write log data to a specific Logstore. Currently, you can write only log groups in [Protocol Buffers \(Protobuf\)](reseller.en-US/API Reference/Common resources/Data encoding method.md) format.

**Note:** Log data is packaged in units of log groups.

You can write log data in either of the following modes:

-   LoadBalance mode: Log Service automatically writes log data to all writable shards in a Logstore in load balancing mode. This mode is highly available for writing data, with the SLA at 99.95%. It is applicable to scenarios in which data is written and consumed irrelevant to shards, for example, scenarios that do not preserve the order of logs.
-   KeyHash mode: A key is specified for writing data. Log Service automatically writes log data to the shard whose MD5 value range contains the key value. For example, Log Service can hash a producer such as an instance to a fixed shard based on the name to ensure that data is written and consumed in strict order in this shard. When shards are merged or split, a key is included only in the MD5 value range of a shard at a time. For more information, see [Shard](../../../../reseller.en-US/Product Introduction/Basic concepts/Shard.md).

## Request syntax {#section_j5v_14t_12b .section}

**LoadBalance mode** 

```
POST /logstores/<logstorename>/shards/lb HTTP/1.1
Authorization: <AuthorizationString>
Content-Type: application/x-protobuf
Content-Length: <Content Length>
Content-MD5: <Content MD5>
Date: <GMT Date>
Host: <Project Endpoint>
x-log-apiversion: 0.6.0
x-log-bodyrawsize: <BodyRawSize>
x-log-compresstype: lz4
x-log-signaturemethod: hmac-sha1
<Compressed data of logs in Protobuf format>
```

**KeyHash mode**

Log Service adds the x-log-hashkey field to the request header to specify the shard whose MD5 value range contains the key value. This field is optional. If you do not specify this field, Log Service automatically writes log data in LoadBalance mode.

```
POST /logstores/<logstorename>/shards/lb HTTP/1.1
Authorization: <AuthorizationString>
Content-Type: application/x-protobuf
Content-Length: <Content Length>
Content-MD5: <Content MD5>
Date: <GMT Date>
Host: <Project Endpoint>
x-log-apiversion: 0.6.0
x-log-bodyrawsize: <BodyRawSize>
x-log-compresstype: lz4
x-log-hashkey : 14d2f850ad6ea48e46e4547edbbb27e0
x-log-signaturemethod: hmac-sha1
<Compressed data of logs in Protobuf format>
```

## Request parameters {#section_mx5_b4t_12b .section}

|Parameter|Type|Required|Description|
|:--------|:---|:-------|:----------|
|logstorename|String|Yes|The name of the Logstore where logs are to be written.|

 **Request header fields** 

In KeyHash mode, the x-log-hashkey field must be added to the request header. For more information, see the preceding request syntax.

For more information about the common request header fields of Log Service operations, see [Common request header fields](reseller.en-US/API Reference/Public request header.md).

 **Response header fields** 

This operation does not return special response header fields. For more information about the common response header fields of Log Service operations, see [Common response header fields](reseller.en-US/API Reference/Public response header.md).

 **Response parameters** 

After a request is sent, no response is returned.

 **Detailed description** 

-   You can use the PutLogs operation to write up to 4,096 logs, with the maximum size of 3 MB. The value of each log in a log group cannot exceed 1 MB in size. A request fails and no log data can be written if any of the preceding conditions are not met.
-   Log Service checks the format of log data written by the PutLogs operation. For more information about log formats, see [Basic concepts](../../../../reseller.en-US/Product Introduction/Basic concepts/Overview.md). A request fails and no log data can be written if any log does not conform to the format specifications.

 **Error codes** 

In addition to the [common errors](reseller.en-US/API Reference/Common error codes.md) of Log Service operations, this operation also returns some specific errors, as listed in the following table.

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|:----------|
|400|PostBodyInvalid|Protobuffer content cannot be parsed.|The error message returned because the logs in Protobuf format cannot be parsed.|
|400|InvalidTimestamp|Invalid timestamps are in logs.|The error message returned because the logs contain invalid timestamps.|
|400|InvalidEncoding|Non-UTF8 characters are in logs.|The error message returned because the logs contain non-UTF-8-encoded characters.|
|400|InvalidKey|Invalid keys are in logs.|The error message returned because the logs contain invalid keys.|
|400|PostBodyTooLarge|Logs must be less than or equal to 3 MB and 4096 entries.|The error message returned because the number of logs exceeds 4,096 or the size of logs is greater than 3 MB.|
|400|PostBodyUncompressError|Failed to decompress logs.|The error message returned because the logs fail to be decompressed.|
|499|PostBodyInvalid|The post data time is out of range|The error message returned because the log timestamp is beyond the valid interval of \[-7 × 24 hours, +15 minutes\].|
|404|LogStoreNotExist|logstore \{Name\} does not exist.|The error message returned because the specified Logstore does not exist.|

**Note:** The \{name\} variable in the preceding error message is replaced with a specific Logstore name.

## Examples {#section_e5p_d4t_12b .section}

**Sample request** 

```
POST /logstores/sls-test-logstore
{
    "Content-Length": 118,
    "Content-Type":"application/x-protobuf",
    "x-log-bodyrawsize":1356,
    "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com",
    "Content-MD5":"6554BD042149C844761C2C094A8FECCE",
    "Date":"Thu, 12 Nov 2015 06:54:26 GMT",
    "x-log-apiversion": "0.6.0",
    "x-log-compresstype":"lz4"
    "x-log-signaturemethod": "hmac-sha1",
    "Authorization":"LOG <yourAccessKeyId>:<yourSignature>"
}
<Binary data obtained after logs in Protobuf format are compressed by using the LZ4 algorithm>
```

 **Sample success response** 

```
Header
{   
    "date": "Thu, 12 Nov 2015 06:53:03 GMT",
    "connection": "close",
    "x-log-requestid": "5644160399248C060600D216",
    "content-length": "0",
    "server": "nginx/1.6.1"
}
```

