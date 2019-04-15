# Alarm function overview {#concept_ddt_5ht_2fb .concept}

This topic describes the alarm mechanism, alarm configuration limits, and the statements used by an alarm in typical scenarios. With the alarm function provided by Log Service, you can create an alarm and associate it with the charts in a dashboard to monitor logged services in real time.

## Overview {#section_o1d_gyt_hhb .section}

An alarm is configured based on the data in specific charts in a dashboard. On the Search or **Dashboard** page of the Log Service console, you can configure an alarm. Specifically, you can set the condition for triggering an alarm and the alarm notifications. After you configure an alarm, Log Service checks the query results of the charts in a dashboard at specified intervals. If a query result meets the condition specified in your alarm rule, Log Service then sends an alarm notification. For more information, see [Configure an alarm](reseller.en-US/User Guide/Alarm/Configure an alarm/Configure an alarm.md).

**Note:** The alarm function has been upgraded in the Log Service. After the upgrade, you can retain an alarm of an earlier version because Log Service retains your alarm configuration information from before this upgraded version. However, we recommend that you upgrade all alarms of an earlier version to the latest version. For more information, see [Upgrade an alarm configuration to the latest version](reseller.en-US/User Guide/Alarm/Modify and view an alarm/Upgrade an alarm configuration to the latest version.md).

## Limits {#section_vsj_2x1_yfb .section}

|Configuration|Description|
|:------------|:----------|
|Charts associated with an alarm|Each alarm must be associated with a chart and can be associated with up to three charts.|
|Condition|An expression \(displayed as **Trigger Condition** in the console\) must be 1 to 128 characters in length.-   Only the first 100 log entries in the query output of a statement are analyzed to determine whether any log entries meet the the condition that you have set to trigger the alarm.
-   A condition can be used for up to 10,000 calculation.

|
|Log entry character|The system can use up to 1024 characters of a log entry \(output by a statement\) for calculation.|
|Search period|Each search and analysis statement can at most search log data from a period of 24 hours at most.|

## Statements used by an alarm {#section_j32_nst_2fb .section}

Alarms are based on the data in charts in a dashboard. Each chart shows the search results of a query statement or a search and analysis statement.

-   If you use a query statement, the system outputs the log entries that meet the conditions of the query statement.
-   If you use a search and analysis statement, the system collects the statistics of the log entries that meet the conditions of the statement and then outputs these statistics.

-   **Configure an alarm for the output of a query statement**

    In this example, a query statement of error is used to query the log entries that contain the word error within the last fifteen minutes, and the system outputs 144 log entries. Each log entry consists of key-value pairs. For this example, you can set an alarm for the value of a key.

    **Note:** If the system outputs more than 100 log entries for a query statement, an alarm only analyzes the first 100 log entries. This means that the alarm can be triggered only by log entries that meet the condition for triggering the alarm and also are among the first 100 log entries.

    ![](images/43201_en-US.png "Query statement")

-   **Configure an alarm for the output of a search and analysis statement**

    In this example, the following search and analysis statement is used to obtain the ratio of the log entries with a status code of the OK format in all log entries:

    ```
    * | select sum(case when status='ok' then 1 else 0 end) *1.0/count(1) as ratio
    ```

    **Note:** For more information, see [Query syntax](reseller.en-US/User Guide/Index and query/Query/Query syntax.md).

    ![](images/43203_en-US.png "Search and analysis statement")

    For this example, you can configure an alarm by setting the condition to trigger the alarm as the `ratio < 0.9`. This means that the alarm is triggered when the ratio of the log entries with status codes of the OK format in all log entries drops below 90%.


