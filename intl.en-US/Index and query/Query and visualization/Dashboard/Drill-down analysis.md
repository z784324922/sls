# Drill-down analysis {#concept_v5f_zk4_x2b .concept}

Log Service analysis charts provide the drill-down analysis in addition to the basic data visualization features. When you add a chart to the dashboard, you can modify the configurations in the drill-down list to present data in a more powerful way.

Drilling is an essential part of data analysis. This feature allows you to view more detailed information by moving to different layers of data. Drilling includes rolling up and drilling down. By rolling up, you move to higher data layers with more summarized information. By drilling down, you move to deeper data layers to reveal more detailed information. By drilling down into data layer by layer, you can view data more clearly, extract more value out of data, and then make more accurate and efficient decisions based on the data.

Log Service supports drill-down analysis for analysis charts in the dashboard. After you set the dimension and layer of a drilldown, you can jump to the analysis page of a deeper dimension by clicking a data point in the dashboard. Analysis charts in the dashboard are actually the results of query statements. If you configure a drill-down analysis for the request status table and add the table to the dashboard, you can click a request status type in the dashboard to view logs of the request status.

## Limits {#section_jks_2l4_x2b .section}

In Log Service, the charts that support drill-down analysis include:

-   Tables
-   Line charts
-   Column charts
-   Bar charts
-   Pie charts
-   Individual value plots
-   Area charts
-   Tree maps

## Prerequisites {#section_xlz_zw3_y2b .section}

1.  You have enabled and configured an index.
2.  You have configured a saved search, a dashboard, and a custom link that you want to jump to.
3.  To add a variable, you must first configure a placeholder in the query statement for the saved search and dashboard that you want to jump to. For more information, see [Saved search](reseller.en-US/Index and query/Query/Other functions.md) and [Create and delete a dashboard](reseller.en-US/Index and query/Query and visualization/Dashboard/Create and delete a dashboard.md).

## Procedure {#section_cnd_wl3_y2b .section}

