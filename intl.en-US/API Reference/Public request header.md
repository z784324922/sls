# Public request header {#reference_ehh_ntq_12b .reference}

Log Service APIs are RESTful APIs based on the HTTP protocol, which support a set of public request headers that can be used in all API requests \(unless stated otherwise, each Log Service API request must provide these public request headers\).  See the following detailed definitions.

|Header name|Type|Description|
|:----------|:---|:----------|
|Accept|String|The type that the client expects Log Service to return.  Currently, application/json and application/x-protobuf are supported.  This field is optional and valid only for GET requests. The specific value is subject to the definition of each API.|
|Accept-Encoding|String|The compression algorithm that the client expects Log Service to return.  Currently, lz4, deflate, and null \(not compressed\) are supported.  This field is optional and valid only for GET requests. The specific value is subject to the definition of each API.|
|Authorization|String|The signature content. For more information, see [Request signature](reseller.en-US/API Reference/Request signature.md).|
|Content-Length|numeric value|The length of the HTTP request body defined in RFC 2616.  If the request has no body, this request header is not required.|
|Content-MD5|String| The string generated after the request body undergoes MD5 computing, and the computing result is in uppercase.  If the request has no body, this request header is not required.|
|Content-Type|String|The type of the HTTP request body defined in RFC 2616.  Currently, Log Service API requests only support application/x-protobuf.  If the request has no body, this request header is not required.  The specific value is subject to the definition of each API.|
|date|String|The time when the request is sent. Currently, parameters only support the RFC 822 format, and the GMT standard time is used.  The formatted string is as follows: %a, %d %b %Y  %H:%M:%S GMT \(for example, Mon, 3 Jan 2010 08:33:47 GMT\).|
|Host|String|The complete host name of the HTTP request, which does not include protocol headers such as `http://`. For example, `big-game.cn-hangzhou.sls.aliyuncs.com`.|
|x-log-apiversion|String| The API version. The current version is 0.6.0.|
|x-log-bodyrawsize|numeric value|The initial size of the request body. If the request has no body, the value is 0. If the body is compressed data, the value is the size of the raw data before the compression.  The value range for this field is 0~3x1024x1024. This field is optional and only required when the body is compressed.|
|x-log-compresstype|String|The compression type of the API request body. Currently, lz4 and deflate are supported \(RFC 1951 uses the zlib format. For more information, see RFC 1950\). If the body is not compressed, this request header is not required.|
|x-log-date|String|The time when the request is sent. The format is the same as that of the Date header. This request header is optional.  If a request contains this public request header, this value will replace the value of the standard header Date for request authentication in Log Service. This field does not participate in the signature. Whether or not the x-log-date header exists, you must provide the HTTP standard header Date.|
|x-log-signaturemethod|String|The signature computing method. Currently, only `hmac-sha1` is supported.|
|x-acs-security-token|String|Use the Security Token Service \(STS\) temporary identity to send data. This request header is required only when the STS temporary identity is used.|

-   The maximum difference between the time expressed in the Date header of a request and the time the server receives the request is 15 minutes. If this difference exceeds 15 minutes, the server will reject this request. If the request contains an x-log-date header, the time difference is computed based on the value of the x-log-date header.
-   If the compression algorithm is specified in x-log-compresstype of the request, the raw data must be compressed and then put into the HTTP body.
-   The maximum difference between the time expressed in the Date header of a request and the time the server receives the request is 15 minutes.  If this difference exceeds 15 minutes, the server will reject this request. If the request contains an x-log-date header, the time difference is computed based on the value of the x-log-date header.

