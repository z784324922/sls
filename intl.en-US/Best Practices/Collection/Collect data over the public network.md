# Collect data over the public network {#concept_44426_zh .concept}

In some scenarios, you may need to collect data from clients, such as mobile clients, HTML webpages, PCs, servers, hardware devices, and cameras, over the public network for real-time processing.

In a traditional architecture, the preceding feature can be achieved by integrating front-end servers with Kafka. Now, you can use [Log Service](https://www.alibabacloud.com/zh/product/log-service?spm) LogHub to replace such architecture with a solution that is more reliable, cost-effective, elastic, and secure.

## Scenarios {#section_ljv_fqk_5fb .section}

Data can be collected from clients, such as mobile clients, external servers, and webpages, over the public network. After data is collected, data applications, such as real-time computing and data warehouse, are required to store or consume data.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13199/156877404132394_en-US.png)

## Solution 1: Integrate front-end servers with Kafka {#section_o02_hfl_467 .section}

Kafka does not support the RESTful protocol and is more commonly used in clusters. Therefore, you need to set up an NGINX server as a public network proxy, and then use LogStash or API to write data to Kafka through NGINX.

The following table lists the required devices.

|Device|Quantity|Configuration|Purpose|Price|
|:-----|:-------|:------------|:------|:----|
|ECS instance|2|Single-core CPU, 2 GB memory|Front-end server, load balancing, and mutual backup|RMB 108 per unit per month|
|Server Load Balancer|1|Standard|Pay-as-you-go instance|RMB 14.4 per month \(lease\) + RMB 0.8 per GB \(data traffic\)|
|Kafka or ZooKeeper|3|Single-core CPU, 2 GB memory|Data writing and processing|RMB 108 per unit per month|

## Solution 2: Use LogHub {#section_slk_1ca_8pd .section}

You can use mobile SDKs, Logtail, or Web Tracking JS to write data to the LogHub endpoint.

The following table lists the required devices.

|Device|Purpose|Price|
|:-----|:------|:----|
|LogHub|Real-time data collection|Less than RMB 0.2 per GB. Click [here](https://www.aliyun.com/price/product#/sls/detail) to check the billing method.|

## Scenario comparison {#section_xcm_1kv_vjk .section}

Scenario 1: Up to 10 GB of data is collected each day, generating about 1 million write requests. The 10 GB in this example is the compressed size. The actual data volume ranges from 50 GB to 100 GB.

``` {#codeblock_sw1_w89_irp}
Solution 1:
--------------
Server Load Balancer (lease): 0.02 × 24 × 30 = RMB 14.4
Server Load Balancer (data traffic): 10 × 0.8 × 30 = RMB 240
ECS cost: 108 × 2 = RMB 216
Kafka ECS: free if shared with other services
Total: RMB 470.4 per month

Solution 2:
--------------
LogHub traffic: 10 × 0.2 × 30 = RMB 60
Number of LogHub requests: 0.12 (assuming 1 million requests per day) × 30 = RMB 3.6
Total: RMB 63.6 per month
			
```

Scenario 2: Up to 1 TB of data is collected each day, generating about 100 million write requests.

``` {#codeblock_zpa_3ql_qak}
Solution 1:
--------------
Server Load Balancer (lease): 0.02 × 24 × 30 = RMB 14.4
Server Load Balancer (data traffic): 1,000 × 0.8 × 30 = RMB 24,000
ECS cost: 108 × 2 = RMB 216
Kafka ECS: free if shared with other services
Total: RMB 24,230.4 per month

Solution 2:
--------------
LogHub traffic: 1,000 × 0.15 × 30 = RMB 4,500 (tiered pricing)
Number of LogHub requests: 0.12 × 100 (assuming 100 million requests per day) × 30 = RMB 360
Total: RMB 4,860 per month
			
```

## Solution comparison {#section_s53_rwa_1zg .section}

The comparison between the preceding scenarios shows that you can use LogHub to collect data from the public network at a competitive cost. Furthermore, Solution 2 outperforms Solution 1 in the following aspects:

-   Auto scaling: LogHub supports collecting MBs or even PBs of data each day.
-   Abundant permission control options: You can use Access Control List \(ACL\) to control the read and write permissions.
-   HTTPS compatibility: Data is encrypted before transmission.
-   Log shipping at no cost: No additional development is required for shipping logs to data warehouses.
-   Detailed data monitoring: You can gain more information about your business.
-   Abundant SDKs for connecting to upstream and downstream systems: Like Kafka, LogHub provides abundant SDKs to connect to downstream systems. It can be deeply integrated with Alibaba Cloud and open-source products.

For information, see the product page of [Log Service](https://www.alibabacloud.com/zh/product/log-service?spm).

