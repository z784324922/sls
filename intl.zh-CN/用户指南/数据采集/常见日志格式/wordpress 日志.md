# wordpress 日志 {#reference_emt_d55_vdb .reference}

## WordPress 默认日志格式 {#section_ykq_yxb_ry .section}

原始日志样例：

```
172.64.0.2 - - [07/Jan/2016:21:06:39 +0800] "GET /wp-admin/js/password-strength-meter.min.js?ver=4.4 HTTP/1.0" 200 776 "http://wordpress.c4a1a0aecdb1943169555231dcc4adfb7.cn-hangzhou.alicontainer.com/wp-admin/install.php" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/47.0.2526.106 Safari/537.36"
```

多行日志起始匹配（使用 IP 信息表示一行开头）：

```
\d+\.\d+\.\d+\.\d+\s-\s.*
```

提取日志信息的正则表达式：

```
(\S+) - - \[([^\]]*)] "(\S+) ([^"]+)" (\S+) (\S+) "([^"]+)" "([^"]+)"
```

时间转换格式：

```
%d/%b/%Y:%H:%M:%S
```

样例日志提取结果：

|Key|Value|
|---|-----|
|ip|10.10.10.1|
|time|07/Jan/2016:21:06:39 +0800|
|method|GET|
|url|/wp-admin/js/password-strength-meter.min.js?ver=4.4 HTTP/1.0|
|status|200|
|length|776|
|ref|http://wordpress.c4a1a0aecdb1943169555231dcc4adfb7.cn-hangzhou.alicontainer.com/wp-admin/install.php|
|user-agent|Mozilla/5.0 \(Macintosh; Intel Mac OS X 10\_10\_5\) AppleWebKit/537.36 \(KHTML, like Gecko\) Chrome/47.0.2526.106 Safari/537.36|

