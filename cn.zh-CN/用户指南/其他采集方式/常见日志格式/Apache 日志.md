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

1.  在Logstore列表界面单击**数据接入向导**图标，进入数据接入向导。
2.  选择数据类型。

    选择**APACHE访问日志**。

3.  配置数据源。

    1.  填写**配置名称**和**日志路径**。
    2.  选择**日志格式**。
    3.  填写**APACHE配置字段**。

        请填写标准Apache配置文件日志配置部分，通常以LogFormat开头。

        **说明：** 如您的**日志格式**为**common**或**combined**格式，此处会自动匹配对应格式的配置字段，请确认是否和本地Apache配置文件中定义的格式一致。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/15621183489380_zh-CN.png)

    4.  确认**APACHE键名称**。

        日志服务会自动读取您的Apache键。请在当前页面确认Apache键名称。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/15621183489381_zh-CN.png)

    5.  选择是否**丢弃解析失败日志**。

        请选择解析失败的日志是否上传到日志服务。

        开启后，解析失败的日志不上传到日志服务；关闭后，日志解析失败时上传原始日志，其中Key为`__raw_log__`、Value为日志内容。

    6.  （可选）配置**高级选项**。

        |配置项|详情|
        |:--|:-|
        |上传原始日志|请选择是否需要上传原始日志。开启该功能后，原始日志内容会作为\_\_raw\_\_字段与解析过的日志一并上传。|
        |Topic生成方式|         -    **空-不生成Topic**：默认选项，表示设置Topic为空字符串，在查询日志时不需要输入Topic即可查询。
        -    **机器组Topic属性**：设置Topic生成方式为机器组Topic属性，可以用于明确区分不同前端服务器产生的日志数据。
        -    **文件路径正则**：选择此项之后，您需要填写下方的**自定义正则**，用正则式从路径里提取一部分内容作为Topic。可以用于区分具体用户或实例产生的日志数据。
 |
        |自定义正则|如您选择了**文件路径正则**方式生成Topic，需要在此处填写您的自定义正则式。|
        |日志文件编码|         -   utf8：指定使用UTF-8编码。
        -   gbk：指定使用GBK编码。
 |
        |最大监控目录深度|指定从日志源采集日志时，监控目录的最大深度，即最多监控几层日志。最大目录监控深度范围0-1000，0代表只监控本层目录。|
        |超时属性|如果一个日志文件在指定时间内没有任何更新，则认为该文件已超时。您可以对**超时属性**指定以下配置。         -   永不超时：指定持续监控所有日志文件，永不超时。
        -   30分钟超时：如日志文件在30分钟内没有更新，则认为已超时，并不再监控该文件。
 |
        |过滤器配置|日志只有**完全符合**过滤器中的条件才会被收集。 例如：

        -    **满足条件即收集**：配置`Key:level Regex:WARNING|ERROR`，表示只收集level为WARNING或ERROR类型的日志。
        -    ** [过滤不符合条件的数据](http://www.regular-expressions.info/lookaround.html) **：
            -   配置`Key:level Regex:^(?!.*(INFO|DEBUG)).*`，表示不收集level为INFO或DEBUG类型的日志。
            -   配置`Key:url Regex:.*^(?!.*(healthcheck)).*`，表示不采集url中带有healthcheck的日志，例如key为url，value为`/inner/healthcheck/jiankong.html`的日志将不会被采集。
更多示例可参考[regex-exclude-word](https://stackoverflow.com/questions/2404010/match-everything-except-for-specified-strings)、[regex-exclude-pattern](https://stackoverflow.com/questions/2078915/a-regular-expression-to-exclude-a-word-string)。

 |

4.  单击**下一步**。
5.  选择机器组，并单击**应用到机器组**。

    若您未创建机器组，请先单击**+创建机器组**，创建一个机器组。

    将Logtail配置应用到机器组之后，日志服务开始按照配置收集Apache日志。您可以在数据接入向导的后续步骤中配置索引、投递日志。


