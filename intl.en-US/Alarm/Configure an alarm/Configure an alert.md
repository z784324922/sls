# Configure an alert {#task_fdb_hfm_2fb .task}

You can configure an alert on a query page or a dashboard. After the alert is configured, Log Service checks log data at specified intervals, and sends an alert notification when the trigger condition for the alert is met.

-   Log data is collected.
-   Indexes are enabled and configured. For more information, see [Enable and set indexes](../reseller.en-US/Index and query/Enable and set indexes.md#).

Alerts are configured based on charts. When you view a chart, you can save the chart on a dashboard and configure an alert based on the chart. You can also configure an alert for existing charts on a dashboard.

-   Create a chart and configure an alert for the chart

    You can save the current query and analysis statement on a dashboard, and configure an alert for the statement. When configuring an alert on the query page, you must specify the name of the dashboard on which the chart is saved and the chart name.

    ![Create a chart](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13160/156825040143219_en-US.png)

-   Configure an alert for existing charts on a dashboard

    You can configure an alert for one or more charts on a dashboard at a time. When configuring an alert for multiple charts, you can specify a conditional expression for each chart and combine them into the trigger condition for the alert.

    ![Configure an alert](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13160/15682504015774_en-US.png)


This topic describes how to configure an alert for existing charts on a dashboard.

**Note:** If an alert is configured for a chart on a dashboard and you update the query and analysis statement of the chart, you also need to update the query and analysis statement in the alert configuration. For more information, see [Modify an alert configuration](reseller.en-US/Alarm/Modify and view an alarm/Modify an alarm configuration.md#update_alarm).

For more information about common alert configuration examples, see [Alert configuration examples](../reseller.en-US/FAQ/Alarm/Alarm configuration examples.md#).

1.  Log on to the [Log Service console](https://partners-intl.console.aliyun.com/#/sls), and then click the target project name. 
2.  In the left-side navigation pane, click the Dashboard icon.
3.  Click the target dashboard name.
4.  In the upper-right corner of the dashboard, choose **Alerts** \> **Create**. 

    ![Create Alert](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13160/15682504015774_en-US.png)

5.  Configure an alert and click **Next**. The following table describes the configuration parameters for an alert.

    |Parameter|Description|
    |:--------|:----------|
    |**Alert Name**|The name of the alert. The name must be 1 to 64 characters in length.|
    |**Associated Chart**|The chart with which the alert is associated. Click **Add**, set **Chart Name**, and then set **Search Period**. The **Search Period** parameter specifies the time range of log data that the server reads for running a data query task. You can select either a relative time or a time frame. For example, if you set **Search Period** to 15 minutes \(relative\) and query log data at 14:30:06, the server reads the log data that was written from 14:15:06 to 14:30:06 for running the data query task. If you set **Search Period** to 15 minutes \(time frame\) and query log data at 14:30:06, the server reads the log data that was written from 14:15:00 to 14:30:00 for running the data query task.

 To associate the alert with multiple charts, you only need to add and configure them separately. The number before the chart name is the sequence number of the chart in the alert configuration. You can use the sequence number to associate a chart with a conditional expression in the trigger condition.

 |
    |**Frequency**|The time interval at which the server checks log data according to the alert configuration. **Note:** Currently, the server samples and checks only the first 100 data entries each time the specified time interval arrives.

 |
    |**Trigger Condition**|The conditional expressions to determine whether the alert is triggered. When the trigger condition is met, the server sends an alert notification based on the specified frequency and notification interval. For example, you can enter `pv%100 > 0 && uv > 0`.

 **Note:** In the conditional expressions of the trigger condition, you can use `$Sequence number` to differentiate between conditional expressions for different associated charts. For example, you can use `$0` to identify the conditional expression for chart 0.

[How can I check the sequence number of a chart?](reseller.en-US/Alarm/Relevant syntax and fields for reference/Set an alarm condition expression.md#searchNO)

 |
    |**Advanced**|
    |**Notification Trigger Threshold**|The threshold for sending an alert notification based on the specified notification interval when the cumulative number of times that the trigger condition is met exceeds this threshold. If the trigger condition is not met, the overall count does not change. The default value of **Notification Trigger Threshold** is 1. That is, each time the specified trigger condition is met, the server checks whether the specified notification interval arrives.

 You can also specify this parameter to enable the server to send an alert notification after the trigger condition is met multiple times. For example, if you set this parameter to 100, the server checks whether the specified notification interval arrives only after the trigger condition is met 100 times. If the specified notification trigger threshold is reached and the specified notification interval arrives, the server sends an alert notification. Then, the overall count is reset. If the server fails to check log data due to exceptions such as a network failure, the overall count does not change.|
    |**Notification Interval**| The time interval at which the server sends an alert notification.

 If the trigger condition is met several times that exceed the specified notification trigger threshold and the specified notification interval arrives, the server sends an alert notification. If you set this parameter to 5 minutes, you can receive up to one alert notification every 5 minutes for the alert. The default value is No Interval.

 **Note:** By setting Notification Trigger Threshold and Notification Interval, you can control the number of alert notifications that you receive.

 |

    ![Alert configuration](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13160/15682504015775_en-US.png)

6.  Configure the alert notification method. You can select one or more notification methods, including **Email**, **WebHook-DingTalk Bot**, **WebHook-Custom**, and **Notifications**.

    For more information, see [Configure the alert notification method](reseller.en-US/Alarm/Configure an alarm/Configure the alert notification method.md#).

    |Notification method|Description|
    |:------------------|:----------|
    |**Email**| Sends alert notifications by Email. To use this notification method, you must specify email addresses as **Recipients** and set **Content**.

 Separate multiple email addresses with a comma \(,\). Enter the content of the email to be sent in the **Content** field, which must be 1 to 500 characters in length. Template variables are supported.

 |
    |**WebHook-DingTalk Bot**| Sends alert notifications by DingTalk. When an alert is triggered, DingTalk Chatbot sends an alert notification to a specified DingTalk group. To use this notification method, you must set **Request URL** and **Content**.

 Enter the content of the DingTalk message to be sent in the **Content** field, which must be 1 to 500 characters in length. Template variables are supported.

 For more information about how to configure DingTalk Chatbot and obtain the request URL, see [Configure the alert notification method](reseller.en-US/Alarm/Configure an alarm/Configure the alert notification method.md).

 **Note:** Each DingTalk Chatbot can send up to 20 alert notifications per minute.

 |
    |**WebHook-Custom**| Sends alert notifications to a specified webhook URL through a specified method. To use this notification method, you must set **Request URL**, **Request Method**, and **Request Content**.

 Valid values of **Request Method** are GET, PUT, POST, DELETE, and OPTIONS. Enter the content of the notification to be sent in the **Request Content** field, which must be 1 to 500 characters in length. Template variables are supported.

 |
    |**Notifications**| Sends alert notifications to specified contacts through the notification method specified in Alibaba Cloud Message Center. To use this notification method, you must set **Content**. Enter the content of the notification to be sent in the **Content** field, which must be 1 to 500 characters in length. Template variables are supported.

 In addition, you must specify contacts and the notification method in **Message Center**.

 |

    ![Alert notifications](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13160/15682504015776_en-US.png)

7.  Click **Submit**.

After configuring an alert, you can [manage alerts](reseller.en-US/Alarm/Modify and view an alarm/Manage an alarm.md) or [view alert logs](reseller.en-US/Alarm/Modify and view an alarm/View and use alarm logs.md).

