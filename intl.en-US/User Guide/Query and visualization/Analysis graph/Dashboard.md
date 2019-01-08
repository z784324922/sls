# Dashboard {#concept_osm_1nq_zdb .concept}

Log Service provides a dashboard for real-time data analysis. You can present frequently-used query and analysis statements as charts and save the charts to a dashboard. With the dashboard, you can view multiple analysis charts at the same time. That is, when you open or refresh the dashboard, each chart on the dashboard automatically executes its query and analysis statement.

Log Service also provides the [Console sharing embedment](reseller.en-US/User Guide/Query and visualization/Other visualization methods/Console sharing embedment.md) feature. You can view a dashboard in the Log Service console, and embed the dashboard on the pages of other websites. Therefore, you have more methods for data analysis and presentation. In addition, when you add a chart to a dashboard, you can configure [drill-down analysis](reseller.en-US/User Guide/Query and visualization/Analysis graph/Drill-down analysis.md). By clicking the chart on the dashboard page, you can obtain the results of analysis in a deeper data layer.

## Limits {#section_zw4_s3k_5cb .section}

-   Up to 50 dashboards can be created for each project.
-   Each dashboard can contain up to 50 analysis charts.

## Try a dashboard for free {#section_x1j_psp_b2b .section}

[Click here to start](https://signin-intl.aliyun.com/5806626462014816/login.htm?spm=5176.2020520153.10101.d1.61ee4945N5maW3&callback=https%3A%2F%2Fsls.console.aliyun.com%2Fnext%2Fproject%2Fmuzi-intl-hangzhou%2Fdashboard%2Fdashboard-show%3F).

Account: sls-reader1

Password: pnX-32m-MHH-xbm

## Create a dashboard { .section}

1.  On the Logstores page, click **Search** in the **LogSearch** column.
2.  In the search box, enter a query and analysis statement, and click **Search & Analysis**.
3.  On the **Graph** tab page, configure a chart.
4.  Click **Add to New Dashboard**.
5.  Set the dashboard parameters.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13143/15469406405695_en-US.png)

6.  Click **OK** to end the configuration.

    You can add multiple analysis charts to a dashboard by repeating the preceding operations.


The following figure shows a dashboard with multiple analysis charts.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13143/15469406415696_en-US.png)

## Modify a dashboard {#section_vsj_zmq_y2b .section}

1.  On the Logstores page, click **Search** in the **LogSearch** column.
2.  In the left-side navigation pane, click a dashboard name.
3.  Click **Edit** in the upper-right corner.

    In the editing view of the dashboard, you can perform the following operations:

    -   Modify the time range, size, and title of each analysis chart.
    -   Create [Markdown chart](reseller.en-US/User Guide/Query and visualization/Analysis graph/Markdown chart.md).
    -   Delete an analysis chart.
4.  Click **Save** in the upper-right corner.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13143/154694064110582_en-US.png)


## View a dashboard {#section_yrz_zmq_y2b .section}

In the left-side navigation pane of the search page, you can click a dashboard name to open the dashboard.

-   **Set the global time range of the dashboard**.

    1.  On the dashboard page, click **Pleases Select** in the upper-left corner.
    2.  On the displayed page, select a time range. You can set a time range type as **Relative**, **Time Frame**, or **Custom**.
    3.  Click **OK** when you set a custom time range.
    **Note:** 

    -   After you set a new time range, all the time ranges of the charts on the dashboard are changed to the new time range.
    -   By clicking **Reset Time** in the upper-right corner, you can restore the default time range for all the charts.
    -   When you click the Please Select time selector in the upper-left corner, you can view the charts of temporary time ranges on the current page. Next time when you view the data analysis results of the charts, the system still uses the default time range to display the results.
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13143/154694064110785_en-US.png)

-   **Add filtering conditions** to filter the analysis results displayed on the dashboard.
    1.  On the dashboard page, click **Add Filter** in the upper-left corner.
    2.  Set **Logstore**, **key**, and **value**.

        For example, you can add filtering conditions in the Logstore named oss to filter the access information of the logs in which the host field value is sls.console.aliyun.com.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13143/154694064210583_en-US.png)

-   **Set dashboard auto refresh**.
    1.  On the dashboard page, click **Auto Refresh** in the upper-right corner.
    2.  Set a time interval at which the dashboard page automatically refreshes.

        The dashboard automatically refreshes the page according to the set interval.


## Delete a dashboard {#section_ik3_43k_5cb .section}

When you do not need a dashboard, you can delete the dashboard. A deleted dashboard cannot be recovered.

1.  In the left-side navigation pane of the Logstores page, click **Dashboard**.
2.  Click **Delete** to delete the corresponding dashboard.
3.  In the displayed dialog box, click **Confirm**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13143/154694064210584_en-US.png)


