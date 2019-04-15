# Upgrade an alarm configuration to the latest version {#task_wfv_rs3_yfb .task}

This topic describes how to upgrade an alarm configuration of an earlier version to the latest version.

Alarms have been upgraded in the Log Service. Log Service has upgraded the alarm function. If you want to modify an alarm configuration of an earlier version, you need to add modifications to the alarm manually and upgrade it to the latest version. You can retain an alarm of an earlier version because Log Service retains your alarm configuration from before this upgrade. However, we recommend that you upgrade your alarm of an earlier version to the latest version.

The differences between an alarm configuration of an earlier version and the latest version are as follows:

-   **Alarm configurations of an earlier version**

    An alarm configuration created with an earlier version is not attached to any dashboard. That is, on the Alarm page, no information is shown in the **Dashboard** column of an alarm of an earlier version.

-   **Alarm configurations of the latest version**

    An alarm configuration created with the latest version is attached to a dashboard. That is, on the Alarm page, the **Dashboard** column of an alarm of the latest version shows the name of the dashboard to which the alarm is attached.


![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/65280/155532286833243_en-US.png)

1.  Log on to the [Log Service console](https://partners-intl.console.aliyun.com/#/sls), and then click the target project name. 
2.  In the left-side navigation pane, click **Alarm**. 
3.  In the **Actions** column of the alarm of an earlier version, click **Modify**. 

    **Note:** The **Dashboard** column of the alarm of an earlier version shows no information.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/65280/155532286933247_en-US.png)

4.  Set the alarm parameters, and then click **Next**. You only need to set the **Chart Name**, and the dashboard to which you attach the alarm. Log Service reserves the original configuration for you, such as the original **Alarm Name**, **Query Statement**, and Trigger Condition. For more information, see [Set an alarm](reseller.en-US/User Guide/Alarm/Configure an alarm/Configure an alarm.md).

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/65280/155532286933259_en-US.png)

5.  Set the notification method. By default, Log Service retains the notification method and content of the original alarm configuration. You can add one or multiple notification methods.
6.  Click **Submit**. After you upgrade an alarm configuration of the earlier version to the latest version, you can view the chart that was automatically created by the system in the dashboard where the alarm is attached. Moreover, you can view the records and configuration of the alarm on the Alert History Statistics page.

