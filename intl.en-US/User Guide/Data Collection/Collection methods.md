# Collection methods {#concept_ikm_ql5_vdb .concept}

LogHub supports a variety of RESTful APIs that provide different log collection methods, for example, log collection through one or more clients, websites, protocols, SDKs, and APIs.

## Data sources {#section_yny_rh4_vgb .section}

Log Service can collect logs from the following sources:

|Type|Source|Access method|Details|
|:---|:-----|:------------|:------|
|Application|Program output|[Logtail](reseller.en-US/User Guide/Logtail collection/Overview/Overview.md)|-|
|Access logs|[Logtail](reseller.en-US/User Guide/Logtail collection/Overview/Overview.md)|[../../../../../dita-oss-bucket/SP\_7/DNSLS11878107/EN-US\_TP\_13019.md](../../../../../reseller.en-US/Quick Start/Analysis - Nginx access logs.md)|
|Link track|Jaeger Collector and [Logtail](reseller.en-US/User Guide/Logtail collection/Overview/Overview.md)|-|
|Programming language|Java|[SDK](../../../../../reseller.en-US/SDK Reference/Basic Descriptions /Overview.md) and [Java Producer Library](reseller.en-US/User Guide/Other collection methods/SDK collection/Producer Library.md)|-|
|Log4J Appender|[1.x](https://github.com/aliyun/aliyun-log-log4j-appender) and [2.x](https://github.com/aliyun/aliyun-log-log4j2-appender)|-|
|LogBack Appender|[LogBack](https://github.com/aliyun/aliyun-log-logback-appender)|-|
|C|[Native](../../../../../reseller.en-US/SDK Reference/Basic Descriptions /Overview.md)|-|
|Python|[Python](../../../../../reseller.en-US/SDK Reference/Basic Descriptions /Overview.md)|-|
|Python Logging|[Python Logging Handler](https://aliyun-log-python-sdk.readthedocs.io/tutorials/tutorial_logging_handler.html)|-|
|PHP|[PHP](../../../../../reseller.en-US/SDK Reference/Basic Descriptions /Overview.md)|-|
|C\#|[C\#](../../../../../reseller.en-US/SDK Reference/Basic Descriptions /Overview.md)|-|
|C++|[C++ SDK](../../../../../reseller.en-US/SDK Reference/Basic Descriptions /Overview.md)|-|
|Go|[Go](../../../../../reseller.en-US/SDK Reference/Basic Descriptions /Overview.md)|-|
|NodeJS|[NodeJs](https://github.com/aliyun-UED/aliyun-sdk-js)|-|
|JS|[JS/Web Tracking](reseller.en-US/User Guide/Other collection methods/Web Tracking.md)|-|
|OS|Linux|[Logtail](reseller.en-US/User Guide/Logtail collection/Overview/Overview.md)|-|
|Windows|[Logtail](reseller.en-US/User Guide/Logtail collection/Overview/Overview.md)|-|
|Mac/Unix|[Native C](../../../../../reseller.en-US/SDK Reference/Basic Descriptions /Overview.md)|-|
|Docker files|[Logtail file collection](reseller.en-US/User Guide/Logtail collection/Container log collection/Container text logs.md)|-|
|Docker output|[Logtail container stdout](reseller.en-US/User Guide/Logtail collection/Container log collection/Container stdout.md)|-|
|Mobile client|iOS/Android|[../../../../../dita-oss-bucket/SP\_7/DNSLS11879553/EN-US\_TP\_13288.md\#](../../../../../reseller.en-US/SDK Reference/iOS SDK.md#) and [../../../../../dita-oss-bucket/SP\_7/DNSLS11879553/EN-US\_TP\_13285.md](../../../../../reseller.en-US/SDK Reference/Android SDK.md)|-|
|Websites|[JS/Web Tracking](reseller.en-US/User Guide/Other collection methods/Web Tracking.md)|-|
|Intelligent IoT|[C Producer Library](https://github.com/aliyun/aliyun-log-c-sdk)|-|
|Cloud product|Various products, such as ECS and OSSFor more information, see [Cloud product logs](#)

|Cloud product console|[Cloud product logs](reseller.en-US/User Guide/Cloud product collection/Cloud product logs.md)|
|Third-party software|Logstash|[Logstash](reseller.en-US/User Guide/Other collection methods/Logstash/Create Logstash collection configurations.md)|-|

The following table lists the cloud products from which Log Service can collect logs:

|Type|Cloud product name|Activation method|Details|
|:---|:-----------------|:----------------|:------|
|Elastic computing|Elastic Compute Service \(ECS\)|Through Logtail installation|[Logtail introduction](reseller.en-US/User Guide/Logtail collection/Overview/Overview.md)|
|Container Service/Container Service for Kubernetes|Through the Container Service console|[Text logs](reseller.en-US/User Guide/Logtail collection/Container log collection/Container text logs.md) and [stdout](reseller.en-US/User Guide/Logtail collection/Container log collection/Container stdout.md)|
|Storage|Object Storage Service \(OSS\)|Through the OSS console|[OSS access logs](reseller.en-US//OSS access logs.md)|
|Network|Server Load Balancer \(SLB\)|Through the SLB console|[Access logs of Layer-7 SLB](reseller.en-US/User Guide/Cloud product collection/Access logs of Layer-7 Server Load Balancer.md)|
|Virtual Private Cloud \(VPC\)|Through the VPC console|[Flow logs](../../../../../reseller.en-US/User Guide/Flow logs.md)|
|API Gateway|Through the API Gateway console|[API Gateway access logs](reseller.en-US/User Guide/Cloud product collection/API Gateway Access Log.md)|
|Security|ActionTrail|Through the ActionTrail console|[ActionTrail overview](reseller.en-US/User Guide/Cloud product collection/ActionTrail access logs/Overview.md)|
|DDoS Protection|Through the DDoS Protection console|[DDOS Protection overview](reseller.en-US/User Guide/Cloud product collection/DDoS log collection/Overview.md)|
|Threat Detection Service|Purchase Threat Detection Service Enterprise Edition and activate the service in the Threat Detection Service console.|[Log retrieval](../../../../../reseller.en-US/User Guide/Log retrieval/Log retrieval.md)|
|Anti-Bot Service|Through the Anti-Bot Service console|[Anti-Bot Service logs](~~100510~~) |
|Application|Log Service \(LOG\)|Through the Log Service console|[Log Service overview](reseller.en-US/User Guide/Log Service Monitor/Service log/Service log overview.md)|

## Network and access point selection {#section_m2v_rl5_vdb .section}

Log Service provides [service endpoints](../../../../../reseller.en-US/API Reference/Service endpoint.md) in each region, and the following types of network access methods are supported:

-   \(**Recommended**\) Intranet \(classic networks\) and private networks \(VPCs\): are applicable to regions with smooth service access and high-quality bandwidth links.
-   Internet \(classic networks\): can be used without any limits. The access speed depends on the link quality. HTTPS is recommended to maintain access security.

## FAQ {#section_sdp_zpc_wgb .section}

-   Q: **Which type of network applies to physical connections?**

    A: Intranet/private networks

-   Q: **Can Internet IP addresses be collected during Internet data collection?**

    A: Yes. You can follow the instructions provided in [EN-US\_TP\_13024.md\#](reseller.en-US/User Guide/Preparation/Manage a Logstore.md#) and enable the Internet IP address recording function.

-   Q: **Which type of network can I use if I want to collect logs from an ECS server located in region A and send them to a project on a Log Service server located in region B?**

    A: Use the Internet to transfer logs after install the Internet-version Logtail on the ECS server. As for other scenarios, follow the instructions provided in [EN-US\_TP\_18799.md](reseller.en-US/User Guide/Logtail collection/Select a network type.md).

-   Q: **How can I determine whether access is established successfully?**

    A: Access is established successfully if information is returned after you run the following command:

    ```
     curl $myproject.cn-hangzhou.log.aliyuncs.com
    ```

    In this command, `$myproject` indicates the project name, and `cn-hangzhou.log.aliuncs.com` indicates the access point.


