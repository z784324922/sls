# PutLogs {#reference_a5t_kxr_zdb .reference}

Write log data to a specified Logstore in the following modes. Currently, only log groups in [Protocol Buffer \(PB\)](reseller.en-US/API Reference/Common resources/Data encoding method.md) format can be written. There are two modes in writing:

-   Load balancing mode: Automatically write logs to all writable shards in a Logstore in the load balancing mode. This mode is highly available for writing \(SLA: 99.95%\), applicable to scenarios in which data writing and consumption are independent of shards , for example, scenarios that do not preserve the order.
-   KeyHash mode: A key is required when writing data. Log Service automatically writes data to the shard that meets the key range. For example, hash a producer \(for example, an instance\) to a fixed shard based on the name to make sure the data writing and consumption in this shard are strictly ordered \(when merging or splitting shards, a key can only appear in one shard at a time point\). For more information, see [Shard](../../../../reseller.en-US/Product Introduction/Basic concepts/Shard.md).

## Request syntax {#section_j5v_14t_12b .section}

**Load balancing mode** 

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
<Compressed log data in PB format>
```

**KeyHash mode**

Add x-log-hashkey in the header to determine which shard range the key belongs to. This parameter is optional. If left blank, Log Service automatically switches to the load balancing

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
<Compressed log data in PB format>
```

## Request parameters {#section_mx5_b4t_12b .section}

|Parameter name|Type|Required|Description|
|:-------------|:---|:-------|:----------|
|logstorename|String|Yes|The name of the Logstore where logs are to be written.|

 **Request header** 

In the KeyHash mode, add the x-log-hashkey request header \(see the preceding example\).

For more information about the public request headers of Log Service APIs, see [Public request header](reseller.en-US/API Reference/Public request header.md).

 **Response element** 

The PutLogs API does not have a special response header. For more information about the public response headers of Log Service APIs, see [Public response header](reseller.en-US/API Reference/Public response header.md).

 **Response element** 

No response element after the successful request.

 **Detailed description** 

-   You can use the PutLogs API to write at most 4096 logs and the size of logs is at most 3 MB. The size of the value section in each log cannot be larger than 1 MB. The request fails and no logs are successfully written if any of the preceding conditions are not met.
-   Log Service checks the format of the logs written by using the PutLogs API \(for more information about the log formats, see [Core concepts](../../../../reseller.en-US/Product Introduction/Basic concepts/Overview.md)\). The request fails and no logs are successfully written if any log does not conform to the specification.

 **Error code** 

Besides the [common error codes](reseller.en-US/API Reference/Common error codes.md) of Log Service APIs, the PutLogs API may return the following special error codes.

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|:----------|
|400|PostBodyInvalid|Protobuffer content cannot be parsed.|The Protobuffer content cannot be parsed.|
|400|InvalidTimestamp|Invalid timestamps are in logs.|Invalid timestamps are in logs.|
|400|InvalidEncoding|Non-UTF8 characters are in logs.|Non-UTF8 characters are in logs.|
|400|InvalidKey|Invalid keys are in logs.|Invalid keys are in logs.|
|400|PostBodyTooLarge|Logs must be less than or equal to 3 MB and 4096 entries.|The number of logs must be no more than 4096 and the size of logs must be no more than 3 MB.|
|400|PostBodyUncompressError|Failed to decompress logs.|Failed to decompress logs.|
|499|PostBodyInvalid|The post data time is out of range|The log time is out of the valid range \[-7\*24Hour, +15Min\].|
|404|LogStoreNotExist|logstore \{Name\} does not exist.|The Logstore does not exist.|

**Note:** The \{name\} in the preceding error message is replaced by a specific Logstore name.

## Example {#section_e5p_d4t_12b .section}

**Request example:** 

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
<Binary data from logs in PB format compressed with LZ4>
```

 **Response example** 

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

