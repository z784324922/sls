# Log reports {#concept_gtd_w3h_1gb .concept}

The Dashboard page of Log Service is embedded in the Log Reports page. The Log Reports page displays the default dashboards. You can adjust dashboard data by modifying the time range and adding filtering conditions.

## View log reports {#section_mvp_4qn_j2b .section}

1.  Log on to the new Anti-DDoS Pro console. In the left-side navigation pane, choose **Statistics** \> **Full Log**.
2.  Select the website domain for which you want to view log reports, and ensure that the **Status** switch is turned on.
3.  Click **Log Reports**.

    The Dashboard page of Log Service is embedded in the current page. The system displays log reports of the target website based on a default **filtering condition**, such as `matched_host:"0523.yuanya.aliyun.com"`.


![](images/33770_en-US.png "View log reports")

After you enable the log collection feature of Anti-DDoS Pro for your website, Log Service automatically creates two default dashboards: Operation Center and Access Center.

|Dashboard name|Description|
|:-------------|:----------|
|Operation Center|Displays the current operational status of the website protected by Anti-DDoS Pro. This includes the traffic volume incurred by valid requests and by CC attacks, the valid request ratio, the maximum bandwidth occupied by CC attacks, and the distribution of CC attackers.|
|Access Center|Displays the current access status of the website protected by Anti-DDoS Pro. This includes statistics on PV, UV, peak throughput, visitor locations, lines, client types, request types, visited websites.|

![](images/6806_en-US.jpg "Default dashboards")

In addition to viewing log reports, you can perform the following operations:

