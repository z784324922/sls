# 分析Apache日志 {#task_gdc_rpl_52b .task}

日志服务支持通过数据接入向导一站式配置采集Apache日志与设置索引。您可以通过默认仪表盘与查询分析语句实时分析网站访问情况。

-   已开启日志服务。
-   已创建了Project和Logstore。

个人站长选用Apache作为服务器搭建网站，需要通过分析Apache访问日志来获取PV、UV、IP区域分布、错误请求、客户端类型和来源页面等，以评估网站访问情况。

日志服务支持通过数据接入向导一站式配置采集Apache日志与设置索引，并为Apache日志默认创建访问分析仪表盘。

为了贴合分析场景，推荐您对Apache日志采用以下自定义配置。

``` {#codeblock_q6r_lnn_m96}
LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\" %D %f %k %p %q %R %T %I %O" customized
```

**说明：** 请根据您的日志内容判断是否有部分字段对应的日志内容中存在空格，例如`%t`、`%{User-Agent}i`、`%{Referer}i`等。若有存在空格的字段，请在配置信息中用`\"`包裹该字段，以免影响日志解析。

各个字段含义如下：

|字段|字段名|说明|
|:-|:--|:-|
|%h|remote\_addr|客户端IP地址。|
|%l|remote\_ident|客户端日志名称，来自identd。|
|%u|remote\_user|客户端用户名。|
|%t|time\_local|服务器时间。|
|%r|request|请求内容，包括方法名、地址和http协议。|
|%\>s|status|返回的http状态码。|
|%b|response\_size\_bytes|返回的大小。|
|%\{Rererer\}i|http\\u0008\_referer|来源页。|
|%\{User-Agent\}i|http\_user\_agent|客户端信息。|
|%D|request\_time\_msec|请求时间，单位为毫秒。|
|%f|filename|带路径的请求文件名。|
|%k|keep\_alive|keep-alive请求数。|
|%p|remote\_port|服务器端口号。|
|%q|request\_query|查询字符串，如果不存在则为空字符串。|
|%R|response\_handler|服务器响应的处理程序。|
|%T|request\_time\_sec|请求时间，单位为秒。|
|%I|bytes\_received|服务器接收的字节数，需要启用mod\_logio模块。|
|%O|bytes\_sent|服务器发送的字节数，需要启用mod\_logio模块。|

1.  登录[日志服务控制台](https://sls.console.aliyun.com)，单击Project名称。
2.  在对应日志库下，单击**数据接入**后的加号。
3.  选择数据类型**APACHE-文本日志**。
4.  选择日志库 可以选择已有的Logstore，也可以新建Project和Logstore。
5.  创建机器组。 

    在创建机器组之前，您需要首先确认已经安装了Logtail。

    -   集团内部机器：默认自动安装，如果没有安装，请根据界面提示进行咨询。
    -   ECS机器： 勾选实例后单击**安装**进行一键式安装。Windows系统不支持一键式安装，请参考[安装Logtail（Windows系统）](../cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Windows系统）.md)手动安装。
    -   自建机器：请根据界面提示进行安装。或者参考[安装Logtail（Linux系统）](../cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Linux系统）.md#)或[安装Logtail（Windows系统）](../cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Windows系统）.md#)文档进行安装。
    安装完Logtail后单击**确认安装完毕**创建机器组。如果您之前已经创建好机器组 ，请直接单击**使用现有机器组**。

6.  应用机器组。
7.  Logtail配置。 
    1.  填写**配置名称**。
    2.  填写**日志路径**。
    3.  选择**日志格式**。 请按照您的Apache日志配置文件中声明的格式选择**日志格式**。为了便于日志数据的查询分析，日志服务推荐您使用自定义的Apache日志格式。
    4.  填写**APACHE配置字段**。 若您的**日志格式**为**common**或**combined**，此处会自动填写对应的配置。若您选择了**自定义**日志格式，请在此处填写您的自定义配置。建议填写上文中日志服务的推荐配置。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/15647371869380_zh-CN.png)

    5.  确认**APACHE键名称**。 日志服务会自动解析您的APACHE键名称，您可以在页面中确认。

        **说明：** %r会被提取为`request_method`、`request_uri`和`request_protocol`三个键。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/15647371869381_zh-CN.png)

    6.  配置高级选项，并单击下一步。 
    7.  查询分析配置 默认已经设置好索引，如果您需要重新设置索引，请在查询分析页面选择**查询分析属性** \> **设置**进行修改。

        确保日志机器组心跳正常的情况下，可以预览采集上来的数据。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/15647371869382_zh-CN.png)

