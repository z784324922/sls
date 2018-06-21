# 对接Grafana {#concept_cfp_pnq_zdb .concept}

阿里云日志服务是针对日志类数据的一站式服务，您只需要将精力集中在日志分析上，过程中数据采集、对接各种存储计算、数据索引和查询等其他工作都可以通过配置日志服务来自动完成。2017年9月日志服务升级日志实时分析功能（LogSearch/Analytics），可以使用查询+SQL92语法对日志进行实时分析。

在结果分析可视化上，除了使用自带Dashboard外，还支持DataV、Grafana、Tableua、QuickBI等对接方式。本文主要通过Grafana示例演示如何通过日志服务对Nginx日志进行分析与可视化。

## 流程架构 {#section_oxj_c1q_12b .section}

日志从收集到分析的流程架构如下。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13155/5856_zh-CN.png "流程架构")

## 配置流程 {#section_cwp_zzp_12b .section}

1.  日志数据采集。详细步骤请参考[五分钟快速入门](../../../../intl.zh-CN/快速入门/五分钟快速入门.md)。
2.  索引设置与控制台查询配置。详细步骤请参考[简介](intl.zh-CN/用户指南/索引与查询/简介.md)或最佳实践[分析Nginx日志](../../../../intl.zh-CN/快速入门/分析Nginx日志.md)。
3.  安装Grafana插件，将实时查询SQL转化为视图。

在做完1、2步骤后，您可以在查询分析界面看到日志服务已采集的日志。

本文档演示步骤3。

## 配置步骤 {#section_zwh_yzp_12b .section}

