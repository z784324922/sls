# Log Report {#reference_emx_xkq_32b .reference}

Log Reports page is embedded in the dashboard of the Log Service. This page displays the default dashboard. You can view dashboard data under various filter conditions by modifying the time range and adding filters. 

## View reports {#section_mvp_4qn_j2b .section}

1.  Log on to the Anti-DDoS Pro console and select **Log** \> **Full Log** in the left-side navigation pane. Enter the Full Log page.
2.  Select the website for which you want to enable DDoS log collection function and make sure the **Status** is on.
3.  Click **Log Reports**.

    Dashboard page of Log Service is embedded in the current page, and the **filter condition** is automatically added. For example, use `matched_host: www.aliyun.com` to view log reports based on selected website.


![](images/6805_en-US.jpg "View reports")

After the DDoS log collection function is enabled for the website, Log Service automatically creates two default instruments for reporting: operation center and access center. For more information about the default dashboard, see [Default dashboard](reseller.en-US/User Guide/Cloud product collection/DDoS log collection/Log Report.md#section_qyt_prn_j2b).  

|Dashboard|Dashboard name|Description|
|:--------|:-------------|:----------|
|**ddos-pro-logstore\_**ddos\_operation\_center|DDoS operation center|Displays the current overall operational status of DDoS protected websites, including valid request status, traffic, trends, attack distributions, and traffic volumes and peaks attacked by CC.|
|**ddos-pro-logstore\_**ddos\_access\_center|DDoS access center|Displays the current overall operational status of DDoS protected websites, including PV/UV trends and bandwidth peaks, visitors, traffic, client type, request, and visited websites distribution.|

![](images/6806_en-US.jpg "Default dashboard")

Besides viewing the report, the following operations can be performed: 

-   Select [time range](reseller.en-US/User Guide/Cloud product collection/DDoS log collection/Log Report.md#section_uf1_hwn_j2b)
-   Add or edit [filter condition](reseller.en-US/User Guide/Cloud product collection/DDoS log collection/Log Report.md#section_kht_4rn_j2b)
-   View [charts](reseller.en-US/User Guide/Cloud product collection/DDoS log collection/Log Report.md#section_gt5_hwn_j2b)

## Time picker {#section_uf1_hwn_j2b .section}

All charts on the dashboard page are based on statistical results for different time periods. For example, the default time range for visits is one day and the access trend is 30 days.  To set all charts on the current page to be displayed in the same time range, you can configure the **time picker**.

1.  Click **Select**.
2.  Configure the settings in the dialog box. You can select relative time, entire point time, or set a custom time. 

**Note:** 

-   When the time range is modified, the time of all charts is changed to this time range.
-   Time picker only provides a temporary view of the chart on the current page, and the system does not save the setting.  The next time you view the report, the system will display the default time range.

![](images/6807_en-US.jpg "Set the time range")

## Filter conditions {#section_gh1_cxn_j2b .section}

Select the website and click **Log Reports** to enter the dashboard page. System automatically adds **filter condition**, such as `matched_host: www.aliyun.com` to view log reports based on selected website. 

You can modify the data display range of the report by setting **filter condition**.

-   **View overall reports for all websites**

    Clear the filter condition to display the overall reports library `ddos-pro-logstore`. 

-   **Add more filter conditions**

    You can filter the report data by setting **key** and **value**. `AND` relationship between multiple filters is supported.

    For example, view the overall situation of access requests by telecommunications lines.

    ![](images/6808_en-US.jpg "Add filter conditions")

    **Note:** The `isp_line` is the field of the DDoS log, indicating the operator network connecting to the port. For more information about fields, see [DDoS log fields](reseller.en-US/User Guide/Cloud product collection/DDoS log collection/Collection procedure.md#table_irg_c1v_j2b). 


## Chart type {#section_gt5_hwn_j2b .section}

The report display area shows multiple reports according to a predefined layout, including the following types. For more information about chart types, see [Graph description](reseller.en-US/User Guide/Query and visualization/Analysis graph/Graph description.md).

|Chart type|Description|
|:---------|:----------|
|Number|Displays important indicators, such as effective request rate, and attack peaks.|
|Line/area map|Displays trend graphs for certain important indicators within a specific time period, such as inbound bandwidth trends and attack interception rates.|
|Map|Displays the geographical distribution of visitors and attackers, such as CC attack country, access hotspot.|
|Pie chart|Displays the distribution of the information, such as the top 10 of the websites being attacked, client type distribution.|
|Table|Displays information such as the list of attackers, typically divided into multiple columns.|
|Maps|Displays the geographical distribution of the data.|

## Default dashboards {#section_qyt_prn_j2b .section}

-   **Operation center**

    **Operations center** displays the current overall operational status of DDoS protected websites, including valid request status, traffic, trends, attacker distributions, and traffic volumes and peaks attacked by CC.

    |Chart|Type|Default time range|Description|Example|
    |:----|:---|:-----------------|:----------|:------|
    |Valid request package rate|Single value|1 hour \(relative\)|A valid request, that is, the number of non-CC attacks or 400 error requests in the total number of all requests.|95%|
    |Valid request flow rate|Single value|1 hour \(relative\)|Valid request percentage of the total flow of all requests.|95%|
    |Received traffic|Single value|1 hour \(relative\)|The sum of valid request inflows. The unit is MB.|300 MB|
    |Attack traffic|Single value|1 hour \(relative\)|The sum of inbound traffic of CC attacks. The unit is MB.|30 MB|
    |Outbound traffic|Single value|1 hour \(relative\)|The sum of valid request outbound traffic. The unit is MB.|300 MB|
    |Network in bandwidth peak.|Single value|1 hour \(relative\)|The highest peak of incoming traffic rate requested by the website. The unit is bytes/s.|100 Bytes/s|
    |Network out bandwidth peak.|Single value|1 hour \(relative\)|The highest peak of outbound traffic rate requested by the website. The unit is bytes/s.|100 Bytes/s|
    |Received data packets|Single value|1 hour \(relative\)|The number of incoming requests for valid requests \(non-CC attacks\), measured in units.|30, 000|
    |Attack data packets|Single value|1 hour \(relative\)|The sum of the number of requests for the CC attack, measured in units.|100|
    |Attack peak|Single value|1 hour \(relative\)|The highest peak of CC attack. The unit is number per minute.|100 per minute|
    |Inbound bandwidth and attack trends|Two-line diagram|1 hour \(entire point\)|Trend chart of valid requests per minute and traffic bandwidth for attack requests.  The unit is KB/s.|-|
    |Request and interception trends|Two-line diagram|1 hour \(entire point\)|Trend chart of the total number of requests and intercepted CC attack requests per minute.  The unit is number per minute.|-|
    |Valid request rate trend|Two-line diagram|1 hour \(entire point\)|Trend chart of the number of valid requests per minute \(non-CC attacks or 400 error requests\) in the total number of all requests.|-|
    |Access status distribution trend|Flow chart|1 hour \(entire point\)|Trend chart of various request processing statuses \(400, 304, 20\) per minute. The unit is number per minute.|-|
    |CC attacks distribution|World map|1 hour \(relative\)|The sum of the number of CC attacks in the source country.|-|
    |CC attack distribution|Map of China|1 hour \(relative\)|The sum of the number of CC attacks in the source province \(China\).|-|
    |List of attacks|Table|1 hour \(relative\)|The attacker information of the first 100 attacks, including IP, city, network, number of attacks, and total traffic.|-|
    |Attack access line distribution|Pie chart|1 hour \(relative\)|CC attack source access DDoS protection line distribution, such as telecommunications, Unicom, and BGP.|-|
    |Top 10 attacked websites|Donut chart|1 hour \(relative\)|Top 10 attacked websites|-|

-   **Access center **

    **Access center** displays the current overall operational status of DDoS protected websites, including PV/UV trends and bandwidth peaks, visitors, traffic, client type, request, and visited websites distribution.

    |Chart|Type|Default time range|Description|Example|
    |:----|:---|:-----------------|:----------|:------|
    |Page view|Single value|1 hour \(relative\)|The total number of requests.|100,000|
    |Unique visitors|Single value|1 hour \(relative\)|Total number of independent access clients. |100,000|
    |Inbound traffic|Single value|1 hour \(relative\)|The sum of inbound traffic of the website.The unit is MB.|300 MB|
    |Network in bandwidth peak.|Single value|1 hour \(relative\)|The highest peak of inbound traffic rate requested by the website. The unit is bytes/s.|100 Bytes/s|
    |Network out bandwidth peak.|Single value|1 hour \(relative\)|The highest peak of inbound traffic rate requested by the website. The unit is bytes/s.|100 Bytes/s|
    |Traffic bandwidth trend|Two-line diagram|1 hour \(entire point\)|Trend chart of website inbound and outbound traffic per minute. The unit is KB/s.|-|
    |Request and interception trends|Two-line diagram|1 hour \(entire point\)|Trend chart of the total number of requests and intercepted CC attack requests per minute.  The unit is number per minute.|-|
    |PV/UV access trends|Two-line diagram|1 hour \(entire point\)|Trend chart of PV and UV per minute. Measured in units.|-|
    |Visitor distribution|World map|1 hour \(relative\)|The distribution of visitors PV \(page view\) in the source country.|-|
    |Visitor heat map|Amap|1 hour \(relative\)|Visitor geographic access heat map.|-|
    |Inbound traffic distribution|World map|1 hour \(relative\)|Sum of inbound traffic distribution in the source country. The Unit is MB.|-|
    |Inbound traffic distribution|Map of China|1 hour \(relative\)|Sum of inbound traffic distribution in the source province. The Unit is MB.|-|
    |Access line distribution|Donut chart|1 hour \(relative\)|Source-based access DDoS protection line distribution, such as telecommunications, Unicom, and BGP.|-|
    |Inbound traffic network provider distribution|Donut chart|1 hour \(relative\)|The distribution of inbound traffic that visitors access by network operators. For example, telecommunications, Unicom, mobile connections, education network. The Unit is MB.|-|
    |Most visited clients|Table|1 hour \(relative\)|The top 100 most visited clients, including IP, city, network, request method distribution, incoming traffic, number of incorrect accesses, number of intercepted CC attacks.|-|
    |Access domain name|Donut chart|1 hour \(relative\)|The top 20 most visited domain names.|-|
    |Referer|Table|1 hour \(relative\)|The top 100 most redirected referer URLs, hosts, and frequency.|-|
    |Client type distribution|Donut chart|1 hour \(relative\)|The top 20 most visited user agents, such as iPhone, iPad, Windows IE, Chrome.|-|
    |Request content type distribution|Donut chart|1 hour \(relative\)|The top 20 most requested content types, such as HTML, Form, JSON, streaming data.|-|


