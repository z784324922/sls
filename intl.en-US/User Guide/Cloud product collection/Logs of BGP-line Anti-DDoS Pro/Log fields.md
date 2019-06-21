# Log fields {#concept_vkt_t3h_1gb .concept}

This topic describes the supported fields of Anti-DDoS Pro log entries.

You can go to the Log Service page to query and analyze collected logs in real time. For more information about log fields, see the following figure.

|Field|Description|Example|
|:----|:----------|:------|
|\_\_topic\_\_|The topic of the log entry. The value of this field is fixed to ddos\_access\_log.|N/A|
|body\_bytes\_sent|The size of the body in the access request, in bytes.|2|
|content\_type|The content type.|application/x-www-form-urlencoded|
|host|The source website.|api.zhihu.com|
|http\_cookie|The request cookie.|k1=v1;k2=v2|
|http\_referer|The request referer. If no referer exists, a hyphen \(`-`\) is displayed.|http://xyz.com|
|http\_user\_agent|The User-Agent of the request.|Dalvik/2.1.0 \(Linux; U; Android 7.0; EDI-AL10 Build/HUAWEIEDISON-AL10\)|
|http\_x\_forwarded\_for|The IP address of the upstream user redirected by proxy.|N/A|
|https|Indicates whether the request is an HTTPS request.-   true: The request is an HTTPS request.
-   false: The request is an HTTP request.

|true|
|matched\_host|The matching source site, which may be a wildcard domain name. If no match is found, a hyphen \(`-`\) is displayed.|\*.zhihu.com|
|real\_client\_ip|The real IP of the visitor. If no real IP is returned, a hyphen \(`-`\) is returned.|1.2.3.4|
|isp\_line|Line information, such as BGP, China Telecom, and China Unicom.|China Telecom|
|remote\_addr|The IP address of the client that initiates the connection request.|1.2.3.4|
|remote\_port|The port number of the client that initiates the connection request.|23713|
|request\_length|The size of the request in bytes.|123|
|request\_method|The HTTP method of the request.|GET|
|request\_time\_msec|The request time in ms.|44|
|request\_uri|The request URI.|/answers/377971214/banner|
|server\_name|The name of the matching host. If no match is found, the value is `default`.|api.abc.com|
|status|The HTTP status code.|200|
|time|The time when the log entry was generated.|2018-05-02T16:03:59+08:00|
|cc\_action|The HTTP flood protection action. Valid values include none, challenge, pass, close, captcha, wait, and login.|close|
|cc\_blocks|Indicates whether HTTP flood attacks are blocked. Valid values:-   1: block
-   Other values: pass

|1|
|cc\_phase|The HTTP flood protection policy. Valid values include seccookie, server\_ip\_blacklist, static\_whitelist, server\_header\_blacklist, server\_cookie\_blacklist, server\_args\_blacklist, and qps\_overmax.|server\_ip\_blacklist|
|ua\_browser|The browser.|ie9|
|ua\_browser\_family|The browser series.|internet explorer|
|ua\_browser\_type|The browser type.|web\_browser|
|ua\_browser\_version|The browser version.|9.0|
|ua\_device\_type|The type of the client device.|computer|
|ua\_os|The operating system of the client.|windows\_7|
|ua\_os\_family|The operating system series of the client.|windows|
|upstream\_addr|The list of origin addresses that are separated with commas \(,\). Each address is in the format of `IP:Port`.|1.2.3.4:443|
|upstream\_ip|The real origin IP address.|1.2.3.4|
|upstream\_response\_time|The response time in seconds for the back-to-origin process.|0.044|
|upstream\_status|The HTTP status of the back-to-origin request.|200|
|user\_id|The user ID of the Alibaba Cloud account.|12345678|
|querystring|The request string.|token=bbcd&abc=123|

