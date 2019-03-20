# Public response header {#reference_qtk_4tq_12b .reference}

Log Service APIs are RESTful APIs based on the HTTP protocol.  All the Log Service API responses provide a set of public response headers. See the following detailed definitions.

|Header name|Type|Description|
|:----------|:---|:----------|
|Content-Length|numeric value|The length of the HTTP response content defined in RFC 2616.|
|Content-MD5|string|The MD5 value of the HTTP response content defined in RFC 2616,  which is an uppercase string generated after the body undergoes MD5 computing.|
|Content-Type|String|The type of the HTTP response content defined in RFC 2616.  Currently, Log Service supports two response types:  application/json and application/x-protobuf.|
|Date|String|The time when the request is returned. Currently, parameters only support the RFC 822 format, and the GMT standard time is used.  The formatted string is as follows: %a, %d %b %Y %H:%M:%S GMT \(for example, Mon, 3 Jan 2010 08:33:47 GMT\).|
|x-log-requestid|The unique ID generated in Log Service that marks this request. |This response header is not related to specific applications,  but is mainly used to track and investigate problems.  To troubleshoot the API request that fails,  provide this ID to the Log Service team.|

