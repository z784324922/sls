# Subscribe to dashboards {#concept_yhp_mmg_vgb .concept}

Log Service dashboards provide many visual components for you to customize your own dashboard based on log query and analysis results. Now, Log Service allows you to subscribe to dashboards. Log Service can periodically take snapshots of dashboards and send the snapshots to subscribers by email or DingTalk group message.

## Limits {#section_wqk_ws4_vgb .section}

-   You can create only one subscription task for a dashboard.
-   You can send up to 50 emails per day under an Alibaba Cloud account.
-   CronExpression can set the minimum interval in units of minutes. We recommend that you set the interval for sending notification messages to at least 1 hour.
-   The total number of subscription and alerting tasks for a project cannot exceed 100. You can open a ticket to increase the limit if you need more subscription and alerting tasks for a project.
-   If a table displays data by page, Log Service sends only the snapshot of the first page to subscribers.

**Note:** 

-   You can select any time range in display mode to view the chart data of different time ranges. The chart data is not affected.
-   To modify the default query time range of charts on a dashboard, you can enter the edit mode and click **Edit** in each chart.
-   The query time range of a subscribed dashboard is the time range of queried data in charts on the dashboard.

## Create a dashboard subscription task {#section_q5m_ct4_vgb .section}

1.  Log on to the [Log Service console](https://partners-intl.console.aliyun.com/#/sls), and then click the target project name.
2.  In the left-side navigation pane, click **Dashboard**.
3.  Click a dashboard name to view the dashboard.
4.  Click **Subscribe** in the upper-right corner of the dashboard page.
5.  Configure the subscription task and click **Next**.

    |Parameter|Description|Example|
    |:--------|:----------|:------|
    |**Subscription Name**|The name of the subscription task. The name must be 1 to 64 characters in length.|Dashboard subscription|
    |**Frequency**|The frequency for sending notification messages after the subscription to the dashboard. Valid values:

    -   Hourly
    -   Daily
    -   Weekly
    -   Fixed interval \(days\)
    -   Interval specified by CronExpression
CronExpression can set the minimum interval in units of minutes. We recommend that you set the interval for sending notification messages to at least 1 hour.

 |Use CronExpression to set the frequency to `* 0/1 * * *`, which specifies that messages are sent every hour starting from 00:00.|
    |**Add Watermark**|Specifies whether to add a watermark to the generated dashboard snapshots. The watermark content is the address used to receive notification messages, such as an email address or the access\_token in the WebHook address of DingTalk Chatbot.|N/A|

    ![Subscribe](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/125837/156574487438999_en-US.png)

6.  Configure the notification method.

    You can set the notification method to Email or WebHook-DingTalk Bot.

    -   Email

        Enter email addresses in the **Recipients** field and set **Subject**. The default email subject is `Log Service Report`.

        ![email](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/125837/156574487439000_en-US.png)

    -   WebHook-DingTalk Bot

        Enter the WebHook address of DingTalk Chatbot in the **Request URL** field. For more information about how to obtain this address, see [Customize DingTalk Chatbot](https://open-doc.dingtalk.com/docs/doc.htm?spm=a219a.7629140.0.0.karFPe&treeId=257&articleId=105735&docType=1).

        ![DingTalk](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/125837/156574487439002_en-US.png)


## Modify and cancel a subscription task {#section_yvf_dt4_vgb .section}

If a subscription task is created for a dashboard, you can click **Subscribe** in the upper-right corner of the dashboard page to modify or cancel the subscription task.

After the subscription task is canceled, Log Service no longer sends subscription messages to subscribers.

![cancel ](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/125837/156574487439004_en-US.png)

