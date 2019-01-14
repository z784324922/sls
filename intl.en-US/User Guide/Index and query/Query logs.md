# Query logs {#task_tqc_ddm_gfb .task}

After enabling the index function and setting indexes, you can query and analyze the collected logs in the console.

-   Logs have been collected.
-   You have [enabled the index function and set indexes](reseller.en-US/User Guide/Index and query/Enable and set indexes.md).

1.  Log on to the [Log Service console](https://partners-intl.console.aliyun.com/#/sls) and click the project name. 
2.  In the **LogSearch** column, click **Search**. 
3.  In the search box, enter a query analysis statement. 

    A query analysis statement consists of a query statement and an analysis statement in the format of `query statement|analysis statement`. For more information, see [Overview](reseller.en-US/User Guide/Index and query/Overview.md).

4.  In the upper-right corner, click **15 Minutes \(Relative\)**to set the time range for queries. You can choose between a relative time period and a time frame or customize a time range.

    **Note:** The query results may contain logs obtained one minute earlier or later than the specified time range.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21326/154743628512618_en-US.png)

5.  Click **Search & Analysis** to view the query results. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21326/154743628512707_en-US.png)

    You can view the query results through a log distribution histogram, raw logs, or various graphs.

    **Note:** By default, 100 query results are returned. If you need to view more results, see [LIMIT syntax](reseller.en-US/User Guide/Index and query/Analysis grammar/LIMIT syntax.md).

    -   **Log distribution histogram**:

        The log distribution histogram shows the log distribution in the time dimension.

        -   Rest the pointer on a green data block to view the time range indicated by the data block and the number of log query results within the time range.
        -   Click a data block to view finer-grained log distribution. Additionally, you can view the log query results on the **Raw logs** tab page.
        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21326/154743628512708_en-US.png)

    -   **Raw logs**:

        On the **Raw logs** tab page, you can view the logs that match your search conditions.

        -   Use the quick analysis function to receive a quick analysis of the distribution of a field over a period of time. For more information, see [Quick analysis](reseller.en-US/User Guide/Index and query/Query/Quick analysis.md).
        -   Click the download icon in the upper-right corner to specify a download range, and then click **OK**.
        -   In the upper-right corner, click **Column Settings**. In the displayed dialog box, select the target fields from the left area and click **Add** to add the fields to the right area. Then, columns indicated by the added fields appear on the tab page. The field names are also column names, and the columns list the field values.

            **Note:** To view the log content on the tab page, you must select **Content**.

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21326/154743628512709_en-US.png)

        -   Set the style of the content column. If the field contains more than 3,000 characters, then some content will be hidden and a message indicating this will be displayed before the field Key. Specifically, click **Display Content Column**. In the displayed dialog box, set **Key-Value Pair Arrangement** and **Truncate Character String**.

            **Note:** If the content limit is set to 10,000 characters, any character past this number will be downgraded. Further, none of these characters will be displayed, and no delimiter will be specified for these characters.

            |Parameter|Description|
            |:--------|:----------|
            |**Key-Value Pair Arrangement**|You can set this parameter to **New Line** or **Full Line** as needed.|
            |**Truncate Character String**|**Key**|When a field value contains more than 3,000 characters, the value is truncated by default. However, this parameter remains empty if this value is not reached.The value of this parameter is the key of the truncated value.

|
            |**Status**|You can determine whether to enable value truncation. It is enabled by default.            -   **Enable**: When a value length exceeds the preset value of **Truncate Step**, the value is automatically truncated. You can click the button at the end of a value to perform incremental expansion. The number of incremental characters is the value of Truncate Step.
            -   **Disable**: A value will not be truncated even if its length exceeds the preset value of **Truncate Step**.
|
            |**Truncate Step**|This parameter indicates the maximum number of a value as well as the number of incremental characters per time.The parameter value ranges from 500 to 10,000 characters. The default value is 3,000.

|

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21326/154743628621106_en-US.png)

    -   **Graph**:

        If you have enabled the statistics function and used a query analysis statement for query, you can view the analysis results on the **Graph** tab page.

        -   Select an appropriate [graph](reseller.en-US/User Guide/Query and visualization/Analysis graph/Graph description.md) type to show analysis results based on your requirements. Several chart types are provided in Log Serve including tables, line charts, and bar charts.

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21326/154743628612710_en-US.png)

        -   Add the graph to the [dashboard](reseller.en-US/User Guide/Query and visualization/Dashboard/Dashboard.md) for real-time data analysis results to be displayed. Click **Add to dashboard** to save common query statements as a graph saved on the dashboard.

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21326/154743628612711_en-US.png)

        -   Set drill-down configurations to gain deeper insight into the analysis results. Then, click the values in the graph to view the analysis results from more dimensions. For more information, see [Drill-down analysis](reseller.en-US/User Guide/Query and visualization/Dashboard/Drill-down analysis.md).

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21326/154743628612712_en-US.png)

    Additionally, you can click **Save Search** and **Save as Alarm** in the upper-right corner. Then, you can use the [saved search](reseller.en-US/User Guide/Index and query/Query/Saved search.md) and [alarm](reseller.en-US/User Guide/Alarm and notification/Set alarms.md) functions.


