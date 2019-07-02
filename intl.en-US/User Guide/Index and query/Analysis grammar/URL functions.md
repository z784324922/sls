# URL functions {#concept_xjz_mlq_zdb .concept}

URL functions allow you to extract fields from standard URLs. A standard URL is as follows:

```
[protocol:][//host[:port]][path][? query][#fragment]
```

## Common URL functions {#section_t2j_xwk_5cb .section}

|Function|Description|Example|
|Input|Output result|
|:-------|:----------|:------|
|:----|:------------|
|`url_extract_fragment(url)`|Extracts the fragment identifier from a URL and returns the result of the varchar type.|`*| select url_extract_fragment('https://sls.console.aliyun.com/#/project/dashboard-demo/categoryList')`|`/project/dashboard-demo/categoryList`|
|`url_extract_host(url)`|Extracts the host from a URL and returns the result of the varchar type.|`*|select url_extract_host('http://www.aliyun.com/product/sls')`|`www.aliyun.com`|
|`url_extract_parameter(url, name)`|Extracts the value of the name parameter in the query from a URL and returns the result of the varchar type.|`*|select url_extract_parameter('http://www.aliyun.com/product/sls?userid=testuser','userid')`|`testuser`|
|`url_extract_path(url)`|Extracts the path from a URL and returns the result of the varchar type.|`*|select url_extract_path('http://www.aliyun.com/product/sls?userid=testuser')`|`/product/sls`|
|`url_extract_port(url)`|Extracts the port number from a URL and returns the result of the bigint type.|`*|select url_extract_port('http://www.aliyun.com:80/product/sls?userid=testuser')`|`80`|
|`url_extract_protocol(url)`|Extracts the protocol from a URL and returns the result of the varchar type.|`*|select url_extract_protocol('http://www.aliyun.com:80/product/sls?userid=testuser')`|`http`|
|`url_extract_query(url)`|Extracts the query string from a URL and returns the result of the varchar type.|`*|select url_extract_query('http://www.aliyun.com:80/product/sls?userid=testuser')`|`userid=testuser`|
|`url_encode(value)`|Encodes a URL.|`*|select url_encode('http://www.aliyun.com:80/product/sls?userid=testuser')`|`http%3a%2f%2fwww.aliyun.com%3a80%2fproduct%2fsls%3fuserid%3dtestuser`|
|`url_decode(value)`|Decodes a URL.|`*|select url_decode('http%3a%2f%2fwww.aliyun.com%3a80%2fproduct%2fsls%3fuserid%3dtestuser')`|`http://www.aliyun.com:80/product/sls?userid=testuser`|

