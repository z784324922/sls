# Apache 日志 {#concept_zcn_wq5_vdb .concept}

apache 日志格式和目录通常在配置文件 /etc/apache2/httpd.conf中。

## 日志格式 {#section_k1d_5sb_ry .section}

Apache日志配置文件中定义了两种打印格式，分别为combined格式和common格式。

-   combined格式：

    ```
    LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" combined
    ```

-   common格式：

    ```
    LogFormat "%h %l %u %t \"%r\" %>s %b" 
    ```


声明使用了 combined 这种日志格式和写入的文件名。

```
CustomLog "/var/log/apache2/access_log" combined
```

## 字段说明 {#section_mm1_gl4_y1b .section}

|字段格式|含义|
|----|:-|
|%a|remote\_ip|
|%A|local\_ip|
|%B|size|
|%b|size|
|%D|time\_taken\_ms|
|%h|remote\_host|
|%H|protocol|
|%l|ident|
|%m|method|
|%p|port|
|%P|pid|
|“%q”|url\_query|
|“%r”|request|
|%s|status|
|%\>s|status|
|%t|time|
|%T|time\_taken|
|%u|remote\_user|
|%U|url\_stem|
|%v|server\_name|
|%V|canonical\_name|
|%I|bytes\_received|
|%O|bytes\_sent|
|“%\{User-Agent\}i”|user\_agent|
|“%\{Referer\}i”|referer|

## 日志样例 {#section_ng3_hl4_y1b .section}

```
192.168.1.2 - - [02/Feb/2016:17:44:13 +0800] "GET /favicon.ico HTTP/1.1" 404 209 "http://localhost/x1.html" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/48.0.2564.97 Safari/537.36"
```

## 配置Logtail收集Apache日志 {#section_al1_gl4_y1b .section}

1.  在Logstore列表界面单击**数据接入向导**图表，进入数据接入向导。
2.  选择数据类型。

    选择**文本文件**并单击**下一步**。

3.  配置数据源。

    1.  填写配置名称、日志路径，并选择日志收集模式为**完整正则模式**。
    2.  输入日志样例并开启**自动提取字段**。
    3.  单击 **手动输入正则表达式**，并调整正则表达式。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13040/2614_zh-CN.png)

        日志服务支持对日志样例划词自动解析，即对您在划词时选取的字段自动生成正则表达式。但鉴于实际的日志数据格式可能会有细微变动，您需要在根据实际情况对自动生成的正则表达式做出调整，使其符合收集过程中所有可能出现的日志格式。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13040/2615_zh-CN.png)

        正则表达式修改完成后，单击**验证** 。如果正则式没有错误，会出现提取的结果，如果有错误请再次调整正则式。

    4.  为日志内容抽取结果填写对应的Key。

        分别为提取结果取一个有意义的字段名称，比如时间字段的命名为 time。开启 **使用系统时间**，然后单击 **下一步**。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13040/2616_zh-CN.png)

    Logtail配置完成后，将此配置应用到机器组即可开始规范收集Apache日志。