1.  [安装Grafana](#section_nwf_txp_12b)
2.  [安装日志服务插件](#section_sm4_wxp_12b)
3.  [配置日志数据源](#section_q3b_zxp_12b)
4.  [添加Dashboard](#section_r3v_3yp_12b)
    1.  [配置模板变量](#section_wjd_lyp_12b)
    2.  [配置PV、UV](#section_ccp_vyp_12b)
    3.  [配置出入网带宽](#section_yqs_2zp_12b)
    4.  [不同HTTP方法的占比](#section_zsj_hzp_12b)
    5.  [不同HTTP状态码占比](#section_azy_jzp_12b)
    6.  [热门来源页面](#section_hzq_lzp_12b)
    7.  [延时最高页面](#section_txv_mzp_12b)
    8.  [热门页面](#section_ur2_4zp_12b)
    9.  [非200请求top页面](#section_u3c_pzp_12b)
    10. [前后端平均延时](#section_fzc_qzp_12b)
    11. [客户端统计](#section_sn1_rzp_12b)
    12. [保存和发布Dashboard](#section_tlk_tzp_12b)
5.  [查看结果](#section_nyj_5zp_12b)

## 1 安装Grafana {#section_nwf_txp_12b .section}

Grafana的详细安装步骤请参见[Grafana官方文档](http://docs.grafana.org/installation/)。

以Ubuntu为例，安装命令为：

```
wget https://s3-us-west-2.amazonaws.com/grafana-releases/release/grafana_4.5.2_amd64.deb
sudo apt-get install -y adduser libfontconfig
sudo dpkg -i grafana_4.5.2_amd64.deb
```

如您需要使用饼状图，则需安装Pie chart插件。详细步骤请参考[Grafana官方文档](https://grafana.com/plugins/grafana-piechart-panel/installation)。

安装命令如下：

```
grafana-cli plugins install grafana-piechart-panel
```

## 2 安装日志服务插件 {#section_sm4_wxp_12b .section}

请确认Grafana的插件目录位置。 Ubuntu的插件地址在`/var/lib/grafana/plugins/`，安装好插件后重启grafana-server。

以Ubuntu系统为例，执行以下命令安装插件，并重启grafana-server。

```
cd /var/lib/grafana/plugins/
git clone https://github.com/aliyun/aliyun-log-grafana-datasource-plugin
service grafana-server restart
```

## 3 配置日志数据源 {#section_q3b_zxp_12b .section}

如您是在本机部署，默认是安装在3000端口。请提前在浏览器打开打开3000端口。

1.  在左上角单击Grafana的logo，并在弹出窗口上选择Data Sources。
2.  单击右上角的**Add data source**，添加新的数据源。使用Grafana和阿里云日志服务进行日志可视化分析。
3.  填写新数据源的配置项。

    各部分配置如下：

    |配置项|配置内容|
    |:--|:---|
    |datasource|Name表示名称，请自定义一个新数据源的名称。Type请选择**LogService**。|
    |Http Setting|Url输入样例：`http://dashboard-demo.cn-hangzhou.log.aliyuncs.com`。`dashboard-demo`是project名称，`n-hangzhou.log.aliyuncs.com`c是project所在地域的endpoint，在配置自己的数据源时，需要特别的替换成自己的project和region地址。Access可以选择Direct，也可以选择Proxy。|
    |Http Auth|采用默认配置。|
    |log service details|日志服务详细配置，分别填写Project和Logstore，以及具备读取权限的AccessKey。AccessKey可以是主账号的AccessKey，也可以是子帐号的AccessKey。|

    配置示例：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13155/5857_zh-CN.png "配置示例")


配置完成后单击**Add**，即可完成添加DataSource。

## 4 添加Dashboard {#section_r3v_3yp_12b .section}

单击打开左上角菜单，选择**Dashboards**并单击**New**。在左上角菜单中新加一个Dashboard。

## 4. 1 配置模板变量 {#section_wjd_lyp_12b .section}

在Grafana中可以配置模板变量，在同一个视图中，通过选择不同的变量值，展示不同的视图。本文档主要配置每个时间区间的大小，以及不同域名的访问情况。

1.  单击页面上方设置图标，然后单击**Templating**。
2.  在当前页面显示出已经配置的模板变量，单击**New**， 创建新的模板。

    首先配置一个时间区间，变量的名称是您在配置中使用的变量，在这里起名为myinterval， 在查询条件中，要写成`$myinterval` ，会自动替换成页面选择的模板值。请参照下表配置。

    |配置项|配置内容|
    |:--|:---|
    |Name|变量名称，您可以命名为myinterval。|
    |Type|请选择`Interval`|
    |Lable|请输入`time interval`|
    |Internal Options|value项请输入`1m,10m,30m,1h,6h,12h,1d,7d,14d,30d`|

3.  配置一个域名模板。

    通常一个vps上可以挂载多个域名，那么需要查看不同域名的访问情况。在模板值中，输入`*,www.host.com,www.host0.com,www.host1.com`， 表示可以查看所有域名，也可以分别查看`www.host.com`、`www.host0.com`或者`www.host1.com`的访问情况。

    域名模板配置如下。

    |配置项|配置内容|
    |:--|:---|
    |Name|变量名称，您可以命名为hostname。|
    |Type|请选择`Custom`|
    |Lable|请输入`域名`    ```

    ```

|
    |Custom Options|value项请输入`*,www.host.com,www.host0.com,www.host1.com`|

    配置完成后，可以在Dashboard页面上方看到刚才配置的模板变量，通过下拉框可以选择任何一个值，例如time interval。


## 4.2 配置PV、UV {#section_ccp_vyp_12b .section}

1.  单击左侧ADD ROW，新建一行图表。如果已有一行row，可以在左侧的弹出式菜单里选择Add Panel：
2.  Grafana可以支持多重类型的视图。对于PV，UV数据，在标签栏中单击**Graph**，创建一个Graph视图。
3.  单击**Pannel Title**，在弹出的窗口中单击**Edit**。
4.  在Metrics配置中，选择datasource为`logservice`，输入Query，Y轴和X轴：

     ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13155/5858_zh-CN.png "配置PV、UV") 

5.  dataSource下拉框中选择之前配置的`logservice`。

    |配置项|配置内容|
    |:--|:---|
    |Query|    ```
$hostname| select approx_distinct(remote_addr) as uv ,count(1) as pv , __time__ -
                  __time__ % $$myinterval as time group by time order by time limit
                1000
    ```

上述Query中的$hostname，在实际展示时，会替换成用户选择的域名。$$myinterval，则会替换成时间区间，注意myinterval前有两个$符号，而hostname有一个。|
    |X-Column|time|
    |Y-Column|uv,pv|

    UV和PV的值相差比较大，需要用两个Y轴来展示，在图标下方，单击uv左侧有颜色的线，可以选择uv是在左Y轴显示，还是在右Y轴显示：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13155/5859_zh-CN.png "Y轴显示")

    视图需要标题，默认是Panel Title。如需修改，请打开General页签，在**Title**配置项中输入新的标题，例如`PV&UV`。


## 4.3 配置出入网带宽 {#section_yqs_2zp_12b .section}

您可以参考[4.2 配置PV、UV](#section_ccp_vyp_12b)，使用同样的方法添加出入网带宽的流量。

主要配置项如下：

|配置项|配置内容|
|:--|:---|
|Query| ```
$hostname | select sum(body_byte_sent) as net_out, sum(request_length) as net_in
              ,__time__ - __time__ % $$myinterval as time group by __time__ - __time__ %
              $$myinterval limit 10000
```

 |
|X-Column|Time|
|Y-Column|net\_in,net\_out|

## 4.4 不同HTTP方法的占比 {#section_zsj_hzp_12b .section}

您可以参考[4.2 配置PV、UV](#section_ccp_vyp_12b)，使用同样的方法配置不同HTTP方法的占比的饼图。

新建一个Row，视图选择**Pie Chart**，并在配置中输入Query，和x，y轴。

主要配置项如下：

|配置项|配置内容|
|:--|:---|
|Query| ```
$hostname | select count(1) as pv ,method group by method
```

 |
|X-Column|pie|
|Y-Column|method,pv|

## 4.5 不同HTTP状态码占比 {#section_azy_jzp_12b .section}

您可以参考[4.2 配置PV、UV](#section_ccp_vyp_12b)，使用同样的方法配置不同HTTP状态码占比的饼图。

新建一个Row，视图选择**Pie Chart**。

主要配置项如下：

|配置项|配置内容|
|:--|:---|
|Query| ```
$hostname | select count(1) as pv ,status group by status
```

 |
|X-Column|pie|
|Y-Column|status,pv|

## 4.6 热门来源页面 {#section_hzq_lzp_12b .section}

您可以参考[4.2 配置PV、UV](#section_ccp_vyp_12b)，使用同样的方法配置热门来源页面的饼图。

新建一个Row，视图选择**Pie Chart**

主要配置项如下：

|配置项|配置内容|
|:--|:---|
|Query| ```
$hostname | select count(1) as pv , referer group by referer order by pv
            desc
```

 |
|X-Column|pie|
|Y-Column|referer,pv|

## 4.7 延时最高页面 {#section_txv_mzp_12b .section}

您可以参考[4.2 配置PV、UV](#section_ccp_vyp_12b)，使用同样的方法配置延时最高页面的视图。

为了以表格形式展示Url和对应的延时，请在创建时，指定**Table**视图。

主要配置项如下：

|配置项|配置内容|
|:--|:---|
|Query| ```
$hostname | select url as top_latency_url ,request_time order by request_time desc
              limit 10
```

 |
|X-Column|X-Column不填写内容|
|Y-Column|top\_latency\_url,request\_time|

## 4.8 热门页面 {#section_ur2_4zp_12b .section}

您可以参考[4.2 配置PV、UV](#section_ccp_vyp_12b)，使用同样的方法配置热门页面的视图。

新建一个表格视图。Data Source选择Logservice，Query内容、X轴和Y轴请参考下表。

|配置项|配置内容|
|:--|:---|
|Query| ```
$hostname | select count(1) as pv, split_part(url,'?',1) as path group by
              split_part(url,'?',1) order by pv desc limit 20
```

 |
|X-Column|X-Column不填写内容|
|Y-Column|path,pv|

## 4.9 非200请求top页面 {#section_u3c_pzp_12b .section}

您可以参考[4.2 配置PV、UV](#section_ccp_vyp_12b)，使用同样的方法配置非200请求top页面的视图。

新建一个表格视图。Data Source选择Logservice，Query内容、X轴和Y轴请参考下表。

|配置项|配置内容|
|:--|:---|
|Query| ```
$hostname not status:200| select count(1) as pv , url group by url order by pv
              desc
```

 |
|X-Column|X-Column不填写内容|
|Y-Column|url,pv|

## 4.10 前后端平均延时 {#section_fzc_qzp_12b .section}

您可以参考[4.2 配置PV、UV](#section_ccp_vyp_12b)，使用同样的方法配置前后端平均延时的视图。

新建一个Graph视图。Data Source选择Logservice，Query内容、X轴和Y轴请参考下表。

|配置项|配置内容|
|:--|:---|
|Query| ```
$hostname | select avg(request_time) as response_time, avg(upstream_response_time) as
              upstream_response_time ,__time__ - __time__ % $$myinterval as time group by __time__ -
              __time__ % $$myinterval limit 10000
```

 |
|X-Column|time|
|Y-Column|upstream\_response\_time,response\_time|

## 4.11 客户端统计 {#section_sn1_rzp_12b .section}

您可以参考[4.2 配置PV、UV](#section_ccp_vyp_12b)，使用同样的方法配置客户端统计的视图。

新建一个饼图。Data Source选择Logservice，Query内容、X轴和Y轴请参考下表。

|配置项|配置内容|
|:--|:---|
|Query| ```
$hostname | select count(1) as pv, case when http_user_agent like '%Android%' then
              'Android' when http_user_agent like '%iPhone%' then 'iOS' else 'unKnown' end as
              http_user_agent group by case when http_user_agent like '%Android%' then 'Android'
              when http_user_agent like '%iPhone%' then 'iOS' else 'unKnown' end order by pv desc
              limit 10
```

 |
|X-Column|pie|
|Y-Column|http\_user\_agent,pv|

## 4.12 保存和发布Dashboard {#section_tlk_tzp_12b .section}

单击页面上方的保存按钮，发布Dashboard。

## 5 查看结果 {#section_nyj_5zp_12b .section}

打开Dashboard首页查看效果。示例请参见[Demo](http://47.96.36.117:3000/dashboard/db/nginxfang-wen-tong-ji?orgId=1)。

您可以在页面上方选择统计的时间范围，也可以选择统计的时间粒度或不同的域名。

这样整个Nginx访问统计的Dashboard就完成了，您可以从视图中挖掘有价值的信息。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13155/5860_zh-CN.png "查看结果")

