# Drill-down analysis {#concept_v5f_zk4_x2b .concept}

Log Service analysis charts provide the drill-down function in addition to the basic data visualization functions. When you add a chart to the dashboard, you can modify the configurations in the drill-down list to present the data in a more powerful way.

Drilling is an essential function for data analysis. It allows you to view more detailed information by moving to different layers of data. Drilling includes rolling up and drilling down. By rolling up, you move to higher data layers with more summarized information. By drilling down, you move to deeper data layers to reveal more detailed information. By drilling down data layer by layer, you can view data more clearly, extract more value out of data, and make more accurate decisions faster based on the data.

Log Service supports drill-down analysis for analysis charts in the dashboard. After you set the dimension and layer of a drilldown, you can jump to the analysis page of a deeper dimension by clicking a data point in the dashboard. Analysis charts in the dashboard are actually results of query statements. If you configure a drill-down analysis for the request status table and add it to the dashboard, you can click a request status type in the dashboard to view logs of the request status.

## Limits {#section_jks_2l4_x2b .section}

In Log Service, that charts that support drill-down analysis include:

-   Table
-   Line chart
-   Column chart
-   Bar chart
-   Pie chart
-   Single value chart
-   Area chart
-   Tree map

## Prerequisites {#section_xlz_zw3_y2b .section}

1.  You have enabled and configured an index.
2.  You have configured a saved search, dashboard, and custom link to jump to.
3.  Configure a placeholder variable of statements in the saved search and dashboard to be jumped to. For more information, see [Saved search](reseller.en-US/User Guide/Index and query/Query/Other functions.md) and [Dashboard](reseller.en-US/User Guide/Query and visualization/Analysis graph/Dashboard.md).

## Procedure {#section_cnd_wl3_y2b .section}

1.  登录[日志服务控制台](https://partners-intl.console.aliyun.com/#/sls)，单击Project名称。
2.  Click **Search** in the **LogSearch** column in the Logstores list.
3.  Enter your query and analysis statement, set the time range, and click **Search & Analysis**.
4.  On the **Graph** tab, select **Chart type** and configure **Properties** of the chart.
5.  Click **Drilldown** on the right side of the **Properties** column, and configure a drill-down event.

    By default, the drilldown configuration is disabled. A drill-down event is triggered by a single clicking. A drill-down event is an event triggered by clicking the analysis chart on the dashboard page. After you configure a drill-down event and click the chart data in the dashboard, your current page jumps to the corresponding page according to your configured drill-down event. Choose one of the following four options.

    -   **Disable**: Disables the drill-down function.
    -   **Open Search Page**: Enables drill down. The drill-down event is to open the search page.

        When you click a value in the chart, the system replaces the placeholder configured in the saved search statement with the chart value you clicked, and then performs a deeper query according to the chart value.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18631/154260564210243_en-US.png)

        |Configuration|Description|
        |:------------|:----------|
        |**Saved Search**|Name of the saved search to be jumped to. For information about configuring a saved search, see [Saved search](reseller.en-US/User Guide/Index and query/Query/Other functions.md).|
        |**Time Range**|Configure the time range for the saved search to be jumped to. The default is **Inherit table time**. That is, after you jump to the saved search by clicking the chart in the dashboard, the time range of the query statement is the table time configured in the dashboard by default when the event is triggered.|
        |**Inherit Filters**|If you turn on the **Inherit Filters** switch, the system synchronizes filtering conditions added in the dashboard to the saved search, and adds the filtering conditions before the query statement by using `AND`.|
        |**Variable**|Click **Add Variable** to enter a placeholder variable name. When the variable in the saved search matches the name of the added variable, the variable in the query statement is replaced with the chart value that triggers the drill-down event. This flexibly changes the dimension of the saved search.**Note:** To add a variable, you must first configure a placeholder variable of the query statement in the saved search to which your page will jump.

