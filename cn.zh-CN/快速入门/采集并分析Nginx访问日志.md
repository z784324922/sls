# 采集并分析Nginx访问日志 {#concept_efy_c3q_zdb .concept}

日志服务支持通过数据接入向导配置采集Nginx日志，并自动创建索引和Nginx日志仪表盘，帮助您快速采集并分析Nginx日志。

许多个人站长选取了Nginx作为服务器搭建网站，在对网站访问情况进行分析时，需要对Nginx访问日志统计分析，从中获取网站的访问量、访问时段等访问情况。传统模式下利用CNZZ等方式，在前端页面插入js，用户访问的时候触发js，但仅能记录访问请求。或者利用流计算、离线统计分析Nginx访问日志，但需要搭建一套环境，并且在实时性以及分析灵活性上难以平衡。

日志服务在支持查询分析实时日志功能，同时提供Nginx日志仪表盘（Dashboard），极大的降低了Nginx访问日志的分析复杂度，可以用于便捷统计网站的访问数据。本文档以分析Nginx访问日志为例，介绍日志分析功能在分析Nginx访问日志场景下的详细步骤。

## 应用场景 {#section_q5y_dmq_12b .section}

个人站长选取Nginx作为服务器搭建了个人网站，需要通过分析Nginx访问日志来分析网站的PV、UV、热点页面、热点方法、错误请求、客户端类型和来源页面制表，以分析、评估网站的访问情况。

## Nginx日志格式 {#section_qdx_2mq_12b .section}

**为了更好满足分析场景，推荐Nginx日志格式采用如下`log_format`配置。**