8.  **可视化分析**。 

    系统已为您预设了名为LogstoreName-apache-dashboard的仪表盘。配置完成后，您可以在仪表盘页面中查看来源IP分布、请求状态占比等实时动态。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/15647371879384_zh-CN.png)

    -   **访问地域分析（ip\_distribution）**：统计访问IP来源情况，统计语句如下：

        ``` {#codeblock_gfn_jj2_0q8}
        * | select ip_to_province(remote_addr) as address,
                    count(1) as c
                    group by address limit 100
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/15647371879389_zh-CN.png)

    -   **请求状态占比（http\_status\_percentage）**：统计最近一天各种http状态码的占比，统计语句如下：

        ``` {#codeblock_nox_hs7_l6o}
        status>0 | select status,
                        count(1) as pv 
                        group by status
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/15647371879388_zh-CN.png)

    -   **请求方法占比（http\_method\_percentage）**：统计最近一天各种请求方法的占比，统计语句如下：

        ``` {#codeblock_yiz_gm5_vbm}
        * | select request_method,
                    count(1) as pv 
                    group by request_method
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/15647371879387_zh-CN.png)

    -   **PV/UV统计（pv\_uv）**：统计最近的PV数和UV数，统计语句如下：

        ``` {#codeblock_msr_87e_s8n}
        * | select date_format(date_trunc('hour', __time__), '%m-%d %H:%i')  as time,
                    count(1) as pv,
                    approx_distinct(remote_addr) as uv
                    group by time
                    order by time
                    limit 1000
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/15647371879385_zh-CN.png)

    -   **出入流量统计（net\_in\_net\_out）**：统计流量的流入和流出情况，统计语句如下：

        ``` {#codeblock_340_zpy_kpq}
        * | select date_format(date_trunc('hour', __time__), '%m-%d %H:%i') as time, 
                    sum(bytes_sent) as net_out, 
                    sum(bytes_received) as net_in 
                    group by time 
                    order by time 
                    limit 10000mit 10
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/15647371889391_zh-CN.png)

    -   **请求UA占比（http\_user\_agent\_percentage）**：统计最近一天各种浏览器的占比，统计语句如下：

        ``` {#codeblock_ff0_qjl_myu}
        * | select  case when http_user_agent like '%Chrome%' then 'Chrome' 
                    when http_user_agent like '%Firefox%' then 'Firefox' 
                    when http_user_agent like '%Safari%' then 'Safari'
                    else 'unKnown' end as http_user_agent,count(1) as pv
                    group by  http_user_agent
                    order by pv desc
                    limit 10
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/15647371889390_zh-CN.png)

    -   **前十访问来源（top\_10\_referer）**：统计最近一天访问PV最高的前十个访问来源页面，统计语句如下：

        ``` {#codeblock_v2n_01o_ag8}
        * | select  http_referer, 
                    count(1) as pv 
                    group by http_referer 
                    order by pv desc limit 10
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/156473718810098_zh-CN.png)

    -   **访问前十地址（top\_page）**：统计最近一天访问pv前十的地址，统计语句如下：

        ``` {#codeblock_cw7_plt_eua}
        * | select split_part(request_uri,'?',1) as path, 
                    count(1) as pv  
                    group by path
                    order by pv desc limit 10
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/15647371889386_zh-CN.png)

    -   **请求时间前十地址（top\_10\_latency\_request\_uri）**：统计最近一天请求响应延时最长的前十个地址，统计语句如下：

        ``` {#codeblock_my4_e72_f7w}
        * | select request_uri as top_latency_request_uri,
                    request_time_sec 
                    order by request_time_sec desc limit 10 10
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/156473718810099_zh-CN.png)


