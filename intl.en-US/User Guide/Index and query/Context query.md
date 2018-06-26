# Context query {#task_xj1_sqc_ry .task}

When you expand a log file, each log records an event. Generally, logs are not independent from each other. Several consecutive logs allow you to view the process of a whole event in sequence.

Log context query specifies the log source \(machine + files\) and a log in the log source. It also queries several logs before and after the log in the original log file, providing a helpful method for troubleshooting the problem in the DevOps scenario.

The Log Service console provides a query page, you can view the context information of the specified log in the original file in the console. It is similar to paging up and down in the original log file. By viewing the context information of a specified log, you can quickly locate the problem.

## Scenarios {#section_yt5_vqc_ry .section}

For example, the O2O take-out website will record the transaction track of a order in the program log on the server:

**User logon** \> **Browse products** \> **Click items** \> **Add to shopping cart** \> **Place an order** \> **Pay for the order** \> **Deduct payment** \> **Generate an order**

If the order cannot be placed, the Operation & Maintenance \(O&M\) personnel must quickly locate the cause of the problem. In the conventional context query, the administrator grants the machine logon permission to related members, and then the investigator logs on to each machine where applications are deployed in turn,  uses the order ID as the keyword to search application log files, and determines what causes the failure.

In Log Service, you can troubleshoot the problem by following these steps:

1.  Install the log collection client Logtail on the server, and add the machine group and log collection configuration in the console. Then, Logtail starts to upload the incremental logs. You can also use producer-related SDK uploads, such as Log4J, LogBack, C-Producer
2.  On the log query page in the Log Service console, specify the time range, and find the order failure log according to the order ID.
3.  Based on the found error log, page up until other related logs are found \(for example, the deduction failure of credit card\).

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13100/5524_en-US.png "Scenarios")

## Benefits {#section_pqq_qpv_1bb .section}

-   No intrusion into the application. No need to modify the log file format.
-   You can view the log context information of any machine or file in the Log Service console, without logging on to each machine to view the log file.
-   Combined with the time when the event occurred, you can specify the time range to quickly locate the suspicious log and then query its context information in the Log Service console to improve the efficiency.
-   No need to worry about the data loss caused by insufficient server storage space or log file rotation. You can view historical data in the Log Service console at any time.

-   [Use Logtail to collect logs](intl.en-US/User Guide/Logtail collection/Overview.md) . Upload data to the Logstore. Create the machine groups and collection configuration. No other configurations are needed. You can also use producer-related SDK upload, such as Producer Library.
-   Enable the Query logs function.

**Note:** Currently, you cannot query the context information of syslog data. 

1.   Log on to the Log Service console. 
2.   On the Project List page, click the project name. 
3.   On the Logstore List page, click **Query** at the right of the Logstore to enter the query interface. 
4.   Enter your query and analysis statement and select the time range. Then, click **Search**. Click **Context View** at the left side of the log, and the window with the context information of the target log is displayed on the right.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13100/5525_en-US.png "Query log")

5.   Select a log and click **Context View**. View the context log for the target log on the right pop-up page. 
6.   Scroll with the mouse on the page to view the context information of the selected log. To view more context logs, click **Earlier**  or **Later**. 

