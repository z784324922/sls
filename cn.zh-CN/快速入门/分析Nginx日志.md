# 分析Nginx日志 {#concept_efy_c3q_zdb .concept}

许多个人站长选取了Nginx作为服务器搭建网站，在对网站访问情况进行分析时，需要对Nginx访问日志统计分析，从中获取网站的访问量、访问时段等访问情况。传统模式下利用CNZZ等方式，在前端页面插入js，用户访问的时候触发js，但仅能记录访问请求。或者利用流计算、离线统计分析Nginx的访问日志，但需要搭建一套环境，并且在实时性以及分析灵活性上难以平衡。

日志服务在支持查询分析实时日志功能，并将分析结果保存到仪表盘（Dashboard），极大的降低了Nginx访问日志的分析复杂度，可以用于便捷统计网站的访问数据。本文档以分析Nginx访问日志为例，介绍日志分析功能在分析Nginx访问日志场景下的详细步骤。

## 应用场景 {#section_q5y_dmq_12b .section}

个人站长选取Nginx作为服务器搭建了个人网站，需要通过分析Nginx访问日志来获取网站的PV、UV、热点页面、热点方法、错误请求、客户端类型和来源页面制表，以评估网站的访问情况。

## 日志格式 {#section_qdx_2mq_12b .section}

**为了更好满足分析场景，推荐采用如下`log_format`配置。**

```
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" $http_host '
                        '$status $request_length $body_bytes_sent "$http_referer" '
                        '"$http_user_agent"  $request_time $upstream_response_time';
```

各字段含义如下：

|字段|含义|
|:-|:-|
|remote\_addr|客户端地址|
|remote\_user|客户端用户名|
|time\_local|服务器时间|
|request|请求内容，包括方法名、地址和http协议|
|http\_host|用户请求时使用的http地址|
|status|返回的http状态码|
|request\_length|请求大小|
|body\_bytes\_sent|返回的大小|
|http\_referer|来源页|
|http\_user\_agent|客户端名称|
|request\_time|整体请求延时|
|upstream\_response\_time|上游服务的处理延时|

## 配置步骤 {#section_vvf_jmq_12b .section}

## 步骤1 数据接入向导 {#section_e33_kmq_12b .section}

日志服务提供数据接入向导快速接入各类数据源，将Nginx访问日志采集到日志服务可以采用如下两种方式进入数据接入向导。

-   新建项目

    在创建项目和创建日志库后，根据页面提示点击**数据接入向导**。

     ![](images/5889_zh-CN.png "数据接入向导") 

-   对于已存在的Logstore，点击列表中数据接入向导图标进入。

    ![](images/5890_zh-CN.png "Logstore列表")


## 步骤2 选择数据类型 {#section_mkt_nmq_12b .section}

日志服务提供多种数据类型接入（云产品、自建软件、API、SDK等），分析NGINX访问日志请选择**自建软件** \> **NGINX访问日志**。

## 步骤3 数据源设置 {#section_tvf_qmq_12b .section}

1.  按照实际情况填写配置名称和日志路径，并将推荐的`log_format`信息填写到NGINX日志格式中。

    ![](images/5891_zh-CN.png "配置数据源")

    日志服务会自动提取出相应的键名称。

    **说明：** 其中`$request`会被提取为`request_method`和`request_uri`两个键。

    ![](images/5892_zh-CN.png "NGINX键")

2.  应用到机器组。

    如果您之前没有创建过机器组，请先根据页面提示[创建机器组](../../../../cn.zh-CN/用户指南/Logtail 采集/机器组/创建机器组.md)。

    **说明：** Logtail配置推送生效时间最长需要3分钟，请耐心等待。


## 步骤4 查询分析和可视化 {#section_lzx_wmq_12b .section}

确保日志机器组心跳正常的情况下，可以通过点击右侧预览按钮获取到采集上来的数据。

![](images/5894_zh-CN.png "预览")

日志服务提供预设的数据键名称以便分析使用，可以选择实际数据键名称（根据预览数据生成）和默认数据键名称形成映射关系。

![](images/5895_zh-CN.png "键值索引属性")

点击下一步，日志服务会为您设置好索引属性并创建`nginx-dashboard`仪表盘以供分析使用。

## 步骤5 分析访问日志 {#section_mz1_1nq_12b .section}

如下图所示，开启索引后，默认生成仪表盘页面可以快速看到各个指标的分析情况。关于如何使用仪表盘，请参考[仪表盘](../../../../cn.zh-CN/用户指南/查询与可视化/分析图表/仪表盘.md)。

![](images/5896_zh-CN.png "仪表盘")

