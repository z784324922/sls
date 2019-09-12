# Configure the alert notification method {#concept_bzz_vht_2fb .concept}

The alerting feature provided by Log Service allows you to select one or more notification methods, including Email, WebHook-DingTalk Bot, WebHook-Custom, and Notifications.

**Notification methods:** 

-   [Email](#)
-   [WebHook-DingTalk Bot](#)
-   [WebHook-Custom](#)
-   [Notifications](#)

**Content**: For more information, see [Notification content](#) in this topic.

## Email {#section_enz_jzb_yfb .section}

You can configure Log Service to send alert notifications by email. When an alert is triggered, Log Service sends an email to specified email addresses.

**Procedure** 

1.  [Configure an alert](reseller.en-US/Alarm/Configure an alarm/Configure an alert.md) in the Log Service console. Select **Email** from the Notifications drop-down list.
2.  Enter one or more email addresses to receive alert notifications in the **Recipients** field, and enter the email subject in the **Subject** field.

    The email subject can be up to 128 characters in length. For example, you can enter `Log Service Alert`.

3.  Enter the email content in the **Content** field.

    Separate multiple email addresses with a comma \(,\). The content of the email to be sent in the **Content** field must be 1 to 500 characters in length. Template variables are supported.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21677/156824971933229_en-US.png)

4.  Click **Submit**.

## WebHook-DingTalk Bot {#section_l15_111_12b .section}

You can configure Log Service to send alert notifications by DingTalk. When an alert is triggered, DingTalk Chatbot sends an alert notification to a specified DingTalk group. You can also specify group members to be reminded by an at sign \(@\).

**Note:** Each DingTalk Chatbot can send up to 20 alert notifications per minute.

 **Procedure** 

1.  Open DingTalk on your computer and select the target DingTalk group.
2.  In the upper-right corner of the chatbox, click the Group Settings icon and click **ChatBot**.
3.  Select **Custom \(Custom message services via Webhook\)**, and click **Add**.
4.  Set **ChatBot Name** and click **Finished**.
5.  Click **Copy** to copy the webhook URL.
6.  [Configure an alert](reseller.en-US/Alarm/Configure an alarm/Configure an alert.md) in the Log Service console. Select **WebHook-DingTalk Bot** from the Notifications drop-down list.
7.  In the **Request URL** field, paste the webhook URL copied in [step 5](#). Set **Tagged List**.

    In the **Tagged List** field, enter the mobile numbers of group members who you want to remind. Separate multiple mobile numbers with a comma \(,\).

8.  Enter the notification content in the **Content** field.

    By default, the content to be sent is configured. You can also modify and customize the content.

    To remind one or more group members by using an at sign \(@\), you must add mobile numbers in `@Mobile number` format to the **Content** field.

    ![Notification content](images/33231_en-US.png "Enter the notification content")


## WebHook-Custom {#section_flh_db1_12b .section}

You can configure Log Service to send alert notifications by using a webhook. When an alert is triggered, Log Service sends an alert notification to a specified webhook URL through a specified method.

**Note:** The timeout period of the **WebHook-Custom** notification method is 5 seconds. If no response is received within 5 seconds after a request is sent, Log Service regards the request as failed.

 **Procedure** 

1.  [Configure an alert](reseller.en-US/Alarm/Configure an alarm/Configure an alert.md) in the Log Service console. Select **WebHook-Custom** from the Notifications drop-down list.
2.  Enter your custom webhook URL in the **Request URL** field. Set **Request Method**.
3.  Optional. Click **Add Request Headers** to add request header fields.

    By default, the request header contains the field `Content-Type: application/json;charset=utf-8`. You can add request header fields as needed.

4.  Enter the notification content in the **Request Content** field.

    ![Alert notification by WebHook-Custom](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21677/156824971933232_en-US.png)

    When an alert is triggered, Log Service sends the specified notification content to the custom webhook URL through the specified method.

5.  Click **Submit**.

## \(Recommended\) Notifications {#section_hrf_3zz_zdb .section}

In the Alibaba Cloud Message Center console, you can specify the contacts of Log Service alert notifications. When an alert is triggered, Log Service sends an alert notification to specified contacts through the notification method specified in Message Center.

 **Procedure** 

1.  [Configure an alert](reseller.en-US/Alarm/Configure an alarm/Configure an alert.md#). Select **Notifications** from the Notifications drop-down list.
2.  In the [Message Center](https://notifications.console.aliyun.com/?spm=5176.202052012811.aliyun_topbar.162.zRRPhO#/subscribeMsg) console, choose **Message Settings** \> **Common Settings** in the left-side navigation pane.

    ![Message settings on the international site](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13160/15682497197269_en-US.png)

3.  Find **Log Service Alarm Notification** in the **Notification Type** column and click **Modify** under Account Contact in the **Contact** column.
4.  In the **Modify Contact** dialog box, select required alert contacts.

    To add a contact, click **+ Add Receiver**, and then specify the email address, mobile number, and position for the contact to receive alert notifications. Only the Alibaba Cloud account owner can specify the mobile number for contacts.

    **Note:** 

    -   The system automatically sends a verification link to the specified mobile number and email address of an added contact. The contact can receive alert notifications only after clicking the verification link to confirm the contact information.
    -   You must specify at least one alert contact.
    -   By default, Message Center supports only email as notification methods. Other methods are not supported.
    -   Up to 50 alert notifications can be sent to each specified email address per day.

## Notification content {#section_r31_b1c_yfb .section}

You must set **Content** for each notification method. In the notification content, you can also reference some template variables in $\{fieldName\} format for the alert to be triggered. When sending an alert notification, Log Service replaces the template variables referenced in **Content** with real values. For example, it replaces $\{Project\} with the name of the project to which the alert belongs.

**Note:** You must reference valid variables. If a referenced variable does not exist or you reference an invalid variable, Log Service processes this variable as a null string. If the value of a referenced variable is of the object type, the value is converted and displayed as a JSON string.

The following table describes all available variables and how to reference these variables for an alert.

|Variable|Description|Example|Reference example|
|:-------|:----------|:------|:----------------|
|Aliuid|The AliUid to which the project belongs.|1234567890|The alert configured by the user $\{Aliuid\} is triggered.|
|Project|The project to which the alert belongs.|my-project|The alert configured in the project $\{Project\} is triggered.|
|AlertID|The unique ID of the alert.|0fdd88063a611aa114938f9371daeeb6-1671a52eb23|The ID of the alert is $\{AlertID\}.|
|AlertName|The name of the alert, which must be unique in a project.|alert-1542111415-153472|The alert $\{AlertName\} is triggered.|
|AlertDisplayName|The display name of the alert.|My alert|The alert $\{AlertDisplayName\} is triggered.|
|Condition|The conditional expression for triggering the alert. Each variable in the conditional expression is replaced with the value that triggers the alert. The value is enclosed in brackets \(\[\]\).|\[5\] \> 1|The conditional expression for triggering the alert is $\{Condition\}.|
|RawCondition|The original conditional expression for triggering the alert. Variables in the conditional expression are not replaced.|count \> 1|The original conditional expression for triggering the alert is $\{RawCondition\}.|
|Dashboard|The name of the dashboard with which the alert is associated.|mydashboard|The alert is associated with the dashboard $\{Dashboard\}.|
|DashboardUrl|The URL of the dashboard with which the alert is associated.|https://sls.console.aliyun.com/next/project/myproject/dashboard/mydashboard|The URL of the dashboard associated with the alert is $\{DashboardUrl\}.|
|FireTime|The time when the alert is triggered.|2018-01-02 15:04:05|The alert is triggered at $\{FireTime\}.|
|FullResultUrl|The URL used to query the trigger history of the alert.|https://sls.console.aliyun.com/next/project/my-project/logsearch/internal-alert-history?endTime=1544083998&queryString=AlertID%3A9155ea1ec10167985519fccede4d5fc7-1678293caad&queryTimeType=99&startTime=1544083968|Click $\{FullResultUrl\} to view details.|
|Results|The parameters and results of each log data query. The value is of the array type. For more information, see [Alert log fields](reseller.en-US/Alarm/Relevant syntax and fields for reference/Alarm log fields.md).| ``` {#codeblock_0md_nz4_rp7}
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

 |The first query starts at $\{Results\[0\].StartTime\} and ends at $\{Results\[0\].EndTime\}. The alert has been triggered $\{Results\[0\].FireResult.count\} times. **Note:** In this example, the digit 0 indicates the sequence number of the chart or the query and analysis statement that is queried.

[How can I check the sequence number of a chart?](reseller.en-US/Alarm/Relevant syntax and fields for reference/Set an alarm condition expression.md#searchNO)

 |

