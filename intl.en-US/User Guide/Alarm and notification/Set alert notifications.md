# Set alert notifications {#concept_bzz_vht_2fb .concept}

The alert function provided by Log Service allows you to set alert notifications through multiple methods, such as DingTalk, webhooks, and Message Center.

**Notification method:**

-   [DingTalk](#)
-   [Webhook](#)
-    [Message Center](#)

****[Notification content](#)

## DingTalk {#section_l15_111_12b .section}

You can configure Log Service to send alert notifications through DingTalk. Then, when an alert is triggered, the alert notification is sent to the configured DingTalk group as a chatbot message.

**Note:** Each chatbot can send up to 20 alert messages per minute.

**Procedure**

1.  Open DingTalk and select the target DingTalk group.
2.  In the upper-right corner, click Group Settings and click **ChatBot**.
3.  Select **Custom \(Custom message services via webhook\)**, and click **Add**.
4.  Enter a **ChatBot Name** and click **Finished**.
5.  Click **Copy** to copy the webhook address.
6.  [Set alarms](intl.en-US/User Guide/Alarm and notification/Set alarms.md) in the Log Service console, and then select **DingTalk Bot** from the notification drop-down list.
7.  In the **Request URL** area, paste the address copied in [Step 5](#). Then enter notification content in the**Content** area.

    ![](images/33231_en-US.png "Notification content")


## Webhook {#section_flh_db1_12b .section}

You can configure Log Service to send alert notifications through webhooks. Then, when an alert is triggered, the alert notification is sent to the custom webhook address through the specified method.

**Procedure**

1.  [Set alarms](intl.en-US/User Guide/Alarm and notification/Set alarms.md) in the Log Service console. Select **WebHook** from the notification drop-down list.
2.  In the **Request URL** area, enter your custom webhook address. Select a**Request Method**. Enter **Request Content**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21677/155166392833232_en-US.png)

    Then, when an alert is triggered, the alert content is sent to the custom webhook address through the specified method.

3.  Click **Submit**.

## Message Center \(recommended\) {#section_hrf_3zz_zdb .section}

In the Message Center console, you can set receivers of Log Service alert notifications. Then, when an alert is triggered, Message Center sends an alert notification by using the specified method.

**Procedure**

1.  [Set alarms](intl.en-US/User Guide/Alarm and notification/Set alarms.md)Select **Notifications** from the notification drop-down list.
2.  In the [Message Center](https://notifications.console.aliyun.com/?spm=5176.202052012811.aliyun_topbar.162.zRRPhO#/subscribeMsg) console, choose **Message Settings** \> **Common Settings**.

    ![](images/7260_en-US.png)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13160/15516639287269_en-US.png)

3.  In the **Account Contact** column of **Notification Type** \> **Log Service Alarm Notification**, click **Modify**.
4.  In the **Modify Contact** dialog box, select the required message receivers.

    china site**** china site

    **Note:** 

    -   china site
    -   You must set at least one message receiver.
    -   By default, notification messages are sent through email only and no other method can be set.
    -   **Up to fifty alert notification messages can be sent to a specified email inbox per day.**

## Notification content {#section_r31_b1c_yfb .section}

You must enter **notification content** for each notification. The template variables used when an alert is triggered can be referenced in the format of $\{fieldName\}. Log Service replaces the template variables of the **notification content** with the real values when sending an alert. For example, $\{Project\} will be replaced with the name of the Project to which the alert rules belong.

**Note:** You must reference valid variables. If a referenced variable does not exist or you reference an invalid variable, the variable will be processed as an empty string. If the value of the variable that you referenced is of the object type, the value is converted to a JSON string for display.

The following table describes all available variables and their corresponding reference methods.

|Variable|Description|Example|Reference example|
|:-------|:----------|:------|:----------------|
|Aliuid|Aliuid to which a Project belongs|1234567890|The alert rules of user $\{Aliuid\} is triggered.|
|Project|Project to which alert rules belong|my-project|The alert of Project $\{Project\} is triggered.|
|AlertID|Unique ID of the executed alert.|0fdd88063a611aa114938f9371daeeb6-1671a52eb23|The ID of an executed alert is $\{AlertID\}.|
|AlertName|Alert rule name, which must be unique in a Project|alert-1542111415-153472|Alert rule $\{AlertName\} is triggered.|
|AlertDisplayName|Displayed name of an alert rule|My alert rule|Alert $\{AlertDisplayName\} is triggered.|
|Condition|Condition expression that triggers an alert. Each variable in the condition expression is replaced with the value that triggers an alert, and is enclosed in square brackets.|\[5\] \> 1|An alert condition expression is in the format of $\{Condition\}.|
|RawCondition|Raw condition expression in which variables of the condition are not replaced with other values|count \> 1|A raw condition expression is in the format of $\{RawCondition\}.|
|Dashboard|Name of the dashboard associated with an alert|mydashboard|The dashboard associated with an alert is $\{Dashboard\}.|
|DashboardUrl|Address of the dashboard associated with an alert|https://sls.console.aliyun.com/next/project/myproject/dashboard/mydashboard|The address of the dashboard associated with an alert is $\{DashboardUrl\}.|
|FireTime|Time when an alert is triggered|2018-01-02 15:04:05|The triggering time of the alert is $\{FireTime\}.|
|FullResultUrl|URL used to query records of triggered alerts|https://sls.console.aliyun.com/next/project/my-project/logsearch/internal-alert-history?endTime=1544083998&queryString=AlertID%3A9155ea1ec10167985519fccede4d5fc7-1678293caad&queryTimeType=99&startTime=1544083968|Click to view details: $\{FullResultUrl\}.|
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

 |The first query starts at $\{Results\[0\]. StartTime\}, and ends at $\{Results\[0\]. EndTime\}. The value of the count parameter is $\{Results\[0\]. FireResult.count\}.|

