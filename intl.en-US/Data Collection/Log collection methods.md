# Log collection methods {#concept_ikm_ql5_vdb .concept}

LogHub supports multiple methods to collect logs, such as by using clients, Web pages, protocols, SDKs, and APIs. All collection methods are implemented based on Restful APIs. You can also implement new collection methods by using APIs and SDKs.

## Data sources {#section_yny_rh4_vgb .section}

The following table describes the data sources from which Log Service can collect logs.

|Category|Source|Collection method|Reference|
|:-------|:-----|:----------------|:--------|
|Application|Program output|[Logtail](reseller.en-US/Data Collection/Logtail collection/Overview/Overview.md)| |
|Access log|[Logtail](reseller.en-US/Data Collection/Logtail collection/Overview/Overview.md)|[Collect and analyze Nginx access logs](../reseller.en-US/Quick Start/Collect and analyze Nginx access logs.md)|
|Link tracking|Jaeger Collector and [Logtail](reseller.en-US/Data Collection/Logtail collection/Overview/Overview.md)|-|
|Language|Java|[SDK](../reseller.en-US/SDK Reference/Basic Descriptions /Overview.md) and [Java Producer Library](reseller.en-US/Data Collection/Other collection methods/SDK collection/Producer Library.md)|-|
|Log4j appender|[1.x](https://github.com/aliyun/aliyun-log-log4j-appender) and [2.x](https://github.com/aliyun/aliyun-log-log4j2-appender)|-|
|Logback appender|[Logback](https://github.com/aliyun/aliyun-log-logback-appender)|-|
|C|[Native](../reseller.en-US/SDK Reference/Basic Descriptions /Overview.md)|-|
|Python|[Python](../reseller.en-US/SDK Reference/Basic Descriptions /Overview.md)|-|
|Python logging|[Python logging handler](https://aliyun-log-python-sdk.readthedocs.io/tutorials/tutorial_logging_handler.html)|-|
|PHP|[PHP](../reseller.en-US/SDK Reference/Basic Descriptions /Overview.md)|-|
|C\#|[C\#](../reseller.en-US/SDK Reference/Basic Descriptions /Overview.md)|-|
|C++|[C++ SDK](../reseller.en-US/SDK Reference/C++ SDK.md)|-|
|Go|[Go](../reseller.en-US/SDK Reference/Basic Descriptions /Overview.md) and [Go Producer Library](../reseller.en-US/Data Collection/Other collection methods/SDK collection/Go Producer Library.md#)|-|
|Node.js|[Node.js](https://github.com/aliyun-UED/aliyun-sdk-js)|-|
|JavaScript|[JavaScript/Web tracking](reseller.en-US/Data Collection/Other collection methods/Web Tracking.md)|-|
|Operating system \(OS\)|Linux|[Logtail](reseller.en-US/Data Collection/Logtail collection/Overview/Overview.md)|-|
|Windows|[Logtail](reseller.en-US/Data Collection/Logtail collection/Overview/Overview.md)|-|
|Mac OS or Unix|[Native C](../reseller.en-US/SDK Reference/Basic Descriptions /Overview.md)|-|
|Docker files|[Use Logtail to collect Docker files](reseller.en-US/Data Collection/Logtail collection/Container log collection/Container text logs.md)|-|
|Docker output|[Use Logtail to collect container logs](reseller.en-US/Data Collection/Logtail collection/Container log collection/Container stdout.md)|-|
|Mobile client|iOS/Android|[iOS SDK](../reseller.en-US/SDK Reference/IOS SDK.md), [Android SDK](../reseller.en-US/SDK Reference/Android SDK.md)|-|
|Web page|[JavaScript/Web tracking](reseller.en-US/Data Collection/Other collection methods/Web Tracking.md)|-|
|Intelligent IoT|[C Producer Library](https://github.com/aliyun/aliyun-log-c-sdk)| |
|Alibaba Cloud services|ECS, OSS, and other Alibaba Cloud services. For more information, see [Cloud service logs](../reseller.en-US/Data Collection/Cloud product collection/Cloud service logs.md#).|Activate Log Service in the Alibaba Cloud console|[Cloud service logs](../reseller.en-US/Data Collection/Cloud product collection/Cloud service logs.md#)|
|MaxCompute import|Use DataWorks to export MaxCompute data|[Use DataWorks to export MaxCompute data to Log Service](../reseller.en-US/Data Collection/Other collection methods/Use DataWorks to export MaxCompute data to Log Service.md#)|
|Third-party software|Logstash|[Logstash](reseller.en-US/Data Collection/Other collection methods/Logstash/Create Logstash collection configurations.md)|-|
|Flume|[Use Flume to consume LogHub logs](../reseller.en-US/Real-time subscription and consumption/Use Flume to consume LogHub logs.md#)|-|

The following table lists the Alibaba Cloud services from which Log Service can collect logs.

|Type|Cloud service|Activation method|Remarks|
|:---|:------------|:----------------|:------|
|Elastic computing|ECS|Install Logtail.|[Logtail introduction](../DNSLS11850791/reseller.en-US/Data Collection/Logtail collection/Overview/Overview.md)|
|Container Service/Container Service for Kubernetes|Activate the service in the Container Service or Container Service for Kubernetes console.|[Text logs](../DNSLS11850791/reseller.en-US/Data Collection/Logtail collection/Container log collection/Container text logs.md) and [output](../DNSLS11850791/reseller.en-US/Data Collection/Logtail collection/Container log collection/Container stdout.md)|
|Storage|OSS|Activate the service in the OSS console.|[Overview](../reseller.en-US/Data Collection/Cloud product collection/OSS access logs/Overview.md#)|
|Network|SLB|Activate the service in the SLB console.|[Access logs of Layer-7 SLB](../reseller.en-US/Data Collection/Cloud product collection/Access logs of Layer-7 Server Load Balancer.md)|
|VPC|Activate the service in the VPC console.|[Create a flow log](../reseller.en-US/Flow logs/Create a flow log.md)|
|API Gateway|Activate the service in the API Gateway console.|[Access logs of API Gateway](../reseller.en-US/Data Collection/Cloud product collection/API Gateway Access Log.md)|
|Security|ActionTrail|Activate the service in the ActionTrail console.|[Overview](../reseller.en-US/Data Collection/Cloud product collection/ActionTrail access logs/Overview.md)|
|Anti-DDoS Pro/BGP-line Anti-DDoS Pro|Activate the service in the Anti-DDoS Pro console.|[Anti-DDoS Pro overview](../reseller.en-US/Data Collection/Cloud product collection/DDoS log collection/Overview.md) and [BGP-line Anti-DDoS Pro overview](../reseller.en-US/Data Collection/Cloud product collection/Logs of BGP-line Anti-DDoS Pro/Overview.md)|
|Threat Detection Service|Purchase Threat Detection Service Enterprise Edition and activate the service in the Threat Detection Service console.|[TDS logs](../reseller.en-US/Data Collection/Cloud product collection/TDS logs.md)|
|Anti-Bot Service|Activate the service in the Anti-Bot Service console.|[Anti-Bot Service logs](~~100510~~)|
|Application|Log Service|Activate the service in the Log Service console.|[Service log overview](../reseller.en-US/Monitor Log Servic/Service log/Service log overview.md)|

## Select a network {#section_m2v_rl5_vdb .section}

Log Service provides service endpoints for different Alibaba Cloud regions. For more information, see [Service endpoint](../reseller.en-US/API Reference/Service endpoint.md). Each region allows access from the following networks:

-   Internal network \(classic network\) or private network \(VPC\): Log Service can access other Alibaba Cloud services in the same region, offering optimal link bandwidth. We recommend that you select this option.
-   Public network \(classic network\): accessible without any limits. The transmission speed depends on the link quality. We recommend that you use HTTPS to ensure secure transmission of data.

## FAQ {#section_dfw_bjk_2fb .section}

-   Q: Which network do I select for private line access?

    A: Select the internal network or private network.

-   Q: Can I collect public IP addresses when collecting public network data?

    A: You need to enable Log Service to record public IP addresses. For more information, see [Manage a Logstore](../reseller.en-US/Preparation/Manage a Logstore.md#).

-   Q: Which network do I select if I want to collect ECS logs from Region A and write these logs into the Log Service project in Region B?

    A: Select the public network. You can install Logtail on the ECS instance in Region A for Internet transmission and specify the service endpoint that is associated with Region B. For more information about how to select a network, see [Select a network type](../reseller.en-US/Data Collection/Logtail collection/Select a network type.md#).

-   Q: How can I determine whether a service endpoint is accessible?

    A: You can run the following command. The service endpoint is accessible if any information is returned.

    ``` {#codeblock_x29_7qr_358}
     curl $myproject.cn-hangzhou.log.aliyuncs.com
    ```

    `$myproject` specifies the project name and `cn-hangzhou.log.aliuncs.com` specifies the service endpoint.


