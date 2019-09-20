# Analyze access logs of Layer-7 Server Load Balancer {#concept_188630 .concept}

Alibaba Cloud Server Load Balancer \(SLB\) distributes traffic among multiple instances to improve the servicing capabilities of your applications. You can use SLB to prevent single point of failures \(SPOFs\) and improve the availability and the fault tolerance capability of your applications. SLB is an infrastructure component for most services in the cloud architecture. It is required to monitor, detect, diagnose, and generate reports for SLB in a continuous way. You can learn about the running status of SLB instances through the monitoring reports provided by cloud service providers.

Currently, access logs can be generated for HTTP- or HTTPS-based Layer-7 SLB. The access log can contain about 30 columns such as the time when the request is received, the IP address of the client, processing latency, request URI, back-end RealServer \(Alibaba Cloud ECS instance\) address, and returned status code. For more information about the columns and features, see Access logs of Layer-7 Server Load Balancer.

This topic describes the optimal scheme for real-time collection, query, and analysis of Layer-7 SLB access logs based on the visualization and real-time query and analysis \(OLTP and OLAP\) features of Log Service. This topic also describes some typical methods of report statistics and log query and analysis for SLB instances. The scheme combines visual reports and query and analysis engines to analyze the status of SLB instances in real time and interactively.

Currently, Layer-7 SLB access logs are available in all regions.

## Prerequisites {#section_u94_l3a_5o0 .section}

1.  Log Service and Layer-7 SLB are activated.
2.  Layer-7 SLB logs are collected. For more information about how to collect the logs, see [Access logs of Layer-7 Server Load Balancer](../../../../reseller.en-US/Data Collection/Cloud product collection/Access logs of Layer-7 Server Load Balancer.md#).

## Visual analysis {#section_yy6_phk_d4o .section}

**Business overview**

SLB supports horizontal scaling of RealServers and fault redundancy recovery, supporting the processing of large-scale concurrent web access requests and guaranteeing high reliability for applications.

**Note:** Typical business overview metrics are as follows:

-   PV: the number of the HTTP or HTTPS requests sent by the client \(the IP address of the request source\).
-   UV: the total number of the requests. The requests from the same client IP address are counted as one request.
-   Request success rate: the percentage of the requests whose status code is 2XX to the total PVs.
-   Request traffic: the sum of the length of the requests \(specified by the request\_length field\).
-   Response traffic: the total number of bytes in the HTTP response body \(specified by the body\_bytes\_sent field\) returned from SLB to the client.
-   PV heat map of requests: displays the density of PVs in the regions where the IP addresses of the clients reside.

-   View the regions from which the requests are sent.

    In this example, most requests are sent from the Pearl River Delta and the Yangtze River Delta, as shown in the following figure.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/162629/156894733045387_en-US.png)

-   In the dashboard of Log Service, you can add filter conditions and display the data that meets the filter conditions, such as the IP address of the client and the ID of the SLB instance, in the current chart.

    For example, you can view the trend of the PVs and UVs of a specified SLB instance over time.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/162629/156894733045389_en-US.png)


**Request scheduling analysis**

The requests sent from clients are first processed by SLB and then dispatched to one of multiple RealServers for processing based on the business logic. SLB can detect unhealthy RealServers and dispatch traffic to other RealServers in a normal state. After the unhealthy RealServers enter a normal state, the traffic continues to be dispatched to these RealServers. The whole process is automatic.Add a listener to the SLB instance. The following three scheduling methods are available to the listener: round robin, weighted round robin \(WRR\), and weighted least connections \(WLC\).

-   Round robin: External requests are sequentially distributed to back-end ECS instances based on the number of visits.
-   WRR: You can set the weight for each back-end RealServer. RealServers with higher weights receive more requests than those with smaller weights.
-   WLC: Requests are forwarded based on the weight and load \(the number of connections\) of each back-end RealServer. If the weights are the same, back-end RealServers with fewer connections receive more requests than those with more connections.