1.  Log on to the [Log Service console](https://partners-intl.console.aliyun.com/#/sls), and then click the target project name.
2.  Click **Search** in the **LogSearch** column on the Logstores page.
3.  Enter your query analysis statement, set the time range, and then click **Search & Analysis**.
4.  Click the **Graph** tab, select a **chart type** and then configure parameters on the **Properties** page of the chart.
5.  Click the **Interactive Behavior** tab, and set **Event Action** for drill-down analysis.

    A drill-down event is an event that you trigger by clicking an analysis chart on the dashboard page. This event is disabled by default. After you configure a drill-down event and click the chart data in the dashboard, your current page jumps to the corresponding page according to the event action that you have configured. Choose any of the following options:

    -   **Disable**: disables drill-down analysis.
    -   **Open Logstore**: enables drill-down analysis. The drill-down event is to open a Logstore.

        When you click a value in the chart, if you have set Filter, the system automatically adds a query statement for the Logstore that you want to jump to. You cannot set Variable.

        |Parameter|Description|
        |---------|-----------|
        |**Select Logstore**|The name of the Logstore that you want to jump to. For more information about how to configure a Logstore, see [Manage a Logstore](reseller.en-US/Preparation/Manage a Logstore.md#).|
        |**Open in New Tab**|If you enable this option, when interaction behavior is triggered, the system opens the corresponding Logstore in a new tab.|
        |**Time Range**|Configure the time range for the Logstore that you want to jump to. Valid values:         -   **Default**: on the dashboard page, click a chart to jump to the Logstore, and use the default time range 15 minutes for a saved search. This is a relative time range.
        -   **Inherit table time**: after you jump to the Logstore by clicking the chart in the dashboard, the time range of the query statement is the table time configured in the dashboard by default when the event is triggered.
        -   Relative: set the time range of the target saved search to the specified relative time.
        -   Time Frame: set the time range of the target saved search to the specified time frame.
 Default value: **Default**.

 |
        |**Inherit Filters**|If you enable **Inherit Filters**, the system synchronizes filtering conditions added in the dashboard to the saved search of the target Logstore, and adds the filtering conditions before the query statement by using the logical `AND` operator.|
        |**Filter**|Click the **Filter** tab, and set **Filter Statement**. The statement can contain any options under **Optional Parameter Fields**. If you have set **Filter**, when you click a value in the chart, the system automatically adds a query statement for the target saved search. The query statement is the value you specify in **Filter Statement**.

 |

    -   **Open Search Page**: enables drill-down analysis. The drill-down event is to open a search page.

        When you click a value in the chart, if you have set Variable, the system replaces the placeholder configured in the saved search statement with the chart value you clicked, and then performs a deeper query according to the chart value. If you have set Filter, the system automatically adds a query statement for the target saved search. You can specify a variable and a placeholder at the same time.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18631/156574883010243_en-US.png)

        |Parameter|Description|
        |:--------|:----------|
        |**Select Saved Search**|The name of the saved search that you want to jump to. For more information about how to configure a saved search, see [Saved search](reseller.en-US/Index and query/Query/Other functions.md).|
        |**Open in New Tab**|If you enable this option, when interaction behavior is triggered, the system opens the corresponding saved search in a new tab.|
        |**Time Range**|Configure the time range for the saved search that you want to jump to. Valid values:         -   **Default**: on the dashboard page, click a chart to jump to the saved search, and use the default time range 15 minutes for a saved search. This is a relative time range.
        -   **Inherit table time**: after you jump to the saved search by clicking the chart in the dashboard, the time range of the query statement is the table time configured in the dashboard by default when the event is triggered.
        -   Relative: set the time range of the target saved search to the specified relative time.
        -   Time Frame: set the time range of the target saved search to the specified time frame.
 Default value: **Default**.

 |
        |**Inherit Filters**|If you enable **Inherit Filters**, the system synchronizes filtering conditions added in the dashboard to the saved search, and adds the filtering conditions before the query statement by using the logical `AND` operator.|
        |**Filter**|Click the **Filter** tab, and set **Filter Statement**. The statement can contain any options under **Optional Parameter Fields**. If you set **Filter**, when you click a value in the chart, the system automatically adds a query statement for the target saved search. The query statement is the value you specify in **Filter Statement**.

 |
        |**Variable**|Click the **Variable** tab, click **Add Variable**, and then set the following parameters:         -   **Replace Variable Name**: the variable that triggers drill-down analysis. You can click this variable to jump to the target saved search.
        -   **Replace the value in the column**: the column where the value that you want to replace data with is located. To process multiple columns, you can specify the current column and other columns. The current column is the column that you want to perform drill-down analysis. This column is the column specified in **Replace the value in the column**. Other columns can be any other columns in the chart for drill-down analysis.
 When the query statement variable of the target saved search matches the name of the added variable, the system replaces the query statement variable with the chart value that triggers the drill-down event. This flexibly changes the query statement of the target saved search.

 **Note:** 

        -   To add a variable, you must first configure a placeholder in the query statement for the saved search that you want to jump to.
        -   You can add up to five variables.
 |

    -   **Open Dashboard**: enables drill-down analysis. The drill-down event is to open a dashboard.

        The chart in the dashboard is the chart-form result of the query statement. You need to pre-configure a placeholder in the query statement for the dashboard that you want to jump to. When you click a chart value in the upper-layer dashboard, if you have set Variable, the system replaces the pre-configured placeholder with the chart value. If you have set Filter, the system adds the filtering conditions for the target dashboard, and then performs a deeper query according to the chart value.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18631/156574883110244_en-US.png)

        |Parameter|Description|
        |:--------|:----------|
        |**Select Dashboard**|The name of the dashboard that you want to jump to. For more information about how to configure a dashboard, see [Create and delete a dashboard](reseller.en-US/Index and query/Query and visualization/Dashboard/Create and delete a dashboard.md).|
        |**Open in New Tab**|If you enable this option, when interaction behavior is triggered, the system opens the corresponding dashboard in a new tab.|
        |**Time Range**|Configure the time range for the dashboard that you want to jump to. Valid values:         -   **Default**: after you jump to the configured dashboard by clicking the chart in the current dashboard, the time range of the configured dashboard is the time configured in the current dashboard chart by default where the drill-down event is triggered.
        -   **Inherit table time**: after you jump to the target dashboard, the time range of the chart in the dashboard is the chart time configured in the dashboard by default when the event is triggered.
        -   Relative: set the time range of the target dashboard to the specified relative time.
        -   Time Frame: set the time range of the target dashboard to the specified time frame.
 Default value: **Default**.

 |
        |**Inherit Filters**|If you enable **Inherit Filters**, the system synchronizes filtering conditions added in the dashboard to the target dashboard, and adds the filtering conditions before the query statement by using the logical `AND` operator.|
        |**Filter**|Click the **Filter** tab, and set **Filter Statement**. The statement can contain any options under **Optional Parameter Fields**. If you set **Filter**, when you click a value in the chart, the system automatically adds a query statement for the target dashboard. The query statement is the value you specify in **Filter Statement**.

 |
        |**Variable**|Click the **Variable** tab, click **Add Variable**, and then set the following parameters:         -   **Replace Variable Name**: the variable that triggers drill-down analysis. You can click this variable to jump to the target dashboard.
        -   **Replace the value in the column**: the column where the value that you want to replace data with is located. To process multiple columns, you can specify the **default column** and other columns. The **default column** is the current column that you want to perform drill-down analysis. Other columns can be any other columns in the chart for drill-down analysis.
 When the query statement variable of the analysis chart in the target dashboard matches the name of the added variable, the system replaces the query statement variable with the chart value that triggers the drill-down event. This flexibly changes the query statement of the analysis chart in the target dashboard.

 **Note:** 

        -   To add a variable, you must first configure a placeholder in the query statement for the dashboard that you want to jump to.
        -   You can add up to five variables.
 |

    -   **Custom HTTP Link**: enables drill-down analysis. The drill-down event is to open a custom HTTP link.

        The path in the HTTP link that is the hierarchical path of the destination file. After you add optional parameter fields to the path in a custom HTTP link and click the chart content of the dashboard, the system replaces the added parameter fields with the chart value to jump to the relocated HTTP link.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18631/156574883110245_en-US.png)

        |Parameter|Description|
        |:--------|:----------|
        |**Enter Link**|The destination address that you want to jump to.|
        |**Optional Parameter Fields**|By clicking an optional parameter variable, you can replace part of the HTTP link with the chart value that triggers a drill-down event.|

