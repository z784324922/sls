# Nginx 日志 {#reference_kv2_sr5_vdb .reference}

nginx 日志格式和目录通常在配置文件 /etc/nginx/nginx.conf中。

## Nginx日志格式 {#section_rbt_stb_ry .section}

配置文件中定义了Nginx日志的打印格式，即main格式：

```
log_format main  '$remote_addr - $remote_user [$time_local] "$request" '
                 '$request_time $request_length '
                 '$status $body_bytes_sent "$http_referer" '
                 '"$http_user_agent"';
```

声明使用了 main 这种日志格式和写入的文件名。

```
access_log /var/logs/nginx/access.log main
```

## 字段说明 {#section_dhb_1c3_dbb .section}

|字段名称|含义|
|:---|:-|
|remoteaddr|表示客户端IP地址。|
|remote\_user|表示客户端用户名称。|
|request|表示请求的URL和HTTP协议。|
|status|表示请求状态。|
|bodybytessent|表示发送给客户端的字节数，不包括响应头的大小；该变量与Apache模块modlogconfig里的bytes\_sent发送给客户端的总字节数相同。|
|connection|表示连接的序列号。|
|connection\_requests|表示当前通过一个连接获得的请求数量。|
|msec|表示日志写入的时间。单位为秒，精度是毫秒。|
|pipe|表示请求是否通过HTTP流水线（pipelined）发送。通过HTTP流水线发送则pipe值为“p”，否则为“.”。|
|httpreferer|表示从哪个页面链接访问过来的。|
|“http\_user\_agent”|表示客户端浏览器相关信息，前后必须加上双引号。|
|requestlength|表示请求的长度。包括请求行，请求头和请求正文。|
|request\_time|表示请求处理时间，单位为秒，精度为毫秒。从读入客户端的第一个字节开始，直到把最后一个字符发送给客户端后进行日志写入为止。|
|\[$time\_local\]|表示通用日志格式下的本地时间，前后必须加上中括号。|

## 日志样例 {#section_g2b_1c3_dbb .section}

```
192.168.1.2 - - [10/Jul/2015:15:51:09 +0800] "GET /ubuntu.iso HTTP/1.0" 0.000 129 404 168 "-" "Wget/1.11.4 Red Hat modified"
```

## 配置Logtail收集Nginx日志 {#section_vkv_15b_ry .section}

1.  在Logstore列表界面单击**数据接入向导**图表，进入数据接入向导。
2.  选择数据类型。

    选择**NGINX访问日志**并单击**下一步**。

3.  配置数据源。

    1.  填写**配置名称**、**日志路径**。
    2.  输入Nginx日志格式。

        请填写标准NGINX配置文件日志配置部分，通常以`log_format`开头。日志服务会自动读取您的Nginx键。

    3.  酌情配置**高级选项**并单击**下一步**。

        高级选项说明请参考[高级选项](intl.zh-CN/用户指南/Logtail 采集/数据源/文本日志.md#table_eq2_ccc_wdb)

    Logtail配置完成后，将此配置应用到机器组即可开始规范收集Nginx日志。


