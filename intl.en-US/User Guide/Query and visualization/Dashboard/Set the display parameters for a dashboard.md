# Set the display parameters for a dashboard {#concept_yyl_dhj_kgb .concept}

This topic describes how to set the display parameters for a dashboard in Log Service. The display mode is the default viewing mode of a dashboard.

To open your target dashboard, perform either of the following two operations:

-   In the left-side navigation pane of the Logstores page, click **Dashboard**, and then click the target dashboard name.
-   In the left-side collapsed navigation pane of the Search & Analysis page, the Saved Search page, or any other page in the Log Service console, move your pointer over the pane to show the items, and then click the target dashboard name.

## Available settings {#section_kpd_2nk_kgb .section}

-   Settings for displaying a dashboard

    When display mode is selected for a dashboard, the following available function buttons are located in the upper-right corner of the dashboard \(left to right\): **Please select**, **Edit**, **Subscribe**, **Alerts**, **Refresh**, **Share**, **Full Screen**, **Title Configuration**, and **Reset Time**.

-   Settings for displaying a chart

    When display mode is selected for a dashboard, the list hidden in the upper-right corner of a chart provides functions for you to set parameters related to chart analysis results.

    **Note:** Different elements in a dashboard have different function lists.


## Set the query time range for a dashboard {#section_vrw_vmk_kgb .section}

All charts in a dashboard use the query time range that is set for the dashboard. To set a query time range exclusive to a single chart, follow the instructions described in [Set the query time range for a chart](#).

**Note:** A custom set query time range for a dashboard is a temporary setting that is not saved by the system. This means that when you re-open the dashboard to view charts, the system displays the query and analysis results of the default query time range.

1.  Click **Please Select**.
2.  Select a query time range.

    Available types of query time ranges for a dashboard are as follows:

    -   **Relative**: indicates to query the logs in an exact time range \(accurate to seconds\) of one minute, five minutes, fifteen minutes, or other time length that starts from the current time point. For example, if the current time point is 19:20:31 and you set this parameter to one hour, then the charts in the dashboard query logs generated from 18:30:31 to 19:20:31.
    -   **Time Frame**: indicates to query the logs in a whole time range at a one-minute, five-minute, fifteen-minute period, or any other time length period that starts from the current time point. For example, if the current time point is 19:20:31 and you set this parameter to one hour, then the charts in the dashboard query logs generated from 18:00:00 to 19:00:00.
    -   **Custom**: indicates to query the logs in a customized time range.
3.  Move your cursor to **Please Select** to verify that the time range that you set has taken effect.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/96158/155315312836994_en-US.png)


## Switch to editing mode {#section_ogy_1pk_kgb .section}

To enter editing mode, click **Edit**. For more information, see [Edit a dashboard](intl.en-US/User Guide/Query and visualization/Dashboard/Edit a dashboard.md).

## Set an alert notification {#section_zsn_mpk_kgb .section}

To create or modify an alert notification, choose **Alerts** \> **Create** or choose **Alerts** \> **Modify** in the upper-right corner. An alert must be associated with at least one chart.

For more information, see [Set an alert](intl.en-US//Set alarms.md).

## Set the refresh page frequency {#section_yrc_wpk_kgb .section}

You can manually refresh a dashboard, or set a time interval at which the dashboard automatically refreshes.

-   To manually refresh the dashboard, choose **Refresh** \> **Once**.
-   To set the dashboard to refresh automatically at specified time intervals, choose **Refresh** \> **Auto Refresh**. Then, select an interval.

    A dashboard can be automatically refreshed at 15-second intervals, 60-second intervals, 5-minute intervals, or 15-minute intervals.


**Note:** If no activity is detected within your browser and the screen goes to sleep, the time interval at which the dashboard refreshes may be affected.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/96158/155315312836995_en-US.png)

## Share a dashboard {#section_r1m_mqk_kgb .section}

To share a dashboard with other users, click **Share** to copy the link of the dashboard page and then send the link to users that have the permission to view the dashboard. Your current dashboard settings are saved in the shared dashboard page \(such as the query time range and the style to display chart titles\).

**Note:** Before you share a dashboard, you must grant the target user or users the permission to view the dashboard.

## Display a dashboard in full screen {#section_pch_2rk_kgb .section}

Click **Full Screen**. We recommend that you perform this operation if you want to focus on data or make a presentation.

## Set the chart title format {#section_iqc_3rk_kgb .section}

Click **Title Configuration**, and then select one of the following formats:

-   **Single-line Title and Time Display**
-   **Scrolling Title and Time Display**
-   **Alternate Title and Time Display**
-   **Title Only**
-   **Time Only**

## Reset the query time range {#section_fjc_xrk_kgb .section}

To restore the default query time ranges of all the charts in a dashboard, click **Reset Time**.

## Select chart view {#section_wwk_b5k_kgb .section}

-   **View analysis details of a chart**

    To view analysis details of a chart \(such as the query statement associated with a chart and the chart properties\) , choose **More Option** \> **View Analysis Details** in the upper-right corner of the chart.

-   **Set the query time range for a chart**

    To set the query time range for a chart, choose **More Option** \> **Select Time Range** in the upper-right corner of the chart. The setting only takes effect for the current chart.

-   **Set an alert notification for a chart**

    To set an alert notification for a chart, choose **More Option** \> **Create Alert** in the upper-right corner of the chart. For more information, see [Set an alert](intl.en-US//Set alarms.md).

-   **Download logs**

    To download logs, choose **More Option** \> **Download Log** in the upper-right corner of the chart. The raw log analysis results are downloaded as a .csv file.

-   **Download a chart**

    To download a chart, choose **More Option** \> **Download Chart** in the upper-right corner of the chart. The chart is downloaded as a .psd file.

-   **Check whether a drill-down analysis is set for a chart**

    To check whether drill-down analysis is set for a chart, move your cursor to the **More Option** button in the upper-right corner of the chart, and check the color of the icon at the bottom of the hidden list. A red icon indicates that a drill-down analysis is set for the chart. A gray icon indicates that no drill-down analysis is set for the chart.


![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/96158/155315312836996_en-US.png)

