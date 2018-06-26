# Common error codes {#reference_ypb_rtq_12b .reference}

When an API request error occurs, Log Service returns an error message, including the HTTP status code and the specific error details in the response body.  The error details in the response body are in the following formats:

```
{
"errorCode" : <ErrorCode>,
"errorMessage" : <ErrorMessage>
}
```

Among all the error messages that may be returned by Log Service, some are applicable to most of the APIs, while the others are unique to some APIs. See the following common error codes in the response of multiple APIs.  For the error codes unique to some APIs,  see the descriptions in the corresponding API reference.

|HTTP status code |Error code|Error message|Description|
|:----------------|:---------|:------------|:----------|
|411|MissingContentLength|Content-Length does not exist in http header when it is necessary.|The required Content-Length request header is not provided.|
|415|InvalidContentType|Content-Type \{type\} is unsupported.|The specified Content-Type is not supported.|
|400|MissingContentType|Content-Type does not exist in http header when body is not empty.|The Content-Type header is not specified when the HTTP request body is not empty.|
|400|MissingBodyRawSize|x-log-bodyrawsize does not exist in header when it is necessary.|The required x-log-bodyrawsize request header is not provided in the compression scenario.|
|400|InvalidBodyRawSize|x-log-bodyrawsize is invalid.|The x-log-bodyrawsize value is invalid.|
|400|InvalidCompressType|x-log-compresstype \{type\} is unsupported.|The compression type specified in x-log-compresstype is not supported.|
|400|Missinghost|Host does not exist in http header.|The HTTP standard request header Host is not provided.|
|The HTTP standard request header Host is not provided.|Missingdate|Date does not exist in http header.|The HTTP standard request header Date is not provided.|
|400|InvalidDateFormat|Date \{date\} must follow RFC822.|The Date request header value does not conform to the RFC822 standard.|
|400|MissingAPIVersion|x-log-apiversion does not exist in http header.|The HTTP request header x-log-apiversion is not provided.|
|400|InvalidAPIVersion|x-log-apiversion \{version\} is unsupported.|The value of the HTTP request header x-log-apiversion is not supported.|
|400|MissAccessKeyId|x-log-accesskeyid does not exist in header.|No AccessKey ID is provided in the Authorization header.|
|401|Unauthorized|The AccessKeyId is unauthorized.|The provided AccessKey ID value is unauthorized.|
|400|MissingSignatureMethod|x-log-signaturemethod does not exist in http header.|The HTTP request header x-log-signaturemethod is not provided.|
|400|InvalidSignatureMethod|signature method \{method\} is unsupported.|The signature method specified by the x-log-signaturemethod header is not supported.|
|400|RequestTimeTooSkewed|Request time exceeds server time more than 15 minutes.|The request sent time is more than 15 minutes before or after the current server time.|
|404|ProjectNotExist|Project \{name\} does not exist.|The Log Service project does not exist.|
|401|SignatureNotMatch|Signature \{signature\} is not matched.|The digital signature of the request does not match with that computed in Log Service.|
|403|WriteQuotaExceed|Write quota is exceeded.|The log write quota is exceeded.|
|403|Readquotaexceed|Read quota is exceeded.|The log read quota is exceeded.|
|500|InternalServerError|Internal server error message.|An internal server error.|
|503|ServerBusy|The server is busy, please try again later.|The server is busy. Try again later.|

The \{…\} in the error message indicates the specific error  information.  For example, \{name\} in the ProjectNotExist error message is replaced by a specific project name.

