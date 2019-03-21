# Subscribe to dashboard snapshots {#concept_yhp_mmg_vgb .concept}

This topic describes how to subscribe to dashboard snapshots in Log Service. Specifically, you can set for the system to send notifications about dashboard snapshots to specific email addresses or send notifications as DingTalk chatbot messages.

## Limits {#section_wqk_ws4_vgb .section}

-   You can create a maximum of one subscription for each dashboard.
-   In each day, Log Service can send up to fifty emails for each account.
-   The interval specified by a Cron expression must be at least one minute. However, we recommend that you set an interval of one hour or longer if you choose to use a Cron expression.
-   The maximum number of subscriptions and alert notifications that you can create for a Log Service project is 100. If you need to increase one or both of these quotas, open a ticket.

## Create a subscription {#section_q5m_ct4_vgb .section}

1.  Log on to the [Log Service console](https://partners-intl.console.aliyun.com/#/sls) and click the project name.
2.  In the left-side navigation pane, click **Dashboard**.
3.  Click the target dashboard.
4.  In the upper-right corner of the page, click **Subscribe**.
5.  Set a subscription, and then click **Next**.

    |Configuration|Description|Example|
    |:------------|:----------|:------|
    |**Subscription Name**|A subscription name must be 1 to 64 characters in length.|Dashboard subscription|
    |**Frequency**|The frequency at which dashboard snapshot notifications are sent.Available values are:

    -   **Hourly**
    -   **Daily**
    -   **Weekly**
    -   **Fixed Interval**
    -   **Cron**
**Cron** indicates a Cron expression that can be used to specify an interval in minutes at least. However, we recommend that you set an interval longer than one hour if you use a Cron expression to set this parameter.

|The `* 0/1 * * *` Cron expression indicates that dashboard snapshot notifications are sent at one-hour intervals starting from 00:00.|
    |**Add Watermark**|Add watermarks to dashboard snapshots. The watermark is the email address or the access\_token of the WebHook.|-|

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/125837/155315324138999_en-US.png)

6.  Set notifications.

    Dashboard snapshot notifications can be sent to a specific email address or sent as a DingTalk chatbot message.

    -   Email

        In the **Recipients** box, enter an email address. You can also set the email **Subject**. If you do not set any email subject, Log Service uses `Log Service Report` as the default subject.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/125837/155315324139000_en-US.png)

    -   DingTalk chatbot

        In the **Request URL** box, enter the DingTalk chatbot address. For more information, see [Set a DingTalk robot](https://open-doc.dingtalk.com/docs/doc.htm?spm=a219a.7629140.0.0.karFPe&treeId=257&articleId=105735&docType=1).

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/125837/155315324139002_en-US.png)


## Modify or cancel a subscription {#section_yvf_dt4_vgb .section}

In the upper-right corner, choose **Subscribe** \> **Modify** or choose **Subscribe** \> **Cancel** according to your requirements.

Note that after you cancel a subscription, Log Service immediately stops sending dashboard snapshot notifications.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/125837/155315324139004_en-US.png)