6.  Click **Add to New Dashboard**, configure the dashboard, and then click **OK**.

    Afterward, you can view the analysis chart on the dashboard page, and click the chart to view deeper analysis results.


## Example {#section_sjb_xlq_y2b .section}

For example, you can store collected Nginx access logs in the Logstore named accesslog, display the common analysis scenarios of Nginx logs in the dashboard named RequestMethod, and display the trend of page view \(PV\) distribution over time in the dashboard named destination\_drilldown. You can configure drill-down analysis for the table of request methods, add the table to the RequestMethod dashboard, and then configure the drill-down event to jump to the destination\_drilldown dashboard. In the RequestMethod dashboard, click a request method to jump to the destination\_drilldown dashboard to view the corresponding PV trend.

Follow these steps:

1.  **Configure a target dashboard named destination\_drilldown**.

    1.  Filter logs according to request types and view the PV changes over time.

        The query statement is as follows:

        ``` {#codeblock_pxi_r28_2d1}
        request_method: * | SELECT date_format(date_trunc('minute', __time__), '%H:%i:%s') AS time, COUNT(1) AS PV GROUP BY time ORDER BY time
        ```

    2.  Use a line chart to display the query result and save the line chart to the dashboard.

        When you save the chart to the dashboard, specify the asterisk \(`*`\) as a placeholder named method. If the variable of the drill-down event that jumps to this saved search is also named method, you can replace the asterisk \(`*`\) with the chart value that you click to perform a query and analysis again.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18631/156574883110732_en-US.png)

2.  **Configure a chart that triggers drill-down analysis, and add the chart to the dashboard named RequestMethod**.

    1.  On the search page, use SQL to analyze the number of logs of each request method in the Nginx access logs, and display the result in a table.

        ``` {#codeblock_6yg_wt3_n6p}
        *|SELECT request_method, COUNT(1) AS c GROUP BY request_method ORDER BY c DESC LIMIT 10
        ```

        The query result is as follows.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18631/156574883110705_en-US.png)

    2.  Configure drill-down analysis for the `request_method` column in the table.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18631/156574883110708_en-US.png)

3.  **Click the GET request in the RequestMethod dashboard**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18631/156574883210714_en-US.png)

4.  **Jump to the destination\_drilldown dashboard**.

    The page automatically jumps to the dashboard configured in step [1](#step_1). The asterisk \(`*`\) in the query statement is replaced with `GET`, the chart value that you click. Afterward, the dashboard shows changes of the GET request PV over time.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18631/156574883210739_en-US.png)


