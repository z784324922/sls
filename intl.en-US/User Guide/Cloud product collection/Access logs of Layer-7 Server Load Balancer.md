# Access logs of Layer-7 Server Load Balancer {#concept_gxr_w3q_zdb .concept}

Alibaba Cloud Server Load Balancer can distribute traffic for multiple Elastic Compute Service \(ECS\) instances, and support Layer-4 Server Load Balancer based on TCP and Layer-7 Server Load Balancer based on HTTP/HTTPS. By using Server Load Balancer, the impact on the business is reduced when a single ECS instance has an exception so that the system availability is enhanced. Working with the dynamic expansion and contraction of Auto Scaling, backend servers can respond to the changes of business traffic quickly.

Each access request to Server Load Balancer records the access logs. The access logs collect the details of all the requests sent to Server Load Balancer, including request time, client IP address, latency, request path, and server response. As an Internet access point, Server Load Balancer hosts a large number of access requests. By using the access logs, you can analyze the user behavior on the client, the geographical distribution of the client users, and troubleshoot the issues.

Use Log Service to collect the Server Load Balancer access logs. You can monitor, probe, diagnose, and report the Layer-7 access logs of HTTP/HTTPS continuously and understand Server Load Balancer instances more comprehensively. 

**Note:** **Only Layer-7 Server Load Balancer** supports the access logs function. The access logs function is available in all regions. For more information, see [Configure access logs](../../../../reseller.en-US/Archives/User Guide (Old Console)/Log management/Configure access logs.md).

## Function advantages {#section_ivt_bzs_zdb .section}

-   **Simple**. Free developers and maintenance staff from tedious and time-consuming log processing so that they can concentrate on business development and technical research.
-   **Massive**. Access logs are proportional to request PVs of Server Load Balancer instances. The data size is usually large. Therefore, the performance and cost issues must be considered when processing access logs. Log Service can analyze 100 million logs in a second and has obvious cost advantages compared with the open-source solutions.
-   **Real-time**. Scenarios such as DevOps, monitoring, and alerting require real-time log data. Traditional data storage and analysis tools cannot meet this requirement. For example, it takes long time to ETL data to Hive at which a lot of work is spent on data integration. Powered by its powerful computing capability, Log Service can process and analyze access logs in seconds.
-   **Flexible**. You can enable or disable the access log function at the level of Server Load Balancer instance.  You can enable or disable the access log function at the level of Server Load Balancer instance. Additionally, you can set the storage period \(1–365 days\) and the Logstore capacity of logs is dynamically scalable to meet business growth requirements. 

## Configure Log Service to collect Layer-7 Server Load Balancer access logs {#section_jwj_2zs_zdb .section}

## Prerequisites {#section_ond_fzs_zdb .section}

1.  You have activated Server Load Balancer and Log Service. The created [Create an SLB instance](../../../../reseller.en-US/Archives/User Guide (Old Console)/SLB instances/Create an instance.md), Log Service project, and Logstore are **in the same region**.

    **Note:** Only Layer-7 Server Load Balancer supports the function of access logs. For the available regions, see Access logs. 

2.  If you are a RAM user, you must be authorized to use the SLB access logging. For more information, see [Authorize a RAM user to configure access logs](../../../../reseller.en-US/Archives/User Guide (Old Console)/Log management/Authorize a RAM user to configure access logs.md). 

## Procedure  {#section_djs_lzs_zdb .section}

1.  Log on to the Log Service console.
2.  After project and Logstore are created, follow the page prompts to enter the **data import wizard**.  You can also click the **Data Import Wizard** icon on the Logstore List page to enter the configuration process.
3.  Select a data source.

    Click **Server Load Balancer** in **Cloud Services** and then click Next.

4.  RAM authorization. 

    Click **Authorize** as instructed on the page. Then, click **Confirm Authorization Policy** to authorize Server Load Balancer to access Log Service.

