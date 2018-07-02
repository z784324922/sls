# 分析IIS日志 {#task_crv_5z2_32b .task}

个人站长选用IIS作为服务器搭建网站，需要通过分析IIS访问日志来获取PV、UV、IP区域分布、错误请求、流量流入流出，以评估网站访问情况。

-   已开启日志服务。
-   已创建了Project和Logstore。详细步骤请参见[准备流程](../cn.zh-CN/用户指南/准备工作/准备流程.md)。

**日志格式**

为了更好满足分析场景，推荐选用W3C日志格式，在IIS管理器中单击**选择字段**按钮，勾选**发送的字节数**和**和接收的字节数**。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15250/6664_zh-CN.png "选择字段")

配置格式如下：

```
logExtFileFlags="Date, Time, ClientIP, UserName, SiteName, ComputerName, ServerIP, Method, UriStem, UriQuery, HttpStatus, Win32Status, BytesSent, BytesRecv, TimeTaken, ServerPort, UserAgent, Cookie, Referer, ProtocolVersion, Host, HttpSubStatus"
```

-   **字段前缀说明**

    |前缀|含义|
    |s-|服务器操作|
    |c-|客户端操作|
    |cs-|客户端到服务器的操作|
    |sc-|服务器到客户的操作|

-   **各字字说明义**

    |字段|说明|
    |date|日期，表示活动发生的日期。|
    |time|时间，表示活动发生的时间。|
    |s-sitename|服务名，表示客户端所访问的该站点的 Internet 服务和实例的号码。|
    |s-computername|服务器名，表示生成日志项的服务器名称。|
    |s-ip|服务器IP，表示生成日志项的服务器的IP地址。|
    |cs-method|​方法，例如GET或POST。|
    |cs-uri-stem|​URI资源，表示请求访问的地址。|
    |cs-uri-query|URI查询，表示查询HTTP请求中问号（?）后的信息。|
    |s-port|服务器端口，表示客户端连接的服务器端口号。|
    |cs-username|通过验证的域或用户名，对于通过身份验证的用户，格式是`域\用户名`；对于匿名用户，是一个连字符 \(-\)。|
    |c-ip|客户端IP，表示访问服务器的客户端 真实IP 地址。|
    |cs-version|协议版本，例如 HTTP 1.0 或 HTTP 1.1。|
    |user-agent|用户代理，表示在客户端使用的浏览器。|
    |Cookie|Cookie，表示发送或接受的Cookie内容，如果没有显示连字符（-）。|
    |referer|引用站点，表示用户访问的前一个站点。此站点提供到当前站点到链接|
    |cs-host|主机，表示主机头内容。|
    |sc-status|​协议返回状态，表示HTTP或FTP的操作状态。|
    |sc-substatus|HTTP子协议的状态。|
    |sc-win32-status|​win32状态，即用 Windows使用的术语表示的操作的状态。|
    |sc-bytes|​服务器发送字节|
    |cs-bytes|​服务器接收字节|
    |time-taken|​所用时间，即操作所花时间长短，单位为毫秒。|


1.  进入数据接入向导。 
    1.  在日志服务控制台首页单击Project名称，进入Logstore列表。 
    2.  单击**数据接入向导**图标。 
2.  选择数据类型为**自建软件**一栏的**IIS访问日志**。 
3.  设置数据源。 
    1.  填写**配置名称**和**日志路径**。 您可在以IIS管理器中查看日志路径。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15250/6665_zh-CN.png "查看日志路径")

4.  选择**日志格式**。 选择您的IIS服务器日志采用的日志格式。
    -   IIS：Microsoft IIS日志文件格式。
    -   NCSA：NCSA公用日志文件格式。
    -   W3C：W3C扩展日志文件格式。
5.  填写**IIS配置字段**。 
    -   IIS或NCSA格式：配置字段已预设。
    -   W3C日志：请按照以下步骤配置**IIS配置字段**。
    1.  打开IIS配置文件。 

        -   IIS5配置文件默认路径：C:\\WINNT\\system32\\inetsrv\\MetaBase.bin
        -   IIS6配置文件默认路径：C:\\WINDOWS\\system32\\inetsrv\\MetaBase.xml
        -   IIS7配置文件默认路径：C:\\Windows\\System32\\inetsrv\\config\\applicationHost.config
        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15250/6666_zh-CN.png "查看配置文件")

    2.  找到`logFile logExtFileFlags`字段，并复制引号内的字段内容。 
    3.  粘贴字段内容到**IIS配置字段**输入框中的引号内。 

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15250/6667_zh-CN.png "配置数据源")

6.  确认IIS键名称。 IIS日志服务会自动提取出相应的键名称。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15250/6668_zh-CN.png "IIS键名称")