|

    -   **Open Dashboard**: Enables the drill-down function. The drill-down event is to open a dashboard.

        The chart in the dashboard is the chart-form result of the query statement. You need to pre-configure a placeholder of the query statement in the dashboard chart to be jumped to. When you click a chart value in the upper layer dashboard, the system replaces the pre-configured placeholder with the chart value, and then performs a deeper query according to the chart value.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18631/154260564210244_en-US.png)

        |Configuration|Description|
        |:------------|:----------|
        |**Dashboard**|Name of the dashboard to be jumped to. For information about configuring a dashboard, see [Dashboard](reseller.en-US/User Guide/Query and visualization/Analysis graph/Dashboard.md).|
        |**Time Range**|Configure the time range of the dashboard to be jumped to. The default is **Inherit table time**. That is, after you jump to the configured dashboard by clicking the chart in the current dashboard, the time range of the configured dashboard is the time configured in the current dashboard chart by default where the drill-down event is triggered.|
        |**Inherit Filters**|If you turn on the **Inherit Filters** switch, the system synchronizes filtering conditions added in the dashboard where an event is triggered to the dashboard to be jumped to. The filtering conditions are added before the query statement by using `AND`.|
        |**Variable**|Click **Add Variable** to enter a placeholder variable name. When the query statement variable of the analysis chart in the dashboard to be jumped to matches the name of the added variable, the query statement variable of the analysis chart is replaced with the chart value that triggers the drill-down event. This flexibly changes the query statement of the analysis chart in the target dashboard.**Note:** To add a variable, you must first configure a placeholder variable of the query statement in the dashboard to jump to.

|

    -   **Custom HTTP link**: Enables the drill- down function. The drill-down event is to open a custom HTTP link.

        The part of path in the HTTP link that is the hierarchical path of the destination file to be accessed. After you add optional parameter fields to the part of path in a custom HTTP link and click the chart content of the dashboard, the system replaces the added parameter fields with the chart value to jump to the relocated HTTP link.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18631/154260564210245_en-US.png)

        |Configuration|Description|
        |:------------|:----------|
        |**Link**|Destination address to be jumped to.|
        |**Optional Parameter Fields**|By clicking an optional parameter variable, you can replace part of the HTTP link with the chart value that triggers a drill-down event.|

6.  Click **Add to New Dashboard**, configure the dashboard, and click **OK**.

    You can then view the analysis chart on the dashboard page, and click the chart to view deeper analysis results.


## Example {#section_sjb_xlq_y2b .section}

For example, you can store collected Nginx access logs in the Logstore named accesslog, display the common analysis scenarios of Nginx logs in the dashboard named RequestMethod, and display the trend of PV distribution over time in the dashboard named destination\_drilldown. You can configure drill-down analysis for the table of request methods, add it to the RequestMethod dashboard, and configure the drill-down event to jump to the destination\_drilldown dashboard. In the RequestMethod dashboard, click each request method to jump to the destination\_drilldown dashboard to view the corresponding PV trend.

The procedure is as follows:

1.  **Configure a dashboard to be jumped to**.

    1.  Filter logs according to request types and view the PV changes over time.

        Query statement:

        ```
        request_method: * | SELECT date_format(date_trunc('minute', __time__), '%H:%i:%s') AS time, COUNT(1) AS PV GROUP BY time ORDER BY time
        ```

    2.  Use a line chart to display the query result and save the line chart to the dashboard.

        When saving the chart to the dashboard, configure `*` as a placeholder named method. If the variable of the drill-down event that jumps to this saved search is also named method, you can replace `*` with your clicked chart value to perform a query and analysis again.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18631/154260564210732_en-US.png)

2.  **Configure a chart that triggers drill-down analysis, and add the chart to the dashboard**.

    1.  On the search page, use SQL to analyze the number of logs of each request method in the Nginx access logs, and display the result in a table.

        ```
        *|SELECT request_method, COUNT(1) AS c GROUP BY request_method ORDER BY c DESC LIMIT 10
        ```

        Query result:

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18631/154260564210705_en-US.png)

    2.  Configure drill-down analysis for the `request_method` column in the table:

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18631/154260564210708_en-US.png)

3.  **Click the GET request in the RequestMethod dashboard**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18631/154260564210714_en-US.png)

4.  **Jump to the destination\_drilldown dashboard**.

    The page automatically jumps to the dashboard configured in [1](#step_1). The `*` in the query statement is replaced with `GET`, the chart value you clicked on. The dashboard then shows changes of the GET request PV over time.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18631/154260564210739_en-US.png)


