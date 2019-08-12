# Collection methods {#concept_ikm_ql5_vdb .concept}

LogHub supports a variety of RESTful APIs that provide different log collection methods, for example, log collection through one or more clients, websites, protocols, SDKs, and APIs.

## Data sources {#section_yny_rh4_vgb .section}

Log Service can collect logs from the following sources:

|Type|Source|Access method|Details|
|:---|:-----|:------------|:------|
|Application|Program output|[Logtail](reseller.en-US/Data Collection/Logtail collection/Overview/Overview.md)|-|
|Access logs|[Logtail](reseller.en-US/Data Collection/Logtail collection/Overview/Overview.md)|[Collect and analyze Nginx access logs](../../../../reseller.en-US/Quick Start/Collect and analyze Nginx access logs.md#)|
|Link track|Jaeger Collector and [Logtail](reseller.en-US/Data Collection/Logtail collection/Overview/Overview.md)|-|
|Programming language|Java|[SDK](../../../../reseller.en-US/SDK Reference/Basic Descriptions /Overview.md) and [Java Producer Library](reseller.en-US/Data Collection/Other collection methods/SDK collection/Producer Library.md)|-|
|Log4J Appender|[1.x](https://github.com/aliyun/aliyun-log-log4j-appender) and [2.x](https://github.com/aliyun/aliyun-log-log4j2-appender)|-|
|LogBack Appender|[LogBack](https://github.com/aliyun/aliyun-log-logback-appender)|-|
|C|[Native](../../../../reseller.en-US/SDK Reference/Basic Descriptions /Overview.md)|-|
|Python|[Python](../../../../reseller.en-US/SDK Reference/Basic Descriptions /Overview.md)|-|
|Python Logging|Python Logging Handler|-|
|PHP|[PHP](../../../../reseller.en-US/SDK Reference/Basic Descriptions /Overview.md)|-|
|C\#|[C\#](../../../../reseller.en-US/SDK Reference/Basic Descriptions /Overview.md)|-|
|C++|[C++ SDK](../../../../reseller.en-US/SDK Reference/Basic Descriptions /Overview.md)|-|
|Go|[Go](../../../../reseller.en-US/SDK Reference/Basic Descriptions /Overview.md)|-|
|NodeJS|[NodeJs](https://github.com/aliyun-UED/aliyun-sdk-js)|-|
|JS|[JS/Web Tracking](reseller.en-US/Data Collection/Other collection methods/Web Tracking.md)|-|
|OS|Linux|[Logtail](reseller.en-US/Data Collection/Logtail collection/Overview/Overview.md)|-|
|Windows|[Logtail](reseller.en-US/Data Collection/Logtail collection/Overview/Overview.md)|-|
|Mac/Unix|[Native C](../../../../reseller.en-US/SDK Reference/Basic Descriptions /Overview.md)|-|
|Docker files|[Logtail file collection](reseller.en-US/Data Collection/Logtail collection/Container log collection/Container text logs.md)|-|
|Docker output|[Logtail container stdout](reseller.en-US/Data Collection/Logtail collection/Container log collection/Container stdout.md)|-|
|Mobile client|iOS/Android|[iOS SDK](../../../../reseller.en-US/SDK Reference/IOS SDK.md#) and [Android SDK](../../../../reseller.en-US/SDK Reference/Android SDK.md)|-|
|Websites|[JS/Web Tracking](reseller.en-US/Data Collection/Other collection methods/Web Tracking.md)|-|
|Intelligent IoT|[C Producer Library](https://github.com/aliyun/aliyun-log-c-sdk)|-|
|Cloud product|Various products, such as ECS and OSS For more information, see [Cloud product logs](#)

 |Cloud product console|[Cloud product logs](reseller.en-US/Data Collection/Cloud product collection/Cloud service logs.md)|
|Import MaxCompute data|Use Dataworks to export MaxCompute data|[Use DataWorks to export MaxCompute data to Log Service](reseller.en-US/Data Collection/Other collection methods/Use DataWorks to export MaxCompute data to Log Service.md#)|
|Third-party software|Logstash|[Logstash](reseller.en-US/Data Collection/Other collection methods/Logstash/Create Logstash collection configurations.md)|-|

The following table lists the cloud products from which Log Service can collect logs:

 

## Network and access point selection {#section_m2v_rl5_vdb .section}

Log Service provides [service endpoints](../../../../reseller.en-US/API Reference/Service endpoint.md) in each region, and the following types of network access methods are supported:

-   \(**Recommended**\) Intranet \(classic networks\) and private networks \(VPCs\): are applicable to regions with smooth service access and high-quality bandwidth links.
-   Internet \(classic networks\): can be used without any limits. The access speed depends on the link quality. HTTPS is recommended to maintain access security.

## FAQ {#section_sdp_zpc_wgb .section}

-   Q: **Which type of network applies to physical connections?** 

    A: Intranet/private networks

-   Q: **Can Internet IP addresses be collected during Internet data collection?** 

    A: Yes. You can follow the instructions provided in [Manage a Logstore](reseller.en-US/Preparation/Manage a Logstore.md#) and enable the Internet IP address recording function.

-   Q: **Which type of network can I use if I want to collect logs from an ECS server located in region A and send them to a project on a Log Service server located in region B?** 

    A: Use the Internet to transfer logs after install the Internet-version Logtail on the ECS server. As for other scenarios, follow the instructions provided in [Select a network type](reseller.en-US/Data Collection/Logtail collection/Select a network type.md#).

-   Q: **How can I determine whether access is established successfully?** 

    A: Access is established successfully if information is returned after you run the following command:

    ```
     curl $myproject.cn-hangzhou.log.aliyuncs.com
    ```

    In this command, `$myproject` indicates the project name, and `cn-hangzhou.log.aliuncs.com` indicates the access point.