5.  Set dispatch rule.  Click **Dispatch configuration** to go to the Server Load Balancer console.
    1.  Click**Logs** \> **Access Log**in the left-side navigation pane.
    2.  Click **Configure**at the right of the Server Load Balancer instance.

        **Note:** Make sure the Log Service project and the SLB instance are in the same region.

        ![](images/5476_en-US.png "Log Settings ")

    3.  Select the project and Logstore of Log Service. Then, click **Confirm**.
    4.  After the configuration is complete, close the dialog box. Return to the **data import wizard** and click **Next**.

        ![](images/5477_en-US.png "Configure Data Source ")

6.  Search, analysis, and visualization.

    Log Service presets the query indexes required by Server Load Balancer. For the field descriptions, see **Field description** in this document.  Click **Next**.

    **Note:** The dashboard \{LOGSTORE\}-slb\_layer7\_access\_center and \{LOGSTORE\}-slb\_layer7\_operation\_center are created by default. After the configuration is complete, you can view it on the Dashboard page.

7.  Click **Confirm** to complete the data access.

## Subsequent operations {#section_bzv_yzs_zdb .section}

-   **Query logs in real time **

    You can perform a rapid accurate or fuzzy [query](reseller.en-US/User Guide/Index and query/Overview.md) by using any keyword in the log. This feature can be used for problem location or statistical query.

-   **Preset analysis reports**

    Server Load Balancer predefines some global statistics graphs, including Top client access, distribution of request status codes, Top URI access, traffic trend of request messages, and statistics of RealServer response time.

-   **Customize analysis charts **

    You can perform an ad-hoc query for any log item according to the statistical requirement and save the results as a chart to meet your daily business requirements. 

-   **Set log monitoring alarms**

    You can perform customized analysis on Server Load Balancer request logs and save the results as a quick query. Set the quick query as an alarm. When the computing results of real-time logs exceed the defined threshold, the system sends an alarm notification.


## Field descriptions {#section_qg2_11t_zdb .section}

|Field|Description|
|:----|:----------|
|body\_bytes\_sent|The size of the HTTP body \(in bytes\) sent to the client.|
|client\_ip|The request client IP.|
|host|The host is obtained from the request parameters first. If no value is obtained, obtain the host from the host header. If the value is still not obtained, use the backend server IP of the processing request as the host.|
|http\_host |The host header contents in the request message.|
|http\_referer |The HTTP referer header contents in the request message received by the proxy.|
|http\_user\_agent|The HTTP user-agent header contents in the request message received by the proxy.|
|http\_x\_forwarded\_for|The x-forwarded-for contents in the request message received by the proxy. |
|http\_x\_real\_ip |The real client IP.|
|read\_request\_time|The time \(in milliseconds\) for the proxy to read request.|
|request\_length |The length of the request message, including startline, HTTP header, and HTTP body.|
|request\_method |The method of the request message.|
|Request\_time|The interval \(in seconds\) between the time when proxy receives the first request message and the time when proxy returns the response.|
|request\_uri |The URI of the request message received by the proxy.|
|scheme|The request schema \(http or https\).  |
|server\_protocol|The HTTP protocol version received by the proxy. For example, HTTP/1.0 or HTTP/1.1.|
|slb\_vport|The listening port of Server Load Balancer.|
|slbid|The Server Load Balancer instance ID.|
|ssl\_cipher|The used cipher, such as ECDHE-RSA-AES128-GCM-SHA256/.|
|ssl\_protocol |The protocol used to establish the SSL connection, such as TLSv1.2.|
|status|The status of proxy responding to the message. |
|tcpinfo\_rtt |The tcp rtt time \(in microseconds\) on the client.|
|time|The log recorded time.|
|Upstream\_addr|The IP address and port of the backend server.|
|upstream\_response\_time |The time \(in seconds\) during which Server Load Balancer establishes a connection on the backend, receives the data, and closes the connection.|
|upstream\_status|The response status code of the backend server received by the proxy.|
|vip\_addr |The vip address.|
|write\_response\_time|The time \(in milliseconds\) for the proxy to write responses.|