For example, the RealServer whose IP address is `172.19.39. **` is also a jump server. Its performance is four times that of the other three RealServers. If you set the weight of this RealServer to 100, the weight of the other three RealServers is set to 20.Run the following statement to aggregate the traffic of two dimensions based on the access logs of the instance:

``` {#codeblock_9hj_f6t_dqa}
* | select COALESCE(client_ip, vip_addr, upstream_addr) as source, COALESCE(upstream_addr, vip_addr, client_ip) as dest, sum(request_length) as inflow group by grouping sets( (client_ip, vip_addr), (vip_addr, upstream_addr))
```

Based on the Sankey diagram, you can obtain a request traffic topology by aggregating and visualizing the result returned by the SQL statement from the virtual IP address dimension. If requests are sent from multiple client IP addresses to the SLB virtual IP address \(`172.19.0. **`\), the request traffic is dispatched to the back-end RealServers in the 20:20:20:100 proportion. The Sankey diagram clearly shows the load of each RealServer.

**Traffic and latency analysis**

Perform aggregate computing on the traffic and latency in each minute.

-   Statistics from the request\_length and body\_bytes\_sent dimensions

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/162629/156894733045404_en-US.png)

-   Statistics from the request\_time and upstream\_response\_time dimensions

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/162629/156894733145405_en-US.png)


**Request overview**

You can analyze the HTTP or HTTPS requests in access logs from multiple dimensions, such as request methods, protocols, and status codes.

You can select a time range and specify the status code to locate a RealServer in the logs based on the query and analysis features of Log Service, as shown in the following figure.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/162629/156894733145408_en-US.png)

1.  You can collect statistics about the PVs for different request methods.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/162629/156894733145409_en-US.png)

2.  With the time dimension, you can view the trend lines of PVs for different request methods over time.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/162629/156894733145410_en-US.png)

3.  You can learn about the service running status from the proportion of the requests with different status codes. If there are a lot of requests with the status code 500, internal errors occur with the applications of the back-end RealServers.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/162629/156894733145411_en-US.png)

4.  You can view the trend lines of PVs for each status code.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/162629/156894733145412_en-US.png)


**Request source analysis**

You can obtain the geographic location \(country, province, or city\) and telecom operator information by analyzing the IP address of the client.

1.  You can make a PV distribution chart for the requests from the IP addresses of the different operators.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/162629/156894733145413_en-US.png)

2.  You can view the request sources in descending order of the PVs by analyzing the client IP addresses.
3.  You can view the user agent information.

    The user agent \(http\_user\_agent\) is a common metric that can be used to identify who is visiting the website or service. For example, a search engine uses web crawlers to scan or download website resources. In general, infrequent crawler access allows the search engine to update website content in a timely manner and facilitates website promotion and search engine optimization \(SEO\). If the requests with high PVs are sent from the web crawlers, the service performance and server resources may be wasted. Therefore, you need to monitor the requests with high PVs and take measures to avoid wastes.

    You can search the access logs for related records based on the SLB instance ID, application host, and user agent. The following figure shows the log of GET requests sent from the Sogou web crawler. The requests are not frequent and have no adverse impact on applications.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/162629/156894733145419_en-US.png)


**Operations overview**

SLB access logs are vital to the marketing team, who can use the access logs for traffic analysis to make business plans.

1.  View the PV distribution of different regions.

    By viewing the PVs of different regions in the geographical dimension, you can know the areas where the key customers of the services are, and the areas with low PVs that may need further promotion.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/162629/156894733145420_en-US.png)

2.  View the host and URI of the requests.

    For a website, analyzing visitor behavior can provide a powerful reference for website content construction. Whether the content is good or poor can be told by the host or the URI of the top websites with the highest PVs or with the lowest PVs.

3.  View the request URI.

    For popular resources \(specified by the request\_uri field in access logs\), you can pay attention to the http\_referer field in the log details to view the request source of the website. Strengthen good request sources and enable hotlinking protection. The following figure shows the log details of a page redirection request from Baidu Image.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/162629/156894733145422_en-US.png)


