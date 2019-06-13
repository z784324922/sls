# Set and use a filter in a dashboard {#concept_ovd_jbb_mfb .concept}

This topic describes how to set and use a filter in a Log Service dashboard. A filter in a dashboard can help you refine search results or replace placeholder variables with specified values across the whole dashboard.

## Overview {#section_8nu_uy6_gpi .section}

Each chart in a Log Service dashboard functions as a search-and-analysis statement. By setting a filter in a dashboard, you are performing an action for each search-and-analysis statement that is indicated by a corresponding chart in the dashboard at one time. To do so, add one or more conditions for filtering the result of each search-and-analysis statement, or replace placeholder variables in each search-and-analysis statement with a specified value. You can set the two following types of filters in a dashboard.

-   **Filter** type: Add a key value as a condition before the search-and-analysis statement`[search query]` to filter the result of the search-and-analysis statement. Then, the new search-and-analysis statement can be executed as `key: value AND [search query]`. This indicates to search for logs containing `key:value` in the result of the original search-analysis.

    For this type of filters, you can add multiple key values. A filter that contains multiple key values can filter out the logs that contain any of the key values from the search result of the search-and-analysis statement.

-   **Replace Variable** type: Set a value to replace the placeholder variables in the search-and-analysis statements that are indicated by corresponding charts.

## Components {#section_bry_bbc_mfb .section}

Each filter contains the following two core components:

-   The key value component, which indicates a filter operation.
-   The list item component, which corresponds to the key.

## Prerequisites {#section_nww_frc_mfb .section}

-   An index is enabled and configured. For more information, see [Enable and set indexes](reseller.en-US/User Guide/Index and query/Enable and set indexes.md).
-   A dashboard is created. For more information, see [Dashboard](reseller.en-US/User Guide/Query and visualization/Dashboard/Create and delete a dashboard.md).
-   A placeholder variable in the target search-and-analysis statement is set. This is so that you can use a filter to replace the placeholder variable in the search-and-analysis statement with a specific value.

## Procedure {#section_gdp_tpc_mfb .section}

1.  Log on to the [Log Service console](https://partners-intl.console.aliyun.com/#/sls), and then click the target project name.
2.  Find the target Logstore, and then click **Search** in the **LogSearch** column.
3.  In the left-side navigation pane, click the name of the target dashboard.
4.  In the progress bar of the dashboard, click **Edit**.
5.  Click the icon ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23712/156041954236998_en-US.png), and then set the filter configuration parameters:

    -   **Chart Name**: the filter name.
    -   **Title**: Turn on this switch to display the filter title in the dashboard.
    -   **Border**: Turn on this switch to display the filter borders.
    -   **Background**: Turn on this switch to add a white background for the filter.
    -   **Key**: There are Filter type and Replace Variable type keys. The key place holder variable must be as the one set in Prerequisites.
        -   For the Filter type, set this parameter as a condition to filter the execution result of the search-and-analysis statement.
        -   For the Replace Variable type, set this parameter as a placeholder variable.
    -   **Type**: Select **Filter** or **Replace Variable**.

        -    If you select **Filter**, a List Item refers to the value of the condition used to filter the results of a search-and-analysis statement.
        -   If you select Replace Variable, a **List Item** refers to the value that replaces a specific variable placeholder.
        For both of these types, you can set more than one value. Furthermore, after you set a filter containing multiple values in a dashboard, you can select a value as needed when viewing the dashboard.

    -   **Alias**: Set the column alias.

        This parameter is only required by the **Filter** type. After you set a column alias for a filter, the alias is shown in the filter in the dashboard.

    -   **Global filter**: Set whether to filter a specific value globally \(that is, applied to all fields\).

        By default, this parameter is disabled. Furthermore, you can only set this parameter for the **Filter** type.

    -   **List Item**: There are two types **Static List Item** and **Dynamic List Item**.
        -   **Static List Item**: You need to manually add list items to the filter. To add items, in the **Add Static List Item** box, enter list items and click **Add**.
        -   **Dynamic List Item**: Set the system to dynamically display list items in the filter. The list items are the results of the search-and-analysis statement that you set. To enable the **Add Dynamic List Item** switch, enter a search-and-analysis statement, and then click **Search**. Then, the dynamic list items are displayed.
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23712/156041954213760_en-US.png)

6.  Click **OK**.

## Examples {#section_ohf_5bc_mfb .section}

In the case that you have collected Nginx logs, you can analyze the Nginx logs by one of the two following methods. \(For information about how to collect Nginx logs, see [Collect and analyze Nginx access logs](../reseller.en-US/Quick Start/Collect and analyze Nginx access logs.md#).\)

-   **Example 1: Analyze the logs by using different time granularities** 

    You can use a search-and-analysis statement to view the PV in terms of different time granularities, such as the second or minute granularity. To change the time granularity to be used, you must change the value of `__time__ - __time__ % 60`. Changing the search-and-analysis statement is the typical method, which is inefficient for multiple times of operations.  If you set a filter, you can more quickly replace the target placeholder variable with a specific value.

    1.  Use the following search-and-analysis statement to view the PV each minute:

        ``` {#codeblock_tm3_9hs_zjt}
        * | SELECT date_format(__time__ - __time__ % 60, '%H:%i:%s') as time, count(1) as count GROUP BY time ORDER BY time
        ```

    2.  Add the search-and-analysis statement as a chart to the target dashboard, select `60` in the statement to generate a placeholder variable, and set the variable as `interval`. The default value of this variable is 60.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23712/156041954213757_en-US.png)

    3.  Set a filter in the target dashboard.

        -   **Type**: Select **Replace Variable**.
        -   **Key**: Enter `interval`.
        -   **Static List Item**: Add `1` \(each second\) and `120` \(every two minutes\) as the static list items.
        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23712/156041954213758_en-US.png)

    4.  In the target dashboard, select `1` from the **Search** drop-down list of the filter to view the data recorded in seconds.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23712/156041954313759_en-US.png)

        **Note:** The search-and-analysis statement that has a new placeholder variable value is then changed to the following:

        ``` {#codeblock_f59_jhw_h82}
        * | SELECT date_format(__time__ - __time__ % 1, '%H:%i:%s') as time, count(1) as count GROUP BY time ORDER BY time 
        ```

-   **Example 2: Dynamically switch filter methods** 

    By adding dynamic list items to a filter, you can dynamically switch different request methods. In example 1, the search-and-analysis statement that starts with `*` means no condition is set to filter the results of the statement with the statement applies to all logs. You can add a new filter to view the PV data of different request methods.

    1.  Set a filter in the target dashboard.

        -   **Type**: Select the **Filter** type.
        -   **Key**: Enter `method`.
        -   **Select Logstore**: Select the Logstore where the target dashboard belongs.
        -   **Add Dynamic List Item**: Enable this switch, and enter a search-and-analysis statement.
        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23712/156041954213760_en-US.png)

    2.  In the target dashboard, select `POST` from the **Search** drop-down list of the filter.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23712/156041954313761_en-US.png)

        **Note:** The chart only displays the data of the accesses that use the `POST` method. The search-and-analysis statement is then changed to the following:

        ``` {#codeblock_ptj_mtg_34e}
        (*) and (method: POST) | SELECT date_format(__time__ - __time__ % 60, '%H:%i:%s') as time, count(1) as count GROUP BY time ORDER BY time 
        ```


