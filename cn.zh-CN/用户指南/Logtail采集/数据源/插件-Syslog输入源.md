# 插件-Syslog输入源 {#concept_lb4_vg2_z2b .concept}

Logtail支持通过自定义插件采集syslog。

## 简介 {#section_cvc_3h2_z2b .section}

在Linux上，本地的syslog数据可以通过rsyslog等syslog agent转发到指定服务器IP地址和端口。为指定服务器添加Logtail配置之后，Logtail插件会以TCP或UDP协议接收转发过来的syslog数据。并且，插件能够将接收到的数据按照指定的syslog协议进行解析，提取日志中的facility、tag\(program\)、severity、content等字段。syslog协议支持[RFC3164](https://tools.ietf.org/html/rfc3164)和[RFC5424](https://tools.ietf.org/html/rfc5424)。

**说明：** Windows Logtail不支持该插件。

## 实现原理 {#section_tj1_kh2_z2b .section}

通过插件对指定的地址和端口进行监听后，Logtail能够作为syslog服务器采集来自各个数据源的日志，包括通过rsyslog采集的系统日志、 [Nginx](http://nginx.org/en/docs/syslog.html)转发的访问日志或错误日志，以及[Java](https://github.com/CloudBees-community/syslog-java-client)等语言的syslog客户端库转发的日志。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/19015/153786003810990_zh-CN.png)

## 注意事项 {#section_agj_zh2_z2b .section}

-   Linux版 Logtail 0.16.13及以上版本支持该功能。
-   Logtail可同时配置多个syslog插件，比如同时使用TCP和UDP监听127.0.0.1:9999。

## Logtail配置项 {#section_vnx_kh2_z2b .section}

该插件的输入类型为：`service_syslog`。

|配置项|类型|是否必须|说明|
|:--|:-|:---|:-|
|Address|string|否|指定插件监听的协议、地址和端口，Logtail插件会根据配置进行监听并获取日志数据。格式为`[tcp/udp]://[ip]:[port]`，默认为`tcp://127.0.0.1:9999`。**说明：** 

-   Logtail插件配置监听的协议、地址和端口号必须与rsyslog配置文件设置的转发规则相同。
-   如果安装Logtail的服务器有多个IP地址可接收日志，可以将地址配置为0.0.0.0，表示监听服务器的所有IP地址。

|
|ParseProtocol|string|否|指定解析所使用的协议，默认为空，表示不解析。其中：-   rfc3164：指定使用RFC3164协议解析日志。
-   rfc5424：指定使用RFC5424协议解析日志。
-   auto：指定插件根据日志内容自动选择合适的解析协议。

|
|IgnoreParseFailure|boolean|否|指定解析失败后的行为，默认为true。其中：-   true：放弃解析直接填充所返回的content字段。
-   false：会丢弃日志。

|

## 默认字段 {#section_kg1_qh2_z2b .section}

|字段名|字段类型|字段含义|
|:--|:---|:---|
|\_hostname\_|string|主机名，如果日志中未提供则获取当前主机名。|
|\_program\_|string|对应协议中的tag字段。|
|\_priority\_|string|对应协议中的priority字段。|
|\_facility\_|string|对应协议中的facility字段。|
|\_severity\_|string|对应协议中的severity字段。|
|\_unixtimestamp\_|string|日志对应的时间戳。|
|\_content\_|string|日志内容，如果解析失败的话，此字段包含未解析日志的所有内容。|
|\_ip\_|string|当前主机的IP地址。|

## 前提条件 {#section_dch_lj2_z2b .section}

-   已创建Project和Logstore。
-   已创建机器组，并在机器组内服务器上安装了0.16.13及以上版本的Logtail。
-   需要被采集syslog的服务器上已安装了rsyslog。

## 配置Logtail插件采集syslog {#section_agb_232_z2b .section}

1.  为rsyslog添加一条转发规则。

    在需要采集syslog的服务器上修改rsyslog的配置文件/etc/rsyslog.conf，在配置文件的最后添加一行转发规则。添加转发规则后，rsyslog会将syslog转发至指定地址端口。

    -   通过当前服务器采集本机syslog：配置转发地址为127.0.0.1，端口为任意非知名的空闲端口。
    -   通过其他服务器采集本机syslog：配置转发地址为其他服务器的公网IP，端口为任意非知名的空闲端口。
    例如以下配置表示将所有的日志都通过TCP转发至127.0.0.1:9000，配置文件详细说明请参考[官网说明](https://www.rsyslog.com/doc/v8-stable/configuration/index.html)。

    ```
    *.* @@127.0.0.1:9000
    ```

2.  执行以下命令重启rsyslog，使日志转发规则生效。

    ```
    sudo service rsyslog restart
    ```

3.  登录[日志服务控制台](https://sls.console.aliyun.com)，单击Project名称。
4.  在Logstore列表页面单击**数据接入向导**图标。
5.  在**自定义数据**中单击**Logtail自定义插件**。
6.  填写**配置名称**。
7.  填写**插件配置**，并单击**下一步**。

    `inputs`部分为采集配置，是必选项；`processors`部分为处理配置，是可选项。采集配置部分需要按照您的数据源配置对应的采集语句，处理配置部分请参考[插件-处理采集数据](cn.zh-CN/用户指南/Logtail采集/数据源/插件-处理采集数据.md)配置一种或多种采集方式。

    同时监听UDP和TCP的示例配置如下：

    ```
    {
         "inputs": [
             {
                 "type": "service_syslog",
                 "detail": {
                     "Address": "tcp://127.0.0.1:9000",
                     "ParseProtocol": "rfc3164"
                 }
             },
             {
                 "type": "service_syslog",
                 "detail": {
                     "Address": "udp://127.0.0.1:9001",
                     "ParseProtocol": "rfc3164"
                 }
             }
         ]
     }
    ```

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/19015/153786003810991_zh-CN.png)

8.  应用到机器组。

    为机器组添加Logtail配置。请在此处勾选**运行有此插件的Logtail机器组**，并单击**应用到机器组**。

    **说明：** 接收syslog的服务器必须在机器组中，且安装了0.16.13及以上版本的Logtail。

    若您之前没有创建过机器组，单击**+创建机器组**创建一个新的机器组，再勾选机器组并单击**应用到机器组**。

9.  开启并配置索引。

    确保日志机器组心跳正常的情况下，可以点击右侧浏览按钮获取到采集上来的数据。

    如果您后续需要对采集到的syslog进行实时查询与分析，可以单击**展开**，根据日志内容和格式配置键值索引，后续可以按照指定字段进行查询和分析。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/19015/153786003810992_zh-CN.png)


## 配置Logtail插件采集Nginx访问日志 {#section_rcs_bj2_z2b .section}

Nginx支持直接把访问日志以syslog协议转发到指定地址和端口。如果您希望把服务器上包括Nginx访问日志在内的所有数据都以syslog的形式集中投递到日志服务，可以创建Logtail配置并应用到该服务器所在的机器组。

1.  在Nginx服务器的 nginx.conf文件中增加转发规则。

    关于Nginx配置文件的详细信息请参考[Nginx官网说明](http://nginx.org/en/docs/beginners_guide.html#conf_structure)。

    例如，在配置文件中增加如下内容：

    ```
    http {
        ...
    
        # Add this line.
        access_log syslog:server=127.0.0.1:9000,facility=local7,tag=nginx,severity=info combined;
    
        ...
    }
    
    ```

2.  执行以下命令重启Nginx服务，使配置生效。

    ```
    sudo service nginx restart
    ```

3.  创建Logtail配置并应用到该服务器所在的机器组。

    配置过程请参考[配置Logtail插件采集syslog](#)。

4.  检验Logtail配置是否生效。

    在shell中执行命令`curl http://127.0.0.1/test.html`生成一条访问日志。如果采集配置已生效，可以在日志服务控制台的查询页面查看到日志信息。

    ![](https://cdn.nlark.com/lark/0/2018/png/130974/1534754375749-0345311e-e29c-468b-9d89-7bdb017dec36.png)


