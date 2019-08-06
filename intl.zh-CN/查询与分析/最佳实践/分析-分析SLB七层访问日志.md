# 分析-分析SLB七层访问日志 {#concept_188630 .concept}

阿里云SLB是对多台云服务器进行流量分发的负载均衡服务，可以通过流量分发扩展应用系统对外的服务能力，通过消除单点故障提升应用系统的可用性。负载均衡对于大部分云上架构来说都是基础设施组件，因此，对SLB持续的监控、探测、诊断和报告是一个强需求。用户一般可以通过云厂商内置的监控报表来了解SLB实例运行状况。

SLB访问日志功能当前支持基于HTTP/HTTPS的七层负载均衡，访问日志内容丰富，提供近30个字段，例如：收到请求的时间、客户端的IP地址、处理Latency、请求URI、后端RealServer（阿里云ECS）地址、返回状态码等。完整字段及功能说明请参考负载均衡7层访问日志功能。

本文档基于阿里云日志服务的可视化和日志实时查询（OLTP+OLAP）能力，为您介绍SLB七层访问日志的实时采集、查询与分析最佳方案，并举例说明SLB实例的一些典型报表统计、日志查询分析的方法。该方案结合了可视化报表和查询分析引擎，可以实时、交互分析SLB实例状况。

目前SLB7层访问日志已在所有区域开放，欢迎使用。

## 前提条件 {#section_u94_l3a_5o0 .section}

1.  已开通日志服务与SLB七层负载均衡。
2.  已成功采集到SLB七层负载均衡日志。详细步骤请参考[采集步骤](../../../../intl.zh-CN/数据采集/云产品采集/负载均衡7层访问日志.md#)。

## 可视化分析 {#section_yy6_phk_d4o .section}

**业务概览**

负载均衡支持RealServer的水平扩展和故障冗余恢复，为应用提供大规模、高可靠的并发web访问服务支撑。

**说明：** 典型的概览指标包括：

-   PV：client（请求源IP）发起的HTTP\(S\)请求次数。
-   UV：对于相同client IP只计算一次，合计的总体请求次数。
-   请求成功率：状态码为2XX的请求次数占总体PV的比例。
-   请求报文流量：客户端请求报文长度（request\_length字段）的总和。
-   返回客户端流量：SLB返回给客户端的HTTP body的字节数（body\_bytes\_sent字段）总和。
-   请求的热点分布：计算client IP的地理位置，按照每个地理位置来统计每个区域的PV情况。

-   查看用户请求的来源地区。

    如下图所示，通过地图可以看到用户请求主要来自珠三角和长三角区域。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/162629/156507255345387_zh-CN.png)

-   在日志服务的Dashboard中，通过添加过滤条件可以在当前图表中筛选符合条件的数据指标来展示，例如：client IP维度、SLB实例ID维度。

    例如，查询一个指定SLB实例ID的PV、UV随时间的变化趋势。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/162629/156507255345388_zh-CN.png)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/162629/156507255345389_zh-CN.png)


**请求调度分析**

从客户端过来的流量会先被SLB处理，并分发到多台RealServer的一台上做实际的业务逻辑处理。SLB可以检测不健康的机器并重新分配流量到其它正常服务的RealServer上，等异常机器恢复后再将流量重新加上去，这个过程是自动完成的。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/162629/156507255345392_zh-CN.png)

对SLB实例添加一个监听，监听可以设置三种调度方法：轮询、加权轮询（WRR）和加权最小连接数（WLC）。

-   轮询：按照访问次数依次将外部请求依序分发到后端ECS上。
-   加权轮询：您可以对每台后端服务器设置权重值，权重值越高的服务器，被轮询到的次数（概率）也越高。
-   加权最小连接数：除了根据每台后端服务器设定的权重值来进行轮询，同时还考虑后端服务器的实际负载（即连接数）。当权重值相同时，当前连接数越小的后端服务器被轮询到的次数（概率）也越高。

例如，`172.19.39.**`机器同时兼有跳板机职能，其性能是其它三台机器的4倍，这里为它设置权重100，其余设置权重为20。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/162629/156507255445394_zh-CN.png)