``` {#codeblock_sp3_6rh_d5i}
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

## 采集Nginx日志 {#section_vvf_jmq_12b .section}

采集日志前，请确认您已[创建Project](../cn.zh-CN/用户指南/准备工作/操作Project.md)和[创建Logstore](../cn.zh-CN/用户指南/准备工作/操作Logstore.md)。

1.  登录[日志服务控制台](https://sls.console.aliyun.com)，单击Project名称。
2.  在对应日志库下，单击**数据接入**后的加号。

    您也可以直接单击概览页面右上角的**接入数据**按钮，在后续步骤中再选择对应Logstore。

    ![采集Nginx日志时，数据接入向导入口](images/5890_zh-CN.png "接入数据")

3.  选择数据类型为**Nginx-正则**。

    日志服务提供多种数据类型接入（云产品、自定义代码、自建开源/商业软件等），分析NGINX访问日志请选择**自建开源/商业软件** \> **Nginx-文本日志**。

4.  **创建机器组** 
    1.  在ECS上安装Logtail客户端

        在**ECS机器**页面勾选实例后单击**安装**。Windows系统请参考[安装Logtail（Windows系统）](../cn.zh-CN/用户指南/Logtail采集/安装/安装Logtail（Windows系统）.md)。

    2.  创建机器组

        如果您之前没有创建过机器组，请在安装完Logtail后单击**确认安装完毕**创建机器组。

5.  机器组配置。

    将**源机器组**中的机器组移动到**应用机器组**中。

6.  Logtail配置。

    1.  指定**配置名称**和**日志路径**。
    2.  将推荐的`log_format`日志格式填写到**NGINX日志配置**中。

        ![配置Nginx访问日志为数据源](images/5891_zh-CN.png "配置数据源")

    3.  确认**Nginx键名称**。

        日志服务会自动提取出Nginx日志中的键名称，请确认是否正确。

        **说明：** **Nginx日志格式**中的`$request`会被提取为`request_method`和`request_uri`两个键。

        ![Nginx访问日志的默认键名称](images/5892_zh-CN.png "NGINX键名称")

    4.  确认是否**丢弃解析失败日志**，并单击**下一步**。

        开启该功能后，解析失败的Nginx访问日志不上传到日志服务；关闭后，Nginx访问日志解析失败时上传原始Nginx访问日志。

7.  查询分析配置

    默认已经设置好索引，如果您需要重新设置索引，请在查询分析页面选择**查询分析属性** \> **设置**进行修改。

    ![Nginx访问日志的索引](images/5895_zh-CN.png "字段索引属性")

    确保日志机器组心跳正常的情况下，可以预览采集上来的数据。

    ![预览Nginx访问日志](images/5894_zh-CN.png "预览")

    单击**下一步**，日志服务会为您设置好Nginx访问日志的索引属性并创建`nginx-dashboard`仪表盘以供分析使用。

    **说明：** Nginx日志采集配置生效时间最长需要3分钟，请耐心等待。


## 分析Nginx访问日志 {#section_mz1_1nq_12b .section}

开启索引后，日志服务默认生成Nginx访问日志的索引和仪表盘，可以通过以下方式分析Nginx访问日志。

-   使用SQL语句分析Nginx访问日志：

    在日志服务查询分析页面输入查询分析语句，可以查看符合条件的Nginx原始日志，或查看可视化的分析结果。另外，查询分析页面还提供快速分析、快速查询等功能，详细说明请查看[查询日志](../cn.zh-CN/用户指南/查询与分析/查询日志.md)和[Nginx访问日志诊断及优化](cn.zh-CN/快速入门/采集并分析Nginx访问日志.md#section_qkz_mnq_12b)。

-   查看预设仪表盘的分析数据，分析Nginx访问日志：

    日志服务预设的Nginx访问日志仪表盘中展示了各个分析指标的详细数据大盘，例如PV/UV统计等数据。关于如何使用仪表盘，请参考[创建和删除仪表盘](../cn.zh-CN/用户指南/可视化分析/仪表盘/创建和删除仪表盘.md)。


![Nginx访问日志的默认仪表盘](images/5896_zh-CN.png "Nginx访问日志仪表盘")

-   **PV/UV统计\(pv\_uv\)** 

    统计最近一天的PV数和UV数。

    ![Nginx访问日志的PV/UV统计](images/5897_zh-CN.png "PV/UV统计")

    统计语句：

    ``` {#codeblock_a0a_qx5_k68}
      * | select approx_distinct(remote_addr) as uv ,
             count(1) as pv , 
             date_format(date_trunc('hour', __time__), '%m-%d %H:%i')  as time
             group by date_format(date_trunc('hour', __time__), '%m-%d %H:%i')
             order by time
             limit 1000
    ```

-   **访问地域分析\(ip\_distribution\)** 

    统计访问IP来源情况。

    ![Nginx访问日志-访问地域分析](images/5898_zh-CN.png "访问地域分析")

    统计语句：

    ``` {#codeblock_yzt_2gq_z3l}
     * | select count(1) as c, 
         ip_to_province(remote_addr) as address 
         group by ip_to_province(remote_addr) limit 100
    ```

-   **访问前十地址\(top\_page\)** 

    统计最近一天访问PV前十的地址。

    ![Nginx访问日志-统计访问数据](images/5899_zh-CN.png "统计访问")

    统计语句：

    ``` {#codeblock_arm_5x1_717}
     * | select split_part(request_uri,'?',1) as path, 
         count(1) as pv  
         group by split_part(request_uri,'?',1) 
         order by pv desc limit 10
    ```

-   **请求方法占比\(http\_method\_percentage\)** 

    统计最近一天各种请求方法的占比。

    ![Nginx访问日志-请求方法占比](images/5900_zh-CN.png "请求方法占比")

    统计语句：

    ``` {#codeblock_hu3_jeh_1it}
     * | select count(1) as pv,
             request_method
             group by request_method
    ```

-   **请求状态占比\(http\_status\_percentage\)** 

    统计最近一天各种http状态码的占比。

    ![Nginx访问日志-请求状态占比](images/5901_zh-CN.png "请求状态占比")

    统计语句：

    ``` {#codeblock_zwb_xjc_a4e}
     * | select count(1) as pv,
             status
             group by status
    ```

-   **请求UA占比\(user\_agent\)** 

    统计最近一天各种浏览器的占比。

    ![Nginx访问日志-请求UA占比](images/5902_zh-CN.png "请求UA占比")

    统计语句：

    ``` {#codeblock_ati_wdf_4mf}
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

    ![Nginx访问日志-前十访问来源](images/5903_zh-CN.png "前十访问来源")

    统计语句：

    ``` {#codeblock_mnk_d18_aie}
     * | select count(1) as pv,
             http_referer
             group by http_referer
             order by pv desc limit 10
    ```


## Nginx访问日志诊断及优化 {#section_qkz_mnq_12b .section}

除了一些默认的访问指标外，站长常常还需要对一些访问请求进行诊断，分析Nginx访问日志中记录的处理请求的延时如何、有哪些比较大的延时、哪些页面的延时比较大。此时可以进入查询页面进行快速分析。

-   **统计平均延时和最大延时** 

    通过每5分钟的平均延时和最大延时，从整体上了解延时情况。

    统计语句：

    ``` {#codeblock_s2k_816_rfv}
      * | select from_unixtime(__time__ -__time__% 300) as time, 
              avg(request_time) as avg_latency ,
              max(request_time) as max_latency  
              group by __time__ -__time__% 300
    ```

-   **统计最大延时对应的请求页面** 

    知道了最大延时之后，需要明确最大延时对应的请求页面，以方便进一步优化页面响应。

    统计语句：

    ``` {#codeblock_ynq_94i_jzr}
      * | select from_unixtime(__time__ - __time__% 60) , 
              max_by(request_uri,request_time)  
              group by __time__ - __time__%60
    ```

-   **统计请求延时的分布** 

    统计网站的所有请求的延时的分布，把延时分布在十个桶里面，看每个延时区间的请求个数。

    统计语句：

    ``` {#codeblock_ek2_qas_n34}
    * |select numeric_histogram(10,request_time)
    ```

-   **统计最大的十个延时** 

    除最大的延时之外，还需要统计最大的十个延时及其对应值。

    统计语句：

    ``` {#codeblock_w30_i4n_g6a}
    * | select max(request_time,10)
    ```

-   **对延时最大的页面调优** 

    假如`/url2`这个页面的访问延时最大，为了对`/url2`页面进行调优，接下来需要统计`/url2`这个页面的访问PV、UV、各种method次数、各种status次数、各种浏览器次数、平均延时和最大延时。

    统计语句：

    ``` {#codeblock_is3_isq_096}
       request_uri:"/url2" | select count(1) as pv,
              approx_distinct(remote_addr) as uv,
              histogram(method) as method_pv,
              histogram(status) as status_pv,
              histogram(user_agent) as user_agent_pv,
              avg(request_time) as avg_latency,
              max(request_time) as max_latency
    ```


得到以上数据后，就可以对网站的访问情况进行有针对性的详细评估。

