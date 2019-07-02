# Overview {#concept_u4v_j54_p2b .concept}

In addition to Virtual Private Cloud \(VPC\) and the Internet, Log Service adds a network type of **Internet-based Global Acceleration**. Compared with the ordinary Internet access, Internet-based Global Acceleration has significant advantages in terms of latency and stability. It is suitable for scenarios with high requirements for data collection, low consumption latency, and reliability. Global Acceleration for Log Service depends on the acceleration environment provided by Alibaba Cloud [Dynamic Route for CDN](https://www.aliyun.com/product/dcdn). Cross-carrier access, network instability, burst traffic, and network congestion used to cause problems such as slow response, packet loss, and unstable services. In this acceleration environment, Alibaba Cloud has solved such problems to improve overall performance and user experience.

Global Acceleration for Log Service is based on Alibaba Cloud Content Delivery Network \(CDN\) hardware resources. Alibaba Cloud has optimized the stability of log collection and data transmission from various forms of data sources, such as mobile phones, Internet of Things \(IoT\) devices, smart devices, on-premises Internet Data Centers \(IDCs\), and other cloud servers.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16815/15620499288060_en-US.png)

## Technical principles {#section_qjb_cjm_q2b .section}

Global Acceleration for Log Service is based on Alibaba Cloud CDN hardware resources. Your terminals \(such as mobile phones, IoT devices, smart devices, on-premises IDCs, and other cloud servers\) can access the nearest edge node of Alibaba Cloud CDN all over the world and be routed to Log Service through the inner high-speed channels of CDN. Compared with data transmission on the Internet, this method can greatly reduce the network latency and jitter.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16815/15620499298061_en-US.png)

The preceding figure shows the flowchart for processing Global Acceleration requests for Log Service. The overall process is described as follows:

1.  Before sending requests for log upload or log download to the accelerating domain name `your-project.log-global.aliyuncs.com` of Log Service, the client needs to send a domain name resolution request to the public DNS.
2.  The public DNS resolves the domain name `your-project.log-global.aliyuncs.com` into the CNAME `your-project.log-global.aliyuncs.com.w.kunlungr.com`. The domain name resolution request is then forwarded to the CNAME in Alibaba Cloud CDN.
3.  Based on the intelligent scheduling system, Alibaba Cloud CDN returns the IP address of the optimal edge node to the public DNS.
4.  The public DNS returns the resolved IP address to the client.
5.  The client sends a request to the server based on the obtained IP address.
6.  After receiving the request, the CDN edge node uses dynamic routing and a private transport protocol to route the request to the node nearest to the Log Service server. The request is then forwarded to Log Service.
7.  After receiving the request from the CDN edge node, the Log Service server returns the result of the request to the CDN edge node.
8.  CDN transparently transmits the result or data returned by Log Service to the client.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16815/15620499298062_en-US.png)

## Billing methods {#section_ujb_cjm_q2b .section}

Global Acceleration costs for Log Service include:

-   Cost for accessing Log Service

    Log Service charges the access in **pay-as-you-go** mode, which is the same as Internet access. Log Service also provides certain **FreeTier quota**. For more information, see [Billing method](../../../../intl.en-US/Pricing/Billing method.md).

-   Service cost for Dynamic Route for CDN

    For more information, see [the pricing of Dynamic Route for CDN](https://www.aliyun.com/price/product?spm=5176.197032.1035646.btn4.10725df1wi3RBN#/dcdn/detail).


## Scenarios {#section_w32_jlm_q2b .section}

-   Advertising

    Log data about ad views and clicks is crucial to the billing of ads. In addition, advertising carriers are distributed all over the world, including mobile terminals, HTML 5 pages, and PCs. In some remote areas, data transmission is less stable on the Internet and logs may be lost during transmission. In this scenario, Global Acceleration for Log Service can provide a more stable and reliable channel for you to upload logs.

-   Online game

    The online game industry raises high requirements for the performance and stability of data collection from various sources, such as the official website, logon service, sales service, and game service. In scenarios where data is collected from mobile games or transmitted from globalized games, timely and stable data collection is hard to be guaranteed. We recommend that you use Global Acceleration for Log Service to resolve the preceding issues.

-   Finance

    Financial applications require a highly available and secured network. Audit logs of each transaction and each user operation must be collected securely and reliably on the server. At present, mobile transactions are popular, such as online banking, credit card malls, and mobile securities. HTTPS Global Acceleration for Log Service can provide a secure, fast, and stable channel for you to collect logs for such transactions.

-   IoT

    IoT devices and smart devices \(such as smart speakers and smart watches\) send collected sensor data, operations logs, critical system logs, and other data to the server for data analysis. These devices are usually distributed all over the world. The surrounding network is not always reliable. To collect logs stably and reliably, we recommend that you use Global Acceleration for Log Service.


## Acceleration effects {#section_ckb_cjm_q2b .section}

|Region|Latency in ms \(Internet\)|Latency in ms \(Global Acceleration\)|Percentage of timed-out requests \(Internet\)|Percentage of timed-out requests \(Global Acceleration\)|
|:-----|:-------------------------|:------------------------------------|:--------------------------------------------|:-------------------------------------------------------|
|Hangzhou|152.881|128.501|0.0|0.0|
|Europe|1750.738|614.227|0.5908|0.0|
|United States|736.614|458.340|0.0010|0.0|
|Singapore|567.287|277.897|0.0024|0.0|
|Middle East|2849.070|444.523|1.0168|0.0|
|Australia|1491.864|538.403|0.014|0.0|

The test environment is as follows:

-   Region of Log Service: China \(Hohhot\)
-   Average upload packet size: 10 KB
-   Test time range: one day \(average\)
-   Request method: HTTPS
-   Request server: Alibaba Cloud Elastic Compute Service \(ECS\) \(instance type: 1 vCPU 1 GiB\)

**Note:** The acceleration effects are for reference only.