基于实例的访问日志，通过如下一条查询语句可以完成和两个维度的流量聚合：

``` {#codeblock_9hj_f6t_dqa}
* | select COALESCE(client_ip, vip_addr, upstream_addr) as source, COALESCE(upstream_addr, vip_addr, client_ip) as dest, sum(request_length) as inflow group by grouping sets( (client_ip, vip_addr), (vip_addr, upstream_addr))
```

结合桑基图对SQL查询结果做vip维度的聚合可视化，最终得到请求报文流量拓扑图。多个client IP向SLB vip（`172.19.0.**`）发起请求，请求报文流量基本遵循20:20:20:100比例转发到后端的RealServer进行处理。桑基图清晰地表述了每台RealServer的负载情况。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/162629/156507255445395_zh-CN.png)

**流量与Latency分析**

按照1分钟时间维度对流量与latency指标做聚合计算：

-   request\_length、body\_bytes\_sent统计

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/162629/156507255445404_zh-CN.png)

-   request\_time、upstream\_response\_time统计

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/162629/156507255445405_zh-CN.png)

-   高延迟RealServer统计

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/162629/156507255445406_zh-CN.png)


**用户请求概览**

根据请求的方法、协议、状态码等维度分析访问日志中HTTP\(S\)请求。

指定时间段、状态码快速定位RealServer的需求，通过日志服务的查询分析功能可以迅速得到结果：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/162629/156507255545408_zh-CN.png)

1.  在一个时间段内，请求方法维度上可以做PV分布统计。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/162629/156507255545409_zh-CN.png)

2.  加上时间属性，同时在时间、请求方法两个维度上可以统计出各方法的PV趋势。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/162629/156507255545410_zh-CN.png)

3.  请求响应状态码分布可以展示服务的基本状况，如果大量的500状态码则意味着后端RealServer的应用程序在发生内部错误。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/162629/156507255545411_zh-CN.png)

4.  围绕着每一个状态码，可以查看其随时间变化趋势。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/162629/156507255645412_zh-CN.png)


**请求源分析**

对client的IP做计算可以得到每条请求的发起地理位置（国家、省份、城市）、电信运营商信息。

1.  对用户请求IP的运营商做PV分布图。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/162629/156507255645413_zh-CN.png)

2.  按照请求PV降序，对client IP做统计可以展示大用户请求的具体来源。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/162629/156507255645414_zh-CN.png)

3.  查看http\_user\_agent。

    用户代理（http\_user\_agent）也是常常需要关注的对象，可以据此区分出谁在访问我们的网站或服务。比如搜索引擎会使用爬虫机器人扫描或下载网站资源，一般情况下的低频爬虫访问可以让搜索引擎及时更新网站内容、有助于网站的推广和SEO。但如果高PV的请求都来自于爬虫，则可能对服务的性能和机器资源造成浪费，需要实时监控高PV请求状况并采取手段来控制影响。

    访问日志中根据查询SLB ID或应用host、http\_user\_agent关键词，可以很快检索出相关记录。下图是一条搜狗爬虫程序的GET请求日志，请求很稀疏，对于应用无影响。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/162629/156507255645419_zh-CN.png)


**运营概览**

SLB访问日志在运营同学手里同样发挥着重要作用，可以基于日志分析出来流量模式，进而辅助业务决策。

1.  查看PV的热点分布。

    在地理维度上，通过PV的热点分布，可以清晰了解到我们服务的重点客户在哪里，PV低的区域可能需要再推广加强。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/162629/156507255745420_zh-CN.png)

2.  查看host/URI。

    对于一个网站而言，通过分析访客的行为可以为网站内容建设提供有力的参考。哪些内容好，哪些内容不好？这个问题可以用头部、尾部PV的host/URI来回答。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/162629/156507255745421_zh-CN.png)

3.  查看request\_uri。

    对于热门资源（访问日志的request\_uri字段），可以关注一下日志详情的http\_referer字段，查看网站的请求来源。好的导流入口需要持续加强，对于盗链行为则需要想办法克制。下图是在日志查询页面中找到一条来自百度图片的跳转请求。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/162629/156507255745422_zh-CN.png)


