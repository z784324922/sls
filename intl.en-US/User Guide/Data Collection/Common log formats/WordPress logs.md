# WordPress logs {#reference_emt_d55_vdb .reference}

## Default WordPress log format {#section_ykq_yxb_ry .section}

Raw sample log:

```
172.64.0.2 - - [07/Jan/2016:21:06:39 +0800] "GET /wp-admin/js/password-strength-meter.min.js? ver=4.4 HTTP/1.0" 200 776 "http://wordpress.c4a1a0aecdb1943169555231dcc4adfb7.cn-hangzhou.alicontainer.com/wp-admin/install.php" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/47.0.2526.106 Safari/537.36"
```

tching of the beginning of a line in multiline logs \(use IP to indicate the beginning of a line\):

```
\d+\.\d+\.\d+\.\d+\s-\s. *
```

The regular expression used to extract log information:

```
(\S+) - - \[([^\]]*)] "(\S+) ([^"]+)" (\S+) (\S+) "([^"]+)" "([^"]+)"
```

Time conversion format:

```
%d/%b/%Y:%H:%M:%S
```

Extraction results of the log sample:

|Key|Value|
|:--|:----|
|ip|10.10.10.1|
|time|07/Jan/2016:21:06:39 +0800|
|method|GET|
|url|/wp-admin/js/password-strength-meter.min.js? ver=4.4 HTTP/1.0|
|status|200|
|length|776|
|ref|http://wordpress.c4a1a0aecdb1943169555231dcc4adfb7.cn-hangzhou.alicontainer.com/wp-admin/install.php|
|user-agent|Mozilla/5.0 \(Macintosh; Intel Mac OS X 10\_10\_5\) AppleWebKit/537.36 \(KHTML, like Gecko\) Chrome/47.0.2526.106 Safari/537.36|

