# View and use alarm logs {#concept_xtk_lqg_yfb .concept}

This topic describes how to view, search, and analyze alarm logs recorded in a Logstore that is created automatically, and also describes how to view the details of operation and notifications of all alarms in a dashboard that is created automatically.

## Background {#section_rpq_hdn_jhb .section}

-   Alarm logs stored in a Logstore

    When you create an alarm for the first time in a project, Log Service automatically creates a Logstore named **internal-alert-history** that records the data of all alarms in this project. Each time that an alarm in the project is executed, a log entry is generated to record the event no matter if the alarm is triggered. The log entry is stored in the **internal-alert-history** Logstore. For more information, see [Alarm log field](reseller.en-US/User Guide/Alarm/Relevant syntax and fields for reference/Alarm log fields.md).

    **Note:** This Logstore is free of charge. It cannot be deleted or modified. Each log entry is retained in this Logstore for seven days.

-   Details of alarm events displayed in a dashboard

    When you create an alarm for the first time in a project, Log Service automatically creates a dashboard named **Alert History Statistics** in the project to record and display all alarm events. The details of all alarm events in the project include the following information:

    -   The number of times in which alarm notifications are sent.
    -   The ratio of successful alarms to the total number of alarms.
    -   The ratio of alarms whose notifications are sent to the total number of successful alarms.
    -   The 10 alarms that are executed for the greatest number of times.
    -   The status of whether an alarm is executed or triggered.
    -   The status of whether the notifications of an alarm are sent.
    -   The cause for which an alarm failed to be triggered.
    -   Each error message and its description and solution.
    **Note:** This dashboard cannot be deleted or modified. You can use it free of charge.


## View alarm logs in the Logstore {#section_szq_nm3_yfb .section}

On the search page of the **internal-alert-history** Logstore, you can preview, search, and analyze alarm logs recorded in this Logstore. For more information, see [Alarm log field](reseller.en-US/User Guide/Alarm/Relevant syntax and fields for reference/Alarm log fields.md).

1.  Log on to the [Log Service console](https://partners-intl.console.aliyun.com/#/sls) and click the project name.
2.  Find the Logstore **internal-alert-history** and click **Search** in the **LogSearch** column.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/65200/155532268233240_en-US.png)

3.  Search for and analyze alarm logs as needed.

## View alarm records in a dashboard {#section_wcn_pm3_yfb .section}

1.  Log on to the [Log Service console](https://partners-intl.console.aliyun.com/#/sls) and click the project name.
2.  In the left-side navigation pane of the Logstores page, click **Alarm**.
3.  Click any alarm name to open the **Alert History Statistics** dashboard.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/65200/155532268233241_en-US.png)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/65200/155532268233242_en-US.png)


