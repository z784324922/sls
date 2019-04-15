# Modify an alarm configuration {#concept_g2j_ng1_cgb .concept}

This topic describes how to modify an alarm configuration through using the example of updating the statement associated with the alarm.

## Limits {#section_m3s_kh1_cgb .section}

-   An alarm can be associated with two types of statements: search statements and search-and-analysis statements. After you associate a statement of either type with an alarm, you can only update the statement as a new version of the same type, rather than one of the other type.

    For example, after you associate the `request_method: GET` search statement with an alarm, you can change it to `error` \( search statement\), but you cannot change it to `error| select count(1) as c`\(search-and-analysis statement\).

-   For more information about how to modify an alarm configuration of an earlier version, see [Upgrade an alarm configuration to the latest version](reseller.en-US/User Guide/Alarm/Modify and view an alarm/Upgrade an alarm configuration to the latest version.md).

-   If you want to modify an alarm configuration of the latest version, you can click **Modify** on the Alarm page, or choose **Alarm** \> **Modify** on the dashboard page where the charts associated with the alarm is added.


## Procedure {#section_ibp_jh1_cgb .section}

1.  Log on to the [Log Service console](https://partners-intl.console.aliyun.com/#/sls) and click the project name.
2.  In the left-side navigation pane of the Logstores page, click **Dashboard**.
3.  Click the name of the target dashboard.
4.  In the progress bar, choose **Alert** \> **Modify**.
5.  On the right of the target statement, click the edit icon ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/79764/155530082434111_en-US.png).

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/79764/155530082443458_en-US.png)

6.  On the displayed page, edit the statement, and then click **OK**.

    **Note:** Before clicking **OK**, you can click **Preview** to preview the output of the new statement on the page.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/79764/155530082443459_en-US.png)

7.  Modify other parameters for the alarm as needed, such as **Search Interval** and **Trigger Condition**, and then click **Next**. For more information, see [Configure an alarm](reseller.en-US/User Guide/Alarm/Configure an alarm/Configure an alarm.md)
8.  Modify notifications for the alarm as needed. For more information, see [Set alarm notifications](reseller.en-US/User Guide/Alarm/Configure an alarm/Set alarm notifications.md).
9.  Click **Submit**.

    The most recent time at which an alarm configuration was updated is shown in the **Last Updated At** column of the Alarm page.