-   **PV/UV统计\(pv\_uv\)**

    统计最近一天的PV数和UV数。

    ![](images/5897_zh-CN.png "PV/UV统计")

    统计语句：

    ```
      * | select approx_distinct(remote_addr) as uv ,
             count(1) as pv , 
             date_format(date_trunc('hour', __time__), '%m-%d %H:%i')  as time
             group by date_format(date_trunc('hour', __time__), '%m-%d %H:%i')
             order by time
             limit 1000
    ```

-   **访问地域分析\(ip\_distribution\)**

    统计访问IP来源情况。

     ![](images/5898_zh-CN.png "访问地域分析") 

    统计语句：

    ```
     * | select count(1) as c, 
         ip_to_province(remote_addr) as address 
         group by ip_to_province(remote_addr) limit 100
    ```

-   **访问前十地址\(top\_page\)**

    统计最近一天访问PV前十的地址。

    ![](images/5899_zh-CN.png "统计访问")

    统计语句：

    ```
     * | select split_part(request_uri,'?',1) as path, 
         count(1) as pv  
         group by split_part(request_uri,'?',1) 
         order by pv desc limit 10
    ```

-   **请求方法占比\(http\_method\_percentage\)**

    统计最近一天各种请求方法的占比。

    ![](images/5900_zh-CN.png "请求方法占比")

    统计语句：

    ```
     * | select count(1) as pv,
             request_method
             group by request_method
    ```

-   **请求状态占比\(http\_status\_percentage\)**

    统计最近一天各种http状态码的占比。

    ![](images/5901_zh-CN.png "请求状态占比")

    统计语句：

    ```
     * | select count(1) as pv,
             status
             group by status
    ```

-   **请求UA占比\(user\_agent\)**

    统计最近一天各种浏览器的占比。

    ![](images/5902_zh-CN.png "请求UA占比")

    统计语句：

    ```
     * | select count(1) as pv,
         case when http_user_agent like '%Chrome%' then 'Chrome' 
         when http_user_agent like '%Firefox%' then 'Firefox' 
         when http_user_agent like '%Safari%' then 'Safari'
         else 'unKnown' end as http_user_agent
         group by  http_user_agent
         order by pv desc
         limit 10
    ```

-   **前十访问来源\(top\_10\_referer\)**

    统计最近一天访问前十的来源信息。

    ![](images/5903_zh-CN.png "前十访问来源")

    统计语句：

    ```
     * | select count(1) as pv,
             http_referer
             group by http_referer
             order by pv desc limit 10
    ```


## 6 访问诊断及优化 {#section_qkz_mnq_12b .section}

除了一些默认的访问指标外，站长常常还需要对一些访问请求进行诊断，查看一下处理请求的延时如何，有哪些比较大的延时，哪些页面的延时比较大。此时可以进入查询页面进行快速分析。

-   **统计平均延时和最大延时**

    通过每5分钟的平均延时和最大延时，从整体上了解延时情况。

    统计语句：

    ```
      * | select from_unixtime(__time__ -__time__% 300) as time, 
              avg(request_time) as avg_latency ,
              max(request_time) as max_latency  
              group by __time__ -__time__% 300
    ```

-   **统计最大延时对应的请求页面**

    知道了最大延时之后，需要明确最大延时对应的请求页面是，以方便进一步优化页面响应。

    统计语句：

    ```
      * | select from_unixtime(__time__ - __time__% 60) , 
              max_by(request_uri,request_time)  
              group by __time__ - __time__%60
    ```

-   **统计请求延时的分布**

    统计网站的所有请求的延时的分布，把延时分布在十个桶里面，看每个延时区间的请求个数。

    统计语句：

    ```
    * |select numeric_histogram(10,request_time)
    ```

-   **统计最大的十个延时**

    除最大的延时之外，还需要统计最大的十个延时及其对应值。

    统计语句：

    ```
    * | select max(request_time,10)
    ```

-   **对延时最大的页面调优**

    假如`/url2`这个页面的访问延时最大，为了对`/url2`页面进行调优，接下来需要统计`/url2`这个页面的访问PV、UV、各种method次数、各种status次数、各种浏览器次数、平均延时和最大延时。

    统计语句：

    ```
       request_uri:"/url2" | select count(1) as pv,
              approx_distinct(remote_addr) as uv,
              histogram(method) as method_pv,
              histogram(status) as status_pv,
              histogram(user_agent) as user_agent_pv,
              avg(request_time) as avg_latency,
              max(request_time) as max_latency
    ```


得到以上数据后，就可以对网站的访问情况进行有针对性的详细评估。