7.  酌情配置高级选项（可选）。 

    |配置项|详情|
    |本地缓存|请选择是否打开**本地缓存**。当日志服务不可用时，日志可以缓存在机器本地目录，服务恢复后进行续传，默认最大缓存值1GB。|
    |上传原始日志|请选择是否需要上传原始日志。开启该功能后默认会新增字段将原始日志内容一并上传。|
    |Topic生成方式|     -   **空-不生成Topic**：默认选项，表示设置Topic为空字符串，在查询日志时不需要输入Topic即可查询。
    -   **机器组Topic属性**：设置Topic生成方式为机器组Topic属性，可以用于明确区分不同前端服务器产生的日志数据。
    -   **文件路径正则**：选择此项之后，您需要填写下方的**自定义正则**，用正则式从路径里提取一部分内容作为Topic。可以用于区分具体用户或实例产生的日志数据。
 |
    |自定义正则|如您选择了**文件路径正则**方式生成Topic，需要在此处填写您的自定义正则式。|
    |日志文件编码|     -   utf8：指定使用UTF-8编码。
    -   gbk：指定使用GBK编码。
 |
    |最大监控目录深度|指定从日志源采集日志时，监控目录的最大深度，即最多监控几层日志。最大目录监控深度范围0-1000，0代表只监控本层目录。|
    |超时属性|如果一个日志文件在指定时间内没有任何更新，则认为该文件已超时。您可以对**超时属性**指定以下配置。    -   永不超时：指定持续监控所有日志文件，永不超时。
    -   30分钟超时：如日志文件在30分钟内没有更新，则认为已超时，并不再监控该文件。
|
    |过滤器配置|日志只有**完全符合**过滤器中的条件才会被收集。例如：配置`Key:level Regex:WARNING|ERROR` 代表只收集level为WARNING或ERROR类型的日志；若需要[过滤不符合某条件的数据](http://www.regular-expressions.info/lookaround.html)，也可用该方式实现，例如：`Key:level Regex:^(?!.*(INFO|DEBUG))` 代表不收集level为INFO或DEBUG类型的日志，类似示例可参考[regex-exclude-word](https://stackoverflow.com/questions/2404010/match-everything-except-for-specified-strings)、[regex-exclude-pattern](https://stackoverflow.com/questions/2078915/a-regular-expression-to-exclude-a-word-string)。

|

    确认配置后单击**下一步**。

8.  应用到机器组。 勾选需要应此用配置的机器组，单击**应用到机器组**。

    如您尚未创建机器组，请单击**+创建机器组**创建一个机器组。

9.  配置**查询分析&可视化**（可选）。 

    确保日志机器组心跳正常的情况下，可以点击右侧浏览按钮获取到采集上来的数据。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15250/6669_zh-CN.png "预览日志")

    如您需要对采集到日志服务的日志数据进行实时查询与分析，请在当前页面中确认您的索引属性配置并单击**下一步**。日志服务提供预设的数据键名称以便分析使用，可以选择实际数据键名称（根据预览数据生成）和默认数据键名称形成映射关系。点击展开可查看键值索引属性。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15250/6670_zh-CN.png "键值索引属性")

    系统已为您预设了名为LogstoreName-iis-dashboard的仪表盘。配置完成后，您可以在仪表盘页面中查看来源IP分布、请求状态占比等实时动态。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15250/6671_zh-CN.png "仪表盘")

    -   **访问地域分析\(ip\_distribution\)**：统计IP来源情况，统计语句为：

        ```
        | select ip_to_geo("c-ip") as country, count(1) as c group by ip_to_geo("c-ip") limit 100
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15250/6672_zh-CN.png "访问地域分析")

    -   **PV/UV统计\(pv\_uv\)**：统计最近的PV数和UV数，统计语句为：

        ```
        *| select approx_distinct("c-ip") as uv ,count(1) as pv , date_format(date_trunc('hour', __time__), '%m-%d %H:%i') as time group by date_format(date_trunc('hour', __time__), '%m-%d %H:%i') order by time limit 1000
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15250/6673_zh-CN.png "PV/UV统计")

    -   **请求状态占比\(http\_status\_percentage\)**：统计http请求状态码的占比，统计语句为：

        ```
        *| select count(1) as pv ,"sc-status" group by "sc-status"
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15250/6674_zh-CN.png "请求状态占比")

    -   **浏览流入流出统计（net\_in\_net\_out）**：统计流量的流入和流出情况，统计语句为：

        ```
        *| select sum("sc-bytes") as net_out, sum("cs-bytes") as net_in ,date_format(date_trunc('hour', time), '%m-%d %H:%i') as time group by date_format(date_trunc('hour', time), '%m-%d %H:%i') order by time limit 10000
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15250/6675_zh-CN.png "出入流量统计")

    -   **请求方法占比\(http\_method\_percentage\)**：统计各种请求方法的占比，统计语句为：

        ```
        *| select count(1) as pv ,"cs-method" group by "cs-method"
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15250/6676_zh-CN.png "请求方法占比")

    -   **请求UA占比\(user\_agent\)**：统计各种浏览器的占比，统计语句为：

        ```
        *| select count(1) as pv, case when "user-agent" like '%Chrome%' then 'Chrome' when "user-agent" like '%Firefox%' then 'Firefox' when "user-agent" like '%Safari%' then 'Safari' else 'unKnown' end as "user-agent" group by case when "user-agent" like '%Chrome%' then 'Chrome' when "user-agent" like '%Firefox%' then 'Firefox' when "user-agent" like '%Safari%' then 'Safari' else 'unKnown' end order by pv desc limit 10
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15250/6677_zh-CN.png "请求UA占比")

    -   **访问前十地址（top\_10\_page）**：统计访问数量前十的地址，统计语句为：

        ```
        *| select count(1) as pv, split_part("cs-uri-stem",'?',1) as path group by split_part("cs-uri-stem",'?',1) order by pv desc limit 10
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15250/6678_zh-CN.png "访问前十地址")


