# Apache 日志 {#task_1546614 .task}

日志服务支持通过控制台数据接入向导一站式配置采集Apache日志与设置索引。

## 日志格式 {#section_tbi_gvk_xyd .section}

Apache日志配置文件中默认定义了两种打印格式，分别为combined格式和common格式。您也可以添加自定义配置，按需求配置您的日志的打印格式。

-   combined格式：

    ``` {#codeblock_exx_7z5_7sl}
    LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" combined
    ```

-   common格式：

    ``` {#codeblock_hfk_4oc_kt8}
    LogFormat "%h %l %u %t \"%r\" %>s %b" 
    ```

-   自定义格式：

    ``` {#codeblock_i8v_3v9_ws2}
    LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\" %D %f %k %p %q %R %T %I %O" customized
    ```


Apache日志配置文件中需要同时指定当前日志的打印格式、日志文件路径及名称。例如以下声明表示日志打印时使用配置文件中定义的combined格式，且日志路径和名称为/var/log/apache2/access\_log。

``` {#codeblock_5uj_i3b_gma}
CustomLog "/var/log/apache2/access_log" combined
```

## 字段说明 {#section_1u3_x4z_dl2 .section}

|字段格式|键名称|含义|
|:---|:--|:-|
|%a|client\_addr|请求报文中的客户端IP地址。|
|%A|local\_addr|本地私有IP地址。|
|%b|response\_size\_bytes|响应字节大小，空值时可能为“-”。|
|%B|response\_bytes|响应字节大小，空值时为0。|
|%D|request\_time\_msec|请求时间，单位为毫秒。|
|%h|remote\_addr|远端主机名。|
|%H|request\_protocol\_supple|请求协议。|
|%l|remote\_ident|客户端日志名称，来自identd。|
|%m|request\_method\_supple|请求方法。|
|%p|remote\_port|服务器端口号。|
|%P|child\_process|子进程ID。|
|%q|request\_query|查询字符串，如果不存在则为空字符串。|
|“%r”|request|请求内容，包括方法名、地址和http协议。|
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
|“%\{User-Agent\}i”|http\_user\_agent|客户端信息。|
|“%\{Rererer\}i”|http\_referer|来源页。|

## 日志样例 {#section_z65_01e_4wf .section}

``` {#codeblock_ley_eb8_m2n}
192.168.1.2 - - [02/Feb/2016:17:44:13 +0800] "GET /favicon.ico HTTP/1.1" 404 209 "http://localhost/x1.html" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/48.0.2564.97 Safari/537.36" 
```

## 采集步骤 {#section_hap_kex_quf .section}

