# Set alarm notifications {#concept_bzz_vht_2fb .concept}

This topic describes how to set the DingTalk chatbot message, WebHook, and Message Center as the notification method for an alarm.

## DingTalk {#section_l15_111_12b .section}

You can configure Log Service to send alarm notifications as DingTalk chatbot messages to the specified DingTalk group. Furthermore, you can specify group members to be reminded by an at \(@\).

**Note:** Each chatbot can send up to 20 alarm messages per minute.

**Procedure**

1.  Open DingTalk and select the target DingTalk group.
2.  In the upper-right corner, click Group Settings and click **ChatBot**.
3.  Select **Custom \(Custom message services via webhook\)**, and click **Add**.
4.  Enter a **ChatBot Name** and click **Finished**.
5.  Click **Copy** to copy the WebHook URL.
6.  [Create an alarm](reseller.en-US/User Guide/Alarm/Configure an alarm/Configure an alarm.md) in the Log Service console, and then select **WebHook-DingTalk Bot** from the notification drop-down list.
7.  In the **Request URL** box, paste the URL copied in [Step 5](#). In the **Tagged List** box, enter the cell phone numbers of the group members to which you want to notification sent, and use commas \(,\) to separate different cell phone numbers.
8.  Edit the content of the notification in the **Content** area.

    Log Service provides default content for notifications. You can customize this content according to your needs.

    To call attention of one or more group members by using ats \(@\), enter `@cell phone number`.

    ![](images/43128_en-US.png "Content of Notification")


## WebHook {#section_flh_db1_12b .section}

You can set WebHook as the notification method for alarm notifications to be sent to a specified URL.

**Note:** The timeout limit of this type of notification is five seconds. If no response is received within five seconds after a request is sent, Log Service determines that the request as a failed request.

**Procedure**

1.  [Create an alarm](reseller.en-US/User Guide/Alarm/Configure an alarm/Configure an alarm.md) in the Log Service console. Select **WebHook-Custom** from the notification drop-down list.
2.  In the **Request URL** box, enter a customized WebHook URL. Select a **Request Method**.
3.  \(Optional\) Click **Add Request Headers** to add request headers.

    By default, the `Content-Type: application/json;charset=utf-8` request header is included in the notification.

4.  Enter **Request Content**.

    ![](images/43353_en-US.png "Notification content")

5.  Click **Submit**.

## Message Center \(recommended\) {#section_hrf_3zz_zdb .section}

In the Message Center console, you can set receivers of Log Service alarm notifications. Then, when an alarm is triggered, Message Center sends an alarm notification by using the specified method.

**Procedure**

1.  [Configure an alarm](reseller.en-US/User Guide/Alarm/Configure an alarm/Configure an alarm.md)Select **Notifications** from the notification drop-down list.
2.  In the [Message Center](https://notifications.console.aliyun.com/?spm=5176.202052012811.aliyun_topbar.162.zRRPhO#/subscribeMsg) console, choose **Message Settings** \> **Common Settings**.

    ![](images/7260_en-US.png)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13160/15553006737269_en-US.png)

3.  In the **Account Contact** column of **Notification Type** \> **Log Service Alarm Notification**, click **Modify**.
4.  In the **Modify Contact** dialog box, select the required message receivers.

    china site**** china site

    **Note:** 

    -   china site
    -   You must set at least one message receiver.
    -   By default, notification messages are sent through email only and no other method can be set.
    -   **Up to fifty alarm notification messages can be sent to a specified email inbox per day.**

## Notification content {#section_r31_b1c_yfb .section}

You must enter **notification content** for each notification. The template variables used when an alarm is triggered can be referenced in the format of $\{fieldName\}. Log Service replaces the template variables of the **notification content** with the real values when sending an alarm. For example, $\{Project\} will be replaced with the name of the Project to which the alarm rules belong.

**Note:** You must reference valid variables. If a referenced variable does not exist or you reference an invalid variable, the variable will be processed as an empty string. If the value of the variable that you referenced is of the object type, the value is converted to a JSON string for display.

The following table describes all available variables and their corresponding reference methods.

|Variable|Description|Example|Reference example|
|:-------|:----------|:------|:----------------|
|Aliuid|Aliuid to which a Project belongs|1234567890|The alarm rules of user $\{Aliuid\} is triggered.|
|Project|Project to which alarm rules belong|my-project|The alarm of Project $\{Project\} is triggered.|
|AlertID|Unique ID of the executed alarm.|0fdd88063a611aa114938f9371daeeb6-1671a52eb23|The ID of an executed alarm is $\{AlertID\}.|
|AlertName|Alert rule name, which must be unique in a Project|alarm-1542111415-153472|Alert rule $\{AlertName\} is triggered.|
|AlertDisplayName|Displayed name of an alarm rule|My alarm rule|Alert $\{AlertDisplayName\} is triggered.|
|Condition|Condition expression that triggers an alarm. Each variable in the condition expression is replaced with the value that triggers an alarm, and is enclosed in square brackets.|\[5\] \> 1|An alarm condition expression is in the format of $\{Condition\}.|
|RawCondition|Raw condition expression in which variables of the condition are not replaced with other values|count \> 1|A raw condition expression is in the format of $\{RawCondition\}.|
|Dashboard|Name of the dashboard associated with an alarm|mydashboard|The dashboard associated with an alarm is $\{Dashboard\}.|
|DashboardUrl|Address of the dashboard associated with an alarm|https://sls.console.aliyun.com/next/project/myproject/dashboard/mydashboard|The address of the dashboard associated with an alarm is $\{DashboardUrl\}.|
|FireTime|Time when an alarm is triggered|2018-01-02 15:04:05|The triggering time of the alarm is $\{FireTime\}.|
|FullResultUrl|URL used to query records of triggered alarms|https://sls.console.aliyun.com/next/project/my-project/logsearch/internal-alarm-history?endTime=1544083998&queryString=AlertID%3A9155ea1ec10167985519fccede4d5fc7-1678293caad&queryTimeType=99&startTime=1544083968|Click to view details: $\{FullResultUrl\}.|
|Results|Parameters and results of a query, and array type.| ```
[
  {
    "EndTime": 1542507580,
    "FireResult": {
      "__time__": "1542453580",
      "count": "0"
    },
    "LogStore": "test-logstore",
    "Query": "* | SELECT COUNT(*) as count",
    "RawResultCount": 1,
    "RawResults": [
      {
        "__time__": "1542453580",
        "count": "0"
      }
    ],
    "StartTime": 1542453580
  }
]
```

 |The first query starts at $\{Results\[0\]. StartTime\}, and ends at $\{Results\[0\]. EndTime\}. The value of the count parameter is $\{Results\[0\]. FireResult.count\}.**Note:** In this example, 0 indicates the sequence number of a chart \(the visualization form of a search and analysis statement\). For more information, see [How to view the sequence number of a chart?](reseller.en-US/User Guide/Alarm/Relevant syntax and fields for reference/Set an alarm condition expression.md#searchNO)

|

