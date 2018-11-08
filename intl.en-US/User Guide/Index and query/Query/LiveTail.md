# LiveTail {#concept_edp_dt1_mfb .concept}

LiveTail is an interactive function provided by Log Service in the console to help you monitor logs in real time and extract key log information.

## Scenarios {#section_ik5_yt1_mfb .section}

In scenarios of online Operation & Maintenance \(O&M\), it is often necessary to monitor inbound data of the log queue in real time, and to extract key information from the latest log data to quickly find the cause of the exception. By using the traditional O&M method, you need to run the `tail -f` command on log files on the server to monitor the log files in real time. If the log information you require is not apparent enough, you can add `grep` or `grep –v` to the command to filter keywords. Log Service provides LiveTail in the console, an interactive function that monitors and analyzes online log data in real time, making O&M easier.

## Benefits {#section_f3g_zt1_mfb .section}

-   Monitors real-time log information, and marks and filters keywords.
-   Distinguishes collected logs by using indexes through the collection configuration.
-   Perform word segmentation for log fields to query the context logs that contain segmented words.
-   Tracks the log file for real-time monitoring according to a single log entry without the need to connect to the server.

## Limits {#section_djl_zw1_mfb .section}

-   LiveTail is only applicable to the logs collected by Logtail.
-   LiveTail is available only when logs are collected.

## Use LiveTail to monitor logs in real time {#section_ejk_c51_mfb .section}

1.  登录[日志服务控制台](https://partners-intl.console.aliyun.com/#/sls)，单击Project名称。
2.  Click **Search** in the **LogSearch** column.
3.  You can use LiveTail in one of the following two ways:

    -   Quickly start LiveTail.

        1.  On the **Raw Logs** tab, click the ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23704/154166193813747_en-US.png) icon on the right of the sequence number of the raw log, and select **LiveTail**.

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23704/154166193813762_en-US.png)

        2.  The system automatically starts LiveTail and starts timing.

            **Source Type**, **Machine Name**, and **File Name** are pre-configured to specify the raw logs.

            After LiveTail is started, the log data collected by Logtail is displayed in order on the page. The latest log data is always displayed at the bottom of the page. The scrollbar is at the lowest position on the page by default so that you can immediately see the latest data. The page displays up to 1000 log entries. When 1000 log entries are displayed, the page automatically refreshes to display the latest log entry at the bottom of the page.

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23704/154166193813763_en-US.png)

        3.  \(Optional\) Enter keywords in the search box.

            Only log entries that contain the keywords can be displayed in the monitoring list. By filtering logs that contain the keywords, you can monitor the content of the logs in real time.

        4.  To analyze logs in which exceptions may exist during the real-time log monitoring process, click **Stop LiveTail**.

            After you stop LiveTail, the LiveTail timing and the real-time log data update also stop.

            For exceptions found in the process of log monitoring, Log Service provides multiple analysis methods. For more information, see [Use LiveTail to analyze logs](#).

    -   Customize LiveTail settings.
        1.  Click the LiveTail tab.

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23704/154166193913764_en-US.png)

        2.  Configure LiveTail.

            |Configuration|Required|Description|
            |:------------|:-------|:----------|
            |**Source type**|Yes|Log source, including:            -   Common log
            -   Kubernetes
            -   Docker
|
            |**Machine name**|Yes|Name of the log source server.|
            |**File name**|Yes|Full path and file name of the log file.|
            |**Filter keywords**|No|Keywords. After you configure a keyword, only the logs that contain the keyword can be displayed in the real-time monitoring window.|

        3.  Click **Start LiveTail**.

            After LiveTail is started, log data collected by Logtail are displayed in orders on the page. The latest log data is always displayed at the bottom of the page. The scrollbar resides at the lowest position of the page by default so that you can see the latest data. The page displays up to 1000 log entries. When 1000 log entries are displayed, the page automatically refreshes to display the latest log entry at the bottom of the page.

        4.  To analyze logs in which exceptions might exist during the real-time log monitoring process, click **Stop LiveTail**.

            After you stop LiveTail, the LiveTail timing and the real-time log data update stop as well.

            For exceptions found in the process of log monitoring, Log Service provides multiple analysis methods. For more information, see [Use LiveTail to analyze logs](#).


## Use LiveTail to analyze logs {#section_nt2_3cb_mfb .section}

After you stop LiveTail, the real-time monitoring window stops updating logs, and you can analyze and troubleshoot the exceptions found in the monitoring process.

-   View the logs that contain the specified field.

    Word segmentation has been conducted to all fields. When you click the exception field content, that is, a keyword, the page automatically jumps to the Raw Logs tab, and the system filters all logs to show the logs that contain the keyword. In addition, you can also analyze the logs that contain the keyword by using context view, statistical charts, and other analysis methods.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23704/154166193913765_en-US.png)

-   Narrow the time range of a query according to the log distribution histogram.

    When LiveTail is started, the log distribution histogram is also updated synchronously. If you find an exception of log distribution for a time period, for example a significant increase in the number of logs, you can click the green rectangle of the time period to narrow the time range of the query. The timeline of the raw logs redirected from the LiveTail page is associated with the timeline clicked in LiveTail. You can view all the raw logs and the detailed log distribution over time during this time period.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23704/154166193913766_en-US.png)

-   Highlight key information with column settings.

    On the LiveTail tab, click **Column Settings** in the upper-right corner of the log list, you can set a specified field as a separate column to make the data in this column more obvious. You can configure the data that requires high attention as one column to make it easier to view and recognize exceptions.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23704/154166193913767_en-US.png)

-   Quickly analyze log data.

    On the LiveTail tab, by clicking the arrow in the upper-left corner of the log list, you can expand the quick analysis area. The time interval of the quick analysis is the period from when LiveTail starts to when it stops. The quick analysis provided in LiveTail is the same as that provided in the raw logs. For more information, see [Quick analysis](reseller.en-US/User Guide/Index and query/Query/Quick analysis.md).

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23704/154166193913768_en-US.png)