-   Specify a [time range](reseller.en-US/User Guide/Cloud product collection/DDoS log collection/Log Report.md#section_uf1_hwn_j2b).
-   Add or edit [filtering conditions](reseller.en-US/User Guide/Cloud product collection/DDoS log collection/Log Report.md#section_kht_4rn_j2b).
-   View [charts](reseller.en-US/User Guide/Cloud product collection/DDoS log collection/Log Report.md#section_gt5_hwn_j2b).

## Time picker {#section_uf1_hwn_j2b .section}

Each chart on a dashboard is generated based on the statistical data of a separate time range. For example, the default time range is one day for the PV chart and 30 days for the chart showing PV and UV trends. To set all charts on the dashboard to be displayed in the same time range, you can configure the **time picker**.

1.  Click **Please Select**.
2.  Specify a time range in the right-side pane that appears. You can select a relative time, a time frame, or set a custom time.

**Note:** 

-   The specified time range applies to all charts on the dashboard.
-   The settings of time pickers apply to a temporary view of charts on a dashboard, and the system does not save these settings. The next time you view log reports, the system still uses the default time range.

![](images/6807_en-US.jpg "Specify a time range")

## Filtering conditions {#section_gh1_cxn_j2b .section}

Select a website domain name and click **Log Reports** to open a dashboard. The system displays log reports of the target website based on a default **filtering condition**, such as `matched_host:"0523.yuanya.aliyun.com"`.

You can use **filtering conditions** to modify the range of data displayed in log reports.

-   **View log reports for all websites** 

    You can clear filtering conditions to view log reports for all websites in the current Logstore.

-   **Add filtering conditions** 

    You can add a filtering condition by setting a pair of **key** and **value**. Multiple filtering conditions are of the `AND` relationship.

    For example, you can set value to an Internet service provider \(such as China Telecom\) to view request statistics of a specific line.

    ![](images/6808_en-US.jpg "Add filtering conditions")

    **Note:** `isp_line` is a log field of Anti-DDoS Pro, which indicates the inbound ISP line connecting to the port. For more information about log fields, see [Log fields](reseller.en-US/User Guide/Cloud product collection/DDoS log collection/Collection procedure.md#table_irg_c1v_j2b).


## Chart types {#section_gt5_hwn_j2b .section}

The dashboard displays multiple types of charts based on a predefined layout. For more information about chart types, see [Graph description](reseller.en-US/User Guide/Query and visualization/Analysis graph/Graph description.md).

|Chart type|Description|
|:---------|:----------|
|Single value chart|Displays key indicators, such as valid request ratio and attack peaks.|
|Line chart and area chart|Displays trends of key indicators within a time range, such as inbound traffic, attacks, and interception.|
|Map|Displays the geographical distribution of visitors and attackers, such as CC attack source and access heatmap.|
|Pie chart|Displays the distribution of information such as top 10 attacked websites and different types of clients.|
|Table|Displays information such as attacker list, which typically contains multiple columns.|
|Map|Displays the geographical distribution of data.|

## Default dashboards {#section_qyt_prn_j2b .section}

-   **Operation Center** 

    **Operation Center** displays the current operational status of the website protected by Anti-DDoS Pro. This includes the traffic volume incurred by valid requests and by CC attacks, the valid request ratio, the maximum bandwidth occupied by CC attacks, and the distribution of CC attackers.

    |Chart name|Type|Default time range|Description|Example|
    |:---------|:---|:-----------------|:----------|:------|
    |Valid request ratio|Single value chart|1 hour \(relative\)|The ratio of valid requests to all requests. Valid requests are requests except CC attack requests and 400 bad requests.|95%|
    |Valid request traffic ratio|Single value chart|1 hour \(relative\)|The ratio of the traffic incurred by valid requests to the traffic incurred by all requests.|95%|
    |Traffic received|Single value chart|1 hour \(relative\)|The total inbound traffic incurred by valid requests. Unit: MB.|300 MB|
    |Attack traffic|Single value chart|1 hour \(relative\)|The total inbound traffic incurred by CC attacks. Unit: MB.|30 MB|
    |Traffic out|Single value chart|1 hour \(relative\)|The total outbound traffic that is generated by valid requests. Unit: MB.|300 MB|
    |Peak network in|Single value chart|1 hour \(relative\)|The maximum inbound throughput of the website's requests. Unit: Bytes/s.|100 Bytes/s|
    |Peak network out|Single value chart|1 hour \(relative\)|The maximum outbound throughput of the website's requests. Unit: Bytes/s.|100 Bytes/s|
    |Received requests|Single value chart|1 hour \(relative\)|The total number of received valid requests \(not CC attacks\).|30,000|
    |Attack count|Single value chart|1 hour \(relative\)|The total number of requests initiated by CC attacks.|100|
    |Peak attack size|Single value chart|1 hour \(relative\)|The maximum number of CC attack requests per minute.|100 per minute|
    |Network traffic in and attack|Two-line chart|1 hour \(time frame\)|The traffic incurred by valid requests and attack requests per minute. Unit: KB/s.|N/A|
    |Request and interception|Two-line chart|1 hour \(time frame\)|The total number of requests and intercepted CC attack requests per minute.|N/A|
    |Valid request ratio|Two-line chart|1 hour \(time frame\)|The ratio of valid requests \(excluding CC attack requests and 400 bad requests\) to all requests every minute.|N/A|
    |Access status distribution|Flow chart|1 hour \(time frame\)|The distribution of requests with different status codes \(such as 400, 304, and 200\) per minute.|N/A|
    |Attack source \(world\)|World map|1 hour \(relative\)|The distribution of CC attacks in origin countries.|N/A|
    |Attack source \(China\)|China map|1 hour \(relative\)|The distribution of CC attacks that originate in the provinces of China.|N/A|
    |Attacker list|Table|1 hour \(relative\)|The information about top 100 attackers, including IP addresses, countries, cities, network, attack count, and attack throughput.|N/A|
    |Attack access line distribution|Pie chart|1 hour \(relative\)|The distribution of ISP lines accessed by CC attacks, such as lines of China Telecom, China Unicom, and BGP.|N/A|
    |Top 10 attacked websites|Doughnut chart|1 hour \(relative\)|The top 10 most attacked websites.|N/A|

-   **Access Center** 

    **Access Center** displays the current access status of the website protected by Anti-DDoS Pro. This includes statistics on PV, UV, peak throughput, visitor locations, lines, client types, request types, visited websites.

    |Chart name|Type|Default time range|Description|Example|
    |:---------|:---|:-----------------|:----------|:------|
    |PV|Single value chart|1 hour \(relative\)|The total number of page views \(PVs\).|100,000|
    |UV|Single value chart|1 hour \(relative\)|The total number of unique visitors \(UVs\).|100,000|
    |Traffic in|Single value chart|1 hour \(relative\)|The total inbound traffic. Unit: MB.|300 MB|
    |Peak network in traffic|Single value chart|1 hour \(relative\)|The maximum inbound throughput of the website's requests. Unit: Bytes/s.|100 Bytes/s|
    |Peak network out traffic|Single value chart|1 hour \(relative\)|The maximum outbound throughput of the website's requests. Unit: Bytes/s.|100 Bytes/s|
    |Traffic network trend|Two-line chart|1 hour \(time frame\)|The trend of inbound and outbound website traffic per minute. Unit: Bytes/s.|N/A|
    |Request and interception|Two-line chart|1 hour \(time frame\)|The total number of requests and intercepted CC attack requests per minute.|N/A|
    |PV/UV trends|Two-line chart|1 hour \(time frame\)|The trends of PV and UV per minute.|N/A|
    |Access source|World map|1 hour \(relative\)|The distribution of visitors in origin countries.|N/A|
    |Access heatmap|Amap|1 hour \(relative\)|The heatmap that represents the geographical locations of visitors.|N/A|
    |Traffic in source \(world\)|World map|1 hour \(relative\)|The distribution of inbound traffic in origin countries. Unit: MB.|N/A|
    |Traffic in source \(China\)|China map|1 hour \(relative\)|The distribution of inbound traffic that originates in the provinces of China. Unit: MB.|N/A|
    |Access line distribution|Doughnut chart|1 hour \(relative\)|The distribution of ISP lines accessed by visitors, such as lines of China Telecom, China Unicom, and BGP.|N/A|
    |Network provider source|Doughnut chart|1 hour \(relative\)|The proportion of inbound traffic that is carried by the line of each ISP, such as China Telecom, China Unicom, China Mobile, and CERNET. Unit: MB.|N/A|
    |Top clients|Table|1 hour \(relative\)|The information about the top 100 most visited clients. The information includes IP addresses, countries, cities, network, request method distribution, inbound traffic, the number of invalid requests, and the number of intercepted CC attacks.|N/A|
    |Accessed websites|Doughnut chart|1 hour \(relative\)|The domain names of the top 20 most visited websites.|N/A|
    |Referer|Table|1 hour \(relative\)|The top 100 most used referer URLs, redirection target hosts, and the number of redirections.|N/A|
    |PC client distribution|Doughnut chart|1 hour \(relative\)|The top 20 most used user agents, such as iPhone, iPad, Windows IE, and Chrome.|N/A|
    |Request content type distribution|Doughnut chart|1 hour \(relative\)|The top 20 most requested content types, such as HTML, Form, JSON, and streaming data.|N/A|


