# Cloud service logs {#concept_lyb_4sn_vgb .concept}

Log Service can collect logs from various cloud services, such as Elastic Compute Service \(ECS\), Object Storage Service \(OSS\), and Server Load Balancer \(SLB\). The logs record cloud service information including operation information, running statuses, and business dynamics.

The following table lists the Alibaba Cloud services from which Log Service can collect logs.

|Type|Cloud service|Activation method|Remarks|
|:---|:------------|:----------------|:------|
|Elastic computing|ECS|Install Logtail.|[Logtail introduction](reseller.en-US/Data Collection/Logtail collection/Overview/Overview.md)|
|Container Service/Container Service for Kubernetes|Activate the service in the Container Service or Container Service for Kubernetes console.|[Text logs](reseller.en-US/Data Collection/Logtail collection/Container log collection/Container text logs.md) and [output](reseller.en-US/Data Collection/Logtail collection/Container log collection/Container stdout.md)|
|Storage|OSS|Activate the service in the OSS console.|[Overview](../../../../reseller.en-US/Data Collection/Cloud product collection/OSS access logs/Overview.md#)|
|Network|SLB|Activate the service in the SLB console.|[Access logs of Layer-7 SLB](reseller.en-US/Data Collection/Cloud product collection/Access logs of Layer-7 Server Load Balancer.md)|
|VPC|Activate the service in the VPC console.|[Create a flow log](../../../../reseller.en-US/Flow logs/Create a flow log.md)|
|API Gateway|Activate the service in the API Gateway console.|[Access logs of API Gateway](reseller.en-US/Data Collection/Cloud product collection/API Gateway Access Log.md)|
|Security|ActionTrail|Activate the service in the ActionTrail console.|[Overview](reseller.en-US/Data Collection/Cloud product collection/ActionTrail access logs/Overview.md)|
|Anti-DDoS Pro/BGP-line Anti-DDoS Pro|Activate the service in the Anti-DDoS Pro console.|[Anti-DDoS Pro overview](reseller.en-US/Data Collection/Cloud product collection/DDoS log collection/Overview.md) and [BGP-line Anti-DDoS Pro overview](reseller.en-US/Data Collection/Cloud product collection/Logs of BGP-line Anti-DDoS Pro/Overview.md)|
|Threat Detection Service|Purchase Threat Detection Service Enterprise Edition and activate the service in the Threat Detection Service console.|[TDS logs](reseller.en-US/Data Collection/Cloud product collection/TDS logs.md)|
|Anti-Bot Service|Activate the service in the Anti-Bot Service console.|[Anti-Bot Service logs](~~100510~~)|
|Application|Log Service|Activate the service in the Log Service console.|[Service log overview](reseller.en-US/Monitor Log Servic/Service log/Service log overview.md)|