这里主要介绍Apache日志采集的Logtail配置部分，关于其他采集步骤的详细说明请参见[文本日志采集流程](cn.zh-CN/数据采集/Logtail采集/文本日志/文本日志采集流程.md#)。

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。
2.  选择数据源类型。 通过APACHE配置模式采集请选择**Apache-文本日志**。
3.  选择日志空间。 请选择Project和Logstore，您也可以直接单击**立即创建**新建Project和Logstore。具体步骤请参见[准备流程](../../../../cn.zh-CN/准备工作/准备流程.md#)。

    如果您是通过日志库下的**数据接入**后的加号进入采集配置流程，系统会直接跳过该步骤。

4.  创建机器组。 在创建机器组之前，您需要首先确认已经安装了Logtail。

    -   ECS机器： Linux系统勾选实例后单击**安装**进行一键式安装。Windows系统不支持一键式安装，请参见[安装Logtail（Windows系统）](../../../../cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Windows系统）.md#)手动安装。
    -   自建（非ECS）机器：请根据界面提示进行安装。或者请参见[安装Logtail（Linux系统）](cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Linux系统）.md#)或[安装Logtail（Windows系统）](cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Windows系统）.md#)进行安装。
    安装完Logtail后单击**确认安装完毕**创建机器组，具体请参见[简介](../../../../cn.zh-CN/数据采集/Logtail采集/机器组/简介.md#)。如果您之前已经创建好机器组 ，请直接单击**使用现有机器组**。

5.  机器组配置。 选择一个机器组，将该机器组从**源机器组**移动到**应用机器组**。

    ![机器组配置](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13062/156878576760275_zh-CN.png)

6.  Logtail配置。 Apache日志采集配置的Logtail配置项说明如下。

    |配置项|详情|
    |---|--|
    |配置名称|配置名称只能包含小写字母、数字、连字符（-）和下划线（\_），且必须以小写字母和数字开头和结尾，长度为3~128字符。 **说明：** 配置名称设置后不可修改。

 |
    |日志路径|指定日志的目录和文件名。     -   日志文件名支持完整文件名和通配符两种模式，文件名规则请参见[Wildcard matching](http://man7.org/linux/man-pages/man7/glob.7.html)。
    -   日志文件查找模式为多层目录匹配，即指定文件夹下所有符合文件名模式的文件都会被监控到，包含所有层次的目录。

        -   例如`/apsara/nuwa/ … /*.log`表示`/apsara/nuwa`目录中（包含该目录的递归子目录）后缀名为`.log`的文件。
        -   例如`/var/logs/app_* … /*.log*`表示`/var/logs`目录下所有符合`app_*`模式的目录中（包含该目录的递归子目录）文件名包含`.log`的文件。
**说明：** 

        -   一个文件只能被一个配置收集。
        -   目录通配符只支持 `*`和`?` 两种。
 |
    |是否为Docker文件|如果是Docker容器内部文件，可以直接配置内部路径与容器Tag，Logtail会自动监测容器创建和销毁，并根据Tag进行过滤采集指定容器的日志。关于容器文本日志采集请参见[容器文本日志](cn.zh-CN/数据采集/Logtail采集/容器日志采集/容器文本日志.md#)。|
    |模式|前面步骤我们选择的数据源是**Apache-文本日志**，所以这里默认是**APACHE配置模式**。|
    |日志格式|请按照您的Apache日志配置文件中声明的格式选择日志格式。为了便于日志数据的查询分析，日志服务推荐您使用自定义的Apache日志格式。|
    |APACHE配置字段|请填写标准Apache配置文件日志配置部分，通常以LogFormat开头。 **说明：** 如您的**日志格式**为**common**或**combined**格式，此处会自动匹配对应格式的配置字段，请确认是否和本地Apache配置文件中定义的格式一致。

 |
    |APACHE键名称|日志服务会自动读取您的Apache键。请在当前页面确认Apache键名称。|
    |丢弃解析失败日志|请选择解析失败的日志是否上传到日志服务。     -   开启后，解析失败的日志不会上传到日志服务。
    -   关闭后，日志解析失败时上传原始日志。
 |
    |最大监控目录深度|指定从日志源采集日志时，监控目录的最大深度，即最多监控几层日志。最大目录监控深度范围0-1000，0代表只监控本层目录。|

    ![apache采集配置](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13040/156878576760715_zh-CN.png)

7.  配置**高级选项**，设置完成后，单击**下一步**。 请根据您的需求选择高级配置。如没有特殊需求，建议保持默认配置。

    |配置项|详情|
    |:--|:-|
    |上传原始日志|请选择是否需要上传原始日志。开启该功能后，原始日志内容会作为\_\_raw\_\_字段与解析过的日志一并上传。|
    |Topic生成方式|     -   **空-不生成Topic**：默认选项，表示设置Topic为空字符串，在查询日志时不需要输入Topic即可查询。
    -   **机器组Topic属性**：设置Topic生成方式为机器组Topic属性，可以用于明确区分不同前端服务器产生的日志数据。
    -   **文件路径正则**：选择此项之后，您需要填写下方的**自定义正则**，用正则式从路径里提取一部分内容作为Topic。可以用于区分具体用户或实例产生的日志数据。
 |
    |自定义正则|如您选择了**文件路径正则**方式生成Topic，需要在此处填写您的自定义正则式。|
    |日志文件编码|     -   utf8：指定使用UTF-8编码。
    -   gbk：指定使用GBK编码。
 |
    |时区属性|设置采集日志时，日志时间的时区属性。     -   机器时区：默认为机器所在时区。
    -   自定义时区：手动选择时区。
 |
    |超时属性|如果一个日志文件在指定时间内没有任何更新，则认为该文件已超时。您可以对**超时属性**指定以下配置。     -   永不超时：指定持续监控所有日志文件，永不超时。
    -   30分钟超时：如日志文件在30分钟内没有更新，则认为已超时，并不再监控该文件。
 |
    |过滤器配置|日志只有**完全符合**过滤器中的条件才会被收集。 例如：

    -   **满足条件即收集**：配置`Key:level Regex:WARNING|ERROR`，表示只收集level为WARNING或ERROR类型的日志。
    -   [过滤不符合条件的数据](http://www.regular-expressions.info/lookaround.html)：
        -   配置`Key:level Regex:^(?!.*(INFO|DEBUG)).*`，表示不收集level为INFO或DEBUG类型的日志。
        -   配置`Key:url Regex:.*^(?!.*(healthcheck)).*`，表示不采集url中带有healthcheck的日志，例如key为url，value为`/inner/healthcheck/jiankong.html`的日志将不会被采集。
更多示例可参见[regex-exclude-word](https://stackoverflow.com/questions/2404010/match-everything-except-for-specified-strings)、[regex-exclude-pattern](https://stackoverflow.com/questions/2078915/a-regular-expression-to-exclude-a-word-string)。

 |

8.  查询分析配置。 默认已经设置好索引，您也可以根据业务需求，重新设置索引。具体请参见[开启并配置索引](../../../../cn.zh-CN/查询与分析/开启并配置索引.md#)。

配置完成后您就可以开始采集Apache日志。

