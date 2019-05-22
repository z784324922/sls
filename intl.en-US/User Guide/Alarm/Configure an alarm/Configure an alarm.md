# Configure an alarm {#task_fdb_hfm_2fb .task}

This topic describes how to configure an alarm. After you configure an alarm, Log Service checks log data at specified intervals, and sends alarm notifications when the condition for an alarm to be triggered is met.

-   Log data is collected.
-   Log indexes are set. For more information, see [Enable and set indexes](reseller.en-US/User Guide/Index and query/Enable and set indexes.md).

Alarms are configured based on charts. When you view a chart, you can create an alarm based on the chart by clicking **Saved as Alarm**. You can also create an alarm based on a chart in a dashboard by choosing **More Option** \> **Create Alert** in the upper-right corner of the chart.

-   Create a chart and configure an alarm for the chart

    On the Search page, enter a search and analysis statement in the search box, and then click **Saved as Alarm**. In this case, you can create an alarm, create a chart that indicates the search and analysis statement, associate the alarm with the chart, and set the dashboard where the chart is attached.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13160/155851136743219_en-US.png)

-   Configure an alarm for the chart in a dashboard

    You can configure an alarm for one or more than one chart in a dashboard at a time. If you associate an alarm with multiple charts, you can add these charts to the condition for which an alarm is triggered.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13160/155851136743363_en-US.png)


The following shows the steps to configure an alarm for the chart in a dashboard.

**Note:** In a dashboard, if an update has been made to the search and analysis statement indicated by a chart that is associated with an alarm, you need to manually update the alarm. More specifically, you need to use the updated search and analysis statement to replace the search and analysis statement that was associated with the alarm. For more information, see [Modify an alarm configuration](reseller.en-US/User Guide/Alarm/Modify and view an alarm/Modify an alarm configuration.md#update_alarm).

For more information, see [Alarm configuration example](../reseller.en-US/FAQ/Alarm/Alarm configuration examples.md).

1.  Log on to the [Log Service console](https://partners-intl.console.aliyun.com/#/sls), and then click the target project name. 
2.  In the left-side navigation pane, click Dashboard. 
3.  Click the target dashboard name. 
4.  In the upper-right corner of the page, choose **Alert** \> **Create**. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13160/155851136743363_en-US.png)

5.  Set alarm parameters, and then click **Next**. 

    |Configuration|Description|
    |:------------|:----------|
    |**Alert Name**|Set the name of an alarm. It must be 4 to 64 characters in length.|
    |**Associated Chart**|Set the chart with which the alarm is associated with.Click **Add**, select a chart from the **Chart Name** drop-down list, and set a **Search Period**.

**Note:** **Search Period** refers to the time range in which data is measured. For the time range, you can choose either **Relative** or **Time Frame**. If you set the parameter as **15Minutes\(Relative\)** at 14:30:06, the system measures data with the period between 14:15:06- 14:30:06 relative to your time. However, if you set the parameter as **15Minutes\(Time Frame\)** at 14:30:06, the system measures data with the exact period between 14:15:00- 14:30:00.

To associate the alarm with multiple charts, add charts multiple times.

**Note:** Each chart has a sequence number, which appears before the chart name. You can use the sequence number of a chart to associate the alarm triggering condition with the chart.

|
    |**Search Interval**|Set the intervals \(in seconds, minutes, or hours\) at which the system detects the output of the charts associated with the alarm.The minimum interval is 60 seconds, and the maximum interval is 24 hours.

**Note:** Each time the system only detects the first one hundred log entries within the specified **Search Period** when the alarm is executed.

|
    |**Trigger Condition**|Enter an expression to set the condition for the alarm to be triggered. When the specified condition for the alarm to be triggered is met, the system sends an alarm notification according to your settings for **Search Interval** and **Notification Interval**.For example, you can set this parameter as `pv%100 > 0 && uv > 0`.

**Note:** You can use `$sequence number` to indicate a chart in the condition expression. For example, if you set `$0`, this means that the chart that has the sequence number of 0. For more information, see [How to view a chart sequence number?](reseller.en-US/User Guide/Alarm/Relevant syntax and fields for reference/Set an alarm condition expression.md#searchNO)

|
    |**Advanced**|
    |**Notification Trigger Threshold**|Set the threshold for the number of times for which the specified condition has to be triggered for alarm notifications to be sent. When the threshold you specified is reached, the system checks whether the specified **Notification Interval** is reached to determine whether to send an alarm notification.The default value of this parameter is 1. That is, each time the specified **Trigger Condition** is met, the system checks whether the specified **Notification Interval** is reached.

After an alarm notification is sent, the number of times counted by the system for when the condition was met is reset by the system. Network failures or other exceptions do not affect the overall count reported by the system.|
    |**Notification Interval**| Set the intervals at which alarm notifications are sent.

 If the output of associated charts meets the specified condition for the alarm to be triggered, and the specified **Notification Trigger Threshold** and **Notification Interval** values are reached, then the system sends an alarm notification. For example, if you set this parameter as five minutes, you may receive up to one alarm notification within 5 minutes. The default value of this parameter is 0.

 **Note:** **Notification Trigger Threshold** and **Notification Interval** help you control the number of alarm notifications that you receive for each event.

 |

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13160/15585113675775_en-US.png)

6.  Set notification types. You can set one or more notification types, including DingTalk chatbot, WebHook, and Notification Center.

    For more information, see [Set alarm notifications](reseller.en-US/User Guide/Alarm/Configure an alarm/Set alarm notifications.md).

    |Alarm notification type|Description|
    |:----------------------|:----------|
    |DingTalk chatbot| Alarm notifications are sent as DingTalk messages to a specified DingTalk group. To use this type of alarm notification, you must set **Request URL** and **Content**.

 **Content** refers to the content of a DingTalk message. It must be 1 to 500 characters in length, and can contain template variables.

 For more information about how to configure a DingTalk chatbot and how to obtain a **Request URL**, see [Set alarm notifications](reseller.en-US/User Guide/Alarm/Configure an alarm/Set alarm notifications.md).

 **Note:** Each DingTalk chatbot sends up to 20 alarm notifications per minute.

 |
    |WebHook| Alarm notifications are sent to a specified WebHook URL through a specified method. To use this type of alarm notification, you must set **Request URL**, **Request Method**, and **Request Content**.

 Available values of **Request Method** are GET, PUT, POST, DELETE, and OPTIONS. **Content** refers to the alarm notification content. It must be 1 to 500 characters in length, and can contain template variables.

 |
    |Message Center| Alarm notifications are sent to specified recipients through a method specified in Alibaba Cloud Message Center. To use this type of alarm notification, you must set **Content**. **Content** refers to the alarm notification content. It must be 1 to 500 characters in length, and can contain template variables. In addition, you need to set notification recipients and notification methods.

 |

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13160/155851136743364_en-US.png)

7.  Click **Submit**. 

[Manage alarm settings](reseller.en-US/User Guide/Alarm/Modify and view an alarm/Manage an alarm.md) or [View alarm records](reseller.en-US/User Guide/Alarm/Modify and view an alarm/View and use alarm logs.md).

