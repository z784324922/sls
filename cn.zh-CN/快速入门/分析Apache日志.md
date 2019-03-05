# 分析Apache日志 {#task_gdc_rpl_52b .task}

日志服务支持通过数据接入向导一站式配置采集Apache日志与设置索引。您可以通过默认仪表盘与查询分析语句实时分析网站访问情况。

-   已开启日志服务。
-   已创建了Project和Logstore。

个人站长选用Apache作为服务器搭建网站，需要通过分析Apache访问日志来获取PV、UV、IP区域分布、错误请求、客户端类型和来源页面等，以评估网站访问情况。

日志服务支持通过数据接入向导一站式配置采集Apache日志与设置索引，并为Apache日志默认创建访问分析仪表盘。

为了贴合分析场景，推荐您对Apache日志采用以下自定义配置。

```
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
2.  在Logstore列表页面单击对应Logstore的数据接入向导图标。 
3.  选择数据类型为**APACHE访问日志**，并单击**下一步**。 
4.  配置数据源。 
    1.  填写**配置名称**。 
    2.  填写**日志路径**。 
    3.  选择**日志格式**。 请按照您的Apache日志配置文件中声明的格式选择**日志格式**。为了便于日志数据的查询分析，日志服务推荐您使用自定义的Apache日志格式。
    4.  填写**Apache配置字段**。 若您的**日志格式**为**common**或**combined**，此处会自动填写对应的配置。若您选择了**自定义**日志格式，请在此处填写您的自定义配置。建议填写上文中日志服务的推荐配置。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/15517513739380_zh-CN.png)

    5.  确认**APACHE键名称**。 日志服务会自动解析您的APACHE键名称，您可以在页面中确认。

        **说明：** %r会被提取为`request_method`、`request_uri`和`request_protocol`三个键。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/15517513739381_zh-CN.png)

    6.  配置高级选项，并单击下一步。 

        |配置项|详情|
        |:--|:-|
        |上传原始日志|请选择是否需要上传原始日志。开启该功能后，原始日志内容会作为\_\_raw\_\_字段与解析过的日志一并上传。|
        |Topic生成方式|         -   **空-不生成Topic**：默认选项，表示设置Topic为空字符串，在查询日志时不需要输入Topic即可查询。
        -   **机器组Topic属性**：设置Topic生成方式为机器组Topic属性，可以用于明确区分不同前端服务器产生的日志数据。
        -   **文件路径正则**：选择此项之后，您需要填写下方的**自定义正则**，用正则式从路径里提取一部分内容作为Topic。可以用于区分具体用户或实例产生的日志数据。
 |
        |自定义正则|如您选择了**文件路径正则**方式生成Topic，需要在此处填写您的自定义正则式。|
        |日志文件编码|         -   utf8：指定使用UTF-8编码。
        -   gbk：指定使用GBK编码。
 |
        |最大监控目录深度|指定从日志源采集日志时，监控目录的最大深度，即最多监控几层日志。最大目录监控深度范围0-1000，0代表只监控本层目录。|
        |超时属性|如果一个日志文件在指定时间内没有任何更新，则认为该文件已超时。您可以对**超时属性**指定以下配置。        -   永不超时：指定持续监控所有日志文件，永不超时。
        -   30分钟超时：如日志文件在30分钟内没有更新，则认为已超时，并不再监控该文件。
|
        |过滤器配置|日志只有**完全符合**过滤器中的条件才会被收集。例如：

        -   **满足条件即收集**：配置`Key:level Regex:WARNING|ERROR`，表示只收集level为WARNING或ERROR类型的日志。
        -   **[过滤不符合条件的数据](http://www.regular-expressions.info/lookaround.html)**：
            -   配置`Key:level Regex:^(?!.*(INFO|DEBUG))`，表示代表不收集level为INFO或DEBUG类型的日志。
            -   配置`Key:url Regex:.*^(?!.*(healthcheck)).*`，表示不采集url中带有healthcheck的日志，例如key为url，value为`/inner/healthcheck/jiankong.html`的日志将不会被采集。
更多示例可参考[regex-exclude-word](https://stackoverflow.com/questions/2404010/match-everything-except-for-specified-strings)、[regex-exclude-pattern](https://stackoverflow.com/questions/2078915/a-regular-expression-to-exclude-a-word-string)。

|

5.  应用配置到机器组。 勾选需要应此用配置的机器组，单击**应用到机器组**。

    如您尚未创建机器组，请单击**+创建机器组**创建一个机器组。

6.  配置**查询分析&可视化**。 确保日志机器组心跳正常的情况下，可以点击右侧**浏览**按钮获取到采集上来的数据。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/15517513739382_zh-CN.png)

    如您需要对采集到日志服务的日志数据进行实时查询与分析，请在当前页面中确认您的索引属性配置。单击**展开**可查看键值索引属性。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/15517513749383_zh-CN.png)

    系统已为您预设了名为LogstoreName-apache-dashboard的仪表盘。配置完成后，您可以在仪表盘页面中查看来源IP分布、请求状态占比等实时动态。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/15517513749384_zh-CN.png)

    -   **访问地域分析（ip\_distribution）**：统计访问IP来源情况，统计语句如下：

        ```
        * | select ip_to_province(remote_addr) as address,
                    count(1) as c
                    group by address limit 100
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/15517513749389_zh-CN.png)

    -   **请求状态占比（http\_status\_percentage）**：统计最近一天各种http状态码的占比，统计语句如下：

        ```
        status>0 | select status,
                        count(1) as pv 
                        group by status
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/15517513749388_zh-CN.png)

    -   **请求方法占比（http\_method\_percentage）**：统计最近一天各种请求方法的占比，统计语句如下：

        ```
        * | select request_method,
                    count(1) as pv 
                    group by request_method
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/15517513749387_zh-CN.png)

    -   **PV/UV统计（pv\_uv）**：统计最近的PV数和UV数，统计语句如下：

        ```
        * | select date_format(date_trunc('hour', __time__), '%m-%d %H:%i')  as time,
                    count(1) as pv,
                    approx_distinct(remote_addr) as uv
                    group by time
                    order by time
                    limit 1000
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/15517513749385_zh-CN.png)

    -   **出入流量统计（net\_in\_net\_out）**：统计流量的流入和流出情况，统计语句如下：

        ```
        * | select date_format(date_trunc('hour', __time__), '%m-%d %H:%i') as time, 
                    sum(bytes_sent) as net_out, 
                    sum(bytes_received) as net_in 
                    group by time 
                    order by time 
                    limit 10000mit 10
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/15517513749391_zh-CN.png)

    -   **请求UA占比（http\_user\_agent\_percentage）**：统计最近一天各种浏览器的占比，统计语句如下：

        ```
        * | select  case when http_user_agent like '%Chrome%' then 'Chrome' 
                    when http_user_agent like '%Firefox%' then 'Firefox' 
                    when http_user_agent like '%Safari%' then 'Safari'
                    else 'unKnown' end as http_user_agent,count(1) as pv
                    group by  http_user_agent
                    order by pv desc
                    limit 10
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/15517513749390_zh-CN.png)

    -   **前十访问来源（top\_10\_referer）**：统计最近一天访问PV最高的前十个访问来源页面，统计语句如下：

        ```
        * | select  http_referer, 
                    count(1) as pv 
                    group by http_referer 
                    order by pv desc limit 10
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/155175137410098_zh-CN.png)

    -   **访问前十地址（top\_page）**：统计最近一天访问pv前十的地址，统计语句如下：

        ```
        * | select split_part(request_uri,'?',1) as path, 
                    count(1) as pv  
                    group by path
                    order by pv desc limit 10
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/15517513749386_zh-CN.png)

    -   **请求时间前十地址（top\_10\_latency\_request\_uri）**：统计最近一天请求响应延时最长的前十个地址，统计语句如下：

        ```
        * | select request_uri as top_latency_request_uri,
                    request_time_sec 
                    order by request_time_sec desc limit 10 10
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/155175137410099_zh-CN.png)


