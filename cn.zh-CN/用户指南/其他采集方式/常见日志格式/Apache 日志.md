# Apache 日志 {#concept_zcn_wq5_vdb .concept}

Apache日志格式和目录通常在配置文件 /etc/apache2/httpd.conf中。

## 日志格式 {#section_k1d_5sb_ry .section}

Apache日志配置文件中默认定义了两种打印格式，分别为combined格式和common格式。您也可以添加自定义配置，按需求配置您的日志的打印格式。

-   combined格式：

    ``` {#codeblock_fh1_jh7_8q4}
    LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" combined
    ```

-   common格式：

    ``` {#codeblock_4io_33y_wso}
    LogFormat "%h %l %u %t \"%r\" %>s %b" 
    ```

-   自定义格式：

    ``` {#codeblock_dhu_w38_mhv}
    LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\" %D %f %k %p %q %R %T %I %O" customized
    ```


Apache日志配置文件中需要同时指定当前日志的打印格式、日志文件路径及名称。例如以下声明表示日志打印时使用配置文件中定义的combined格式，且日志路径和名称为/var/log/apache2/access\_log。

``` {#codeblock_rs8_25t_48h}
CustomLog "/var/log/apache2/access_log" combined
```

## 字段说明 {#section_mm1_gl4_y1b .section}

|字段格式|键名称|含义|
|:---|:--|:-|
|%a|client\_addr|请求报文中的客户端IP地址。|
|%A|local\_addr|本地私有IP地址。|
|%b|response\_size\_bytes|响应字节大小，空值时可能为"-"。|
|%B|response\_bytes|响应字节大小，空值时为0。|
|%D|request\_time\_msec|请求时间，单位为毫秒。|
|%h|remote\_addr|远端主机名。|
|%H|request\_protocol\_supple|请求协议。|
|%l|remote\_ident|客户端日志名称，来自identd。|
|%m|request\_method\_supple|请求方法。|
|%p|remote\_port|服务器端口号。|
|%P|child\_process|子进程ID。|
|%q|request\_query|查询字符串，如果不存在则为空字符串。|
|"%r"|request|请求内容，包括方法名、地址和http协议。|
|%s|status|响应的http状态码。|
|%\>s|status|响应的http状态码的最终结果。|
|%f|filename|请求的文件名。|
|%k|keep\_alive|keep-alive请求数。|
|%R|response\_handler|服务端的处理程序类型。|
|%t|time\_local|服务器时间。|
|%T|request\_time\_sec|请求时间，单位为秒。|
|%u|remote\_user|客户端用户名。|
|%U|request\_uri\_supple|请求的URI路径，不带query。|
|%v|server\_name|服务器名称。|
|%V|server\_name\_canonical|服务器权威规范名称。|
|%I|bytes\_received|服务器接收的字节数，需要启用mod\_logio模块。|
|%O|bytes\_sent|服务器发送的字节数，需要启用mod\_logio模块。|
|"%\{User-Agent\}i"|http\_user\_agent|客户端信息。|
|"%\{Rererer\}i"|http\_referer|来源页。|

## 日志样例 {#section_ng3_hl4_y1b .section}

``` {#codeblock_obx_a5t_eeq}
192.168.1.2 - - [02/Feb/2016:17:44:13 +0800] "GET /favicon.ico HTTP/1.1" 404 209 "http://localhost/x1.html" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/48.0.2564.97 Safari/537.36" 
```

## 配置Logtail收集Apache日志 {#section_al1_gl4_y1b .section}

1.  登录[日志服务控制台](https://sls.console.aliyun.com)，单击Project名称。
2.  单击**接入数据**按钮，并在**接入数据**页面中选择**APACHE文本日志**。
3.  选择日志空间。

    可以选择已有的Logstore，也可以新建Project或Logstore。

4.  创建并配置机器组。

    在创建机器组之前，您需要首先确认已经安装了Logtail。 安装完Logtail后单击**确认安装完毕**创建机器组。如果您之前已经创建好机器组 ，请直接单击**使用现有机器组**。

    选择一个机器组，将该机器组从**源机器组**移动到**应用机器组**。

5.  数据源设置。
    1.  填写**配置名称**和**日志路径**。
    2.  选择**日志格式**。
    3.  填写**APACHE配置字段**。

        请填写标准Apache配置文件日志配置部分，通常以LogFormat开头。

        **说明：** 如您的**日志格式**为**common**或**combined**格式，此处会自动匹配对应格式的配置字段，请确认是否和本地Apache配置文件中定义的格式一致。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/15643903709380_zh-CN.png)

    4.  确认**APACHE键名称**。

        日志服务会自动读取您的Apache键。请在当前页面确认Apache键名称。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/15643903719381_zh-CN.png)

    5.  （可选）配置**高级选项**。
6.  查询分析配置。

    默认已经设置好索引，如果您需要重新设置索引，请在查询分析页面选择**查询分析属性** \> **设置**进行修改。

    确保日志机器组心跳正常的情况下，可以预览采集上来的数据。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/15643903719382_zh-CN.png)


