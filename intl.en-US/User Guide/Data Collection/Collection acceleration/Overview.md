# Overview {#concept_u4v_j54_p2b .concept}

Log Service adds a network type of **Global Acceleration Public Network** on the basis of Virtual Private Cloud \(VPC\) and public network. Compared with the ordinary public network access, Global Acceleration Public Network has significant advantages in terms of delay and stability, and is suitable for scenarios with high demands for data collection, low consumption delay, and reliability. Global Acceleration for Log Service depends on the acceleration environment provided by Alibaba Cloud [Dynamic Route for CDN](https://www.aliyun.com/product/dcdn) products. This function improves overall site performance and user experience by solving problems of slow response, packet loss, and unstable services. These problems are caused by factors such as cross-carriers access, network instability, traffic spikes, and network congestion.

Global Acceleration for Log Service is based on Alibaba Cloud Content Delivery Network \(CDN\) hardware resources, and optimizes the stability of log collection and data transmission from various forms of data sources such as mobile phones, Internet of Things \(IoT\) devices, smart devices, self-built Internet Data Centers \(IDCs\), and other cloud servers.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16815/15368945508060_en-US.png)

## Technical principles {#section_qjb_cjm_q2b .section}

Global Acceleration for Log Service is based on Alibaba Cloud CDN hardware resources. Your global access terminals \(such as mobile phones, IOT devices, smart devices, self-built IDCs, and other cloud servers\), access the nearest edge node of Alibaba Cloud CDN all over the world and route to Log Service through CDN inner high-speed channels. Compared with common public network transmission, network delay and jitter can be reduced greatly in this method.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16815/15368945518061_en-US.png)

The processing flow of Global Acceleration requests for Log Service is shown in the preceding figure. The overall flow is detailed as follows:

1.  The client needs to send a domain name resolution request to the public DNS before sending requests of log upload or log download to the Log Service acceleration domain name `your-project.log-global.aliyuncs.com`.
2.  The domain name at the public DNS `your-project.log-global.aliyuncs.com` points to the CNAME address `your-project.log-global.aliyuncs.com.w.kunlungr.com`. The domain name resolution is forwarded to the CNAME nodes of Alibaba Cloud CDN.
3.  Based on Alibaba Cloud CDN smart scheduling system, CNAME nodes return the IP address of the optimal CDN edge node to the public DNS.
4.  The public DNS returns the IP address finally resolved to the client.
5.  The client sends a request to the server based on the obtained IP address.
6.  After receiving the request, the CDN edge node routes the request to the node closest to the Log Service server based on the dynamic route lookup and private transport protocol. Finally, the request is forwarded to Log Service.
7.  After receiving the request from the CDN node, the server of Log Service returns the result of the request to the CDN node.
8.  CDN transparently transmits the result or data returned by Log Service to the client.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16815/15368945518062_en-US.png)

## Billing method  {#section_ujb_cjm_q2b .section}

Global Acceleration costs for Log Service include:

-   Costs for accessing Log Service

    Costs for accessing Log Service is the same as that in common public network. Log Service supports **Pay-As-You-Go** billing method, and provides **FreeTier quota**. For more information, see [Billing method](../../../../intl.en-US/Pricing/Billing method.md).

-   Service costs for Dynamic Route for CDN

    For information about cloud product costs of Dynamic Route for CDN, see [Billing Method of Dynamic Route for CDN](https://www.aliyun.com/price/product?spm=5176.197032.1035646.btn4.10725df1wi3RBN#/dcdn/detail).


## Scenarios {#section_w32_jlm_q2b .section}

-   Advertisement

    Log data about advertising browsing and clicking are extremely important for advertising billing. Advertising carriers include mobile terminal embedding, H5 pages, PC ends, and more all over the world. In some remote areas, the public network data transmission is less stable and risks of log loss exist. A more stable and reliable log upload channel can be obtained through Global Acceleration.

-   Online game

    The online game industry has high requirements on the performance and stability of data collection in the official website, logon service, sales service, game service, and other services. The timeliness and stability of data collection are hard to be guaranteed in the case of mobile game data collection and data back transmission from globalized games. We recommend that you use Global Acceleration for Log Service to solve the preceding issues.

-   Finance

    Financial-related applications require high availability and high security for network. Audit logs of each transaction and each user action must be collected securely and reliably to the server. At present, mobile transactions have become mainstream. For example, online banking, credit card malls, mobile securities, and other types of transactions can achieve secure, fast, and stable log collection by using HTTPS Global Acceleration for Log Service.

-   Internet of Things

    IoT devices and smart devices \(for example, smart speakers and smart watches\) collect sensor data, operation logs, critical system logs, and other data to the server for data analysis. These devices are usually distributed all over the world and the surrounding network is not always reliably. To achieve stable and reliable log collection, we recommend using Global Acceleration for Log Service.


## Acceleration effect {#section_ckb_cjm_q2b .section}

|Region|Delay ms \(common public network\)|Delay ms \(acceleration\)|Time-out ratio % \(common public network\)|Time-out ratio % \(acceleration\)|
|:-----|:---------------------------------|:------------------------|:-----------------------------------------|:--------------------------------|
|Hangzhou|152.881|128.501|0.0|0.0|
|Europe|1750.738|614.227|0.5908|0.0|
|USA|736.614|458.340|0.0010|0.0|
|Singapore|567.287|277.897|0.0024|0.0|
|Middle East|2849.070|444.523|1.0168|0.0|
|Australia|1491.864|538.403|0.014|0.0|

The test environment is as follows:

-   Region of Log service: North China 5 \(Hohhot\)
-   Average upload packet size: 10KB
-   Test time range: one day \(average\)
-   Request type: HTTPS
-   Request server: Alibaba Cloud ECS \(Specification 1C1GB\)

**Note:** The acceleration effect is for reference only.

