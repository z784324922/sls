# Dashboard filter {#concept_ovd_jbb_mfb .concept}

A filter applied to a Log Service dashboard can help you refine a query or replace placeholder variables across the whole dashboard.

All charts in Log Service function as query analysis statements. This means that to add a filter to the dashboard is to add filtering conditions to all charts, or replace specified placeholder variables across all charts. You can configure a filter as one of the following two types:

-   **Filter** type: Filter type, which specifies a key and value whereby you then add the key and value as a filtering condition before `[search query]`. The new query statement is then `key: value AND [search query]`, which indicates to search in the result of the original query statement for logs containing `key:value`.
-   **Replace Variable** type: Specify a placeholder variable. If the dashboard has a chart in which the placeholder variable is configured, the placeholder variable of the query statement in the chart is replaced with the selected value.

## Components {#section_bry_bbc_mfb .section}

Each filter chart can consist of one or multiple filters. Each filter generally contains the following elements:

-   Key value, which indicates a filter operation.
-   List item, which corresponds to the key.

## Limits {#section_o1d_3vc_mfb .section}

-   Up to 5 filters can be configured for each dashboard.
-   In a filter of the **Filter** type, you can select multiple values, or enter a custom value in the **Please enter** box. When multiple values are selected, the filter conditions are in an `OR` relationship.

## Prerequisites {#section_nww_frc_mfb .section}

1.  You have enabled and configured an index.
2.  You have created a [Dashboard](reseller.en-US/User Guide/Query and visualization/Analysis graph/Dashboard.md) and configured a placeholder variable.

## Procedure {#section_gdp_tpc_mfb .section}

1.  登录[日志服务控制台](https://partners-intl.console.aliyun.com/#/sls)，单击Project名称。
2.  Click **Search** in the **LogSearch** column.
3.  In the left-side navigation pane, click the configured dashboard name.
4.  In the upper-right corner of the dashboard page, click **Add Filter**.
5.  Configure display settings for the filter in the dashboard.

    |Configuration|Description|
    |:------------|:----------|
    |**Chart name**|Filter chart name.|
    |**Show border**|Turn on the show border switch to add borders for the filter chart.|
    |**Show title**|Turn on the show title switch to display the filter chart title in the dashboard.|
    |**Show background**|Turn on the show background switch to add a white background for the filter chart.|

6.  Click **Add Filter**, configure the filter, and click **OK**.

    |Configuration|Description|
    |:------------|:----------|
    |**Type**|Types of filters, including:    -   Filter
    -   Replace Variable
|
    |**Key value**|     -   For the **Filter** type, **Key value** is the key of the filtering condition.
    -   For the **Replace Variable** type, **Key value** is the configured placeholder variable.
 **Note:** The placeholder variable must be a placeholder variable configured in [Prerequisites](#). Otherwise, it cannot be replaced.

 |
    |**List item**|List items pre-configured in the filter, where:    -   For the **Filter** type, **List item** is the value of the filter condition. You can configure multiple values. After the filter is generated, select values as needed when you view the dashboard.
    -   For the **Replace Variable** type, **List item** is the replacement value of the configured placeholder variable. You can configure multiple replacement values. After the filter is generated, select replacement values as needed when you view the dashboard.
**Note:** Enter a list item value in the box on the right of the **List Item**, and click **Add List Item**.

|
    |**Drop-down mode**|Configure the display box type for the list items.    -   If you turn this switch on, the list items are displayed in a drop-down list.
    -   If you turn this switch off, the list items are displayed as options.

        -   The **Filter** type uses check boxes.
        -   The **Replace Variable** type uses radio buttons.
|

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23712/154166178513760_en-US.png)


The current dashboard page is automatically refreshed to show the new filter configuration. In the **Please select** drop-down list, select a value or a replacement value of the placeholder as needed, and click **Add**.

In a filter of the **Filter** type, you can select multiple values, or enter a custom value in the **Please Enter** box. When multiple values are selected, the filter conditions are in an `OR` relationship.

## Scenarios {#section_ojx_sbc_mfb .section}

Filters are mostly used in the current dashboard to dynamically modify query conditions and replace the existing placeholder variables in charts with new variables. Each chart functions as a query and analysis statement in the form of`[search query] | [sql query]`. This means that filters operate on this statement.

-   A filter of the Filter type adds the filter value followed by `AND` before `[search query]` to make a new query statement, that is, `key: value AND [search query]`.
-   A filter of the replace variable type searches for charts that have placeholder variables in the entire dashboard, and replaces the placeholder variables with the selected `values`.

## Example {#section_ohf_5bc_mfb .section}

In this example, assume that you have [collected Nginx logs](../reseller.en-US/Quick Start/Analysis - Nginx access logs.md) and you want to query and analyze the collected logs in real time.

-   **Scenario 1: based on different time granularity**

    By using a query and analysis statement, you can view the PV per minute. To view data measured in seconds, you must modify the value of `__time__ - __time__ % 60`. In standard methods, you would need to modify the query and analysis statement, but this process is inefficient for querying second-level data multiple times. In this case, you can use a filter to replace the variable.

    1.  Use the following statement to view data of PV per minute.

        ```
        * | SELECT date_format(__time__ - __time__ % 60, '%H:%i:%s') as time, count(1) as count GROUP BY time ORDER BY time
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23712/154166178513756_en-US.png)

    2.  Add the chart to the dashboard, select `60` as the default value of the placeholder variable, and enter `interval` as the variable name.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23712/154166178613757_en-US.png)

    3.  Add a filter and select the **Replace Variable** type. Where:

        -   **Type** is Replace Variable.
        -   **Key value** is `interval`.
        -   **List item** is `1` \(that is, per second\) and `120` \(that is, per two minutes\).
        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23712/154166178613758_en-US.png)

    4.  Select `1` in the filter. Now the dashboard displays data measured in seconds.

        The query statement with the variable replaced is as follows:

        ```
        * | SELECT date_format(__time__ - __time__ % 1, '%H:%i:%s') as time, count(1) as count GROUP BY time ORDER BY time 
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23712/154166178613759_en-US.png)

-   **Scenario 2: dynamically switch filtering methods**

    By using filters, you can switch different request methods dynamically. The query statement in scenario 1 starts with `*`, which means no filtering condition is configured \(that is, all logs are in the query scope\). You can add one more filter to view access statistics of another `request_method`.

    1.  Add a new filter in the dashboard in scenario 1 as follows.

        -   **Type** is filter.
        -   **Key value** is `request_method`.
        -   **List item** includes: `GET`, `POST`, and `PUT`.
        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23712/154166178513760_en-US.png)

    2.  In the drop-down list of the filter, select `GET`, and enter `DELETE`.

        The chart displays only the access statistics of `request_method` of`GET` and`DELETE`. The query and analysis statement is then changed to the following:

        ```
        (*) and (request_method: GET OR request_method: DELETE) | SELECT date_format(__time__ - __time__ % 60, '%H:%i:%s') as time, count(1) as count GROUP BY time ORDER BY time 
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23712/154166178613761_en-US.png)


