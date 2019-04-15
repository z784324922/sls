# Manage an alarm {#concept_emk_lvd_yfb .concept}

This topic describes how to manage an alarm on the Alarm page. Specifically, this topic describes how to view overview information of an alarm, how to disable or enable an alarm, disable and recover alarm notifications, and how to delete an alarm.

## View overview information of an alarm {#section_e4d_1zd_yfb .section}

1.  Log on to the [Log Service console](https://partners-intl.console.aliyun.com/#/sls) and click the project name.
2.  In the left-side navigation pane of the Logstores page, click **Alarm**.

    The Logstores page displays information relating to the alarms you created such as the name of the corresponding dashboard where an alarm is attached, the date at which each alarm was created and updated, and whether the notification of each alarm is enabled.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/65187/155532277733234_en-US.png)


## Disable or enable an alarm {#section_wyd_svd_yfb .section}

After you create an alarm, you can disable or enable the alarm at any time. If you disabled an alarm, then the system does not perform required analyses at specified intervals or send alarm notifications.

1.  Log on to the [Log Service console](https://partners-intl.console.aliyun.com/#/sls) and click the project name.
2.  In the left-side navigation pane of the Logstores page, click **Alarm**.
3.  On the right of the target alarm, turn on the **Enable** switch.

    The switch indicates the status of an alarm.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/65187/155532277733235_en-US.png)


## Disable and recover alarm notifications {#section_vl3_mwd_yfb .section}

After you disable alarm notifications for an enabled alarm, the system still performs the required analyses at specified intervals. However, the system does not send any notifications when the condition for triggering an alarm is met during the period in which you have disabled alarm notifications.

1.  Log on to the [Log Service console](https://partners-intl.console.aliyun.com/#/sls) and click the project name.
2.  In the left-side navigation pane of the Logstores page, click **Alarm**.
3.  On the right of the target alarm, click **Disable Notifications**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/65187/155532277733236_en-US.png)

4.  Set the time length in which the alarm notifications remains disabled, and then click **Confirm**.

    After you disable alarm notifications for an alarm, you can view the data at which alarm notifications will be recovered in the **Notification Status** column.

    **Note:** The disabled state of alarm notifications can last up to 30 days.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/65187/155532277733237_en-US.png)

5.  Optional. To enable alarm notifications for the alarm before the time that alarm notifications are recovered for the alarm, click **Enable Notifications**.

## Delete an alarm {#section_y3k_txd_yfb .section}

**Note:** A deleted alarm cannot be recovered.

1.  Log on to the [Log Service console](https://partners-intl.console.aliyun.com/#/sls) and click the project name.
2.  In the left-side navigation pane of the Logstores page, click **Alarm**.
3.  On the right of the target alarm, click **Delete**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/65187/155532277733239_en-US.png)

4.  In the displayed dialog box, click **Confirm**.

## What to do next {#section_rzb_1xp_3hb .section}

You can also manage an alarm by viewing its records and modify its configurations. For more information, see [View alarm records](reseller.en-US/User Guide/Alarm/Modify and view an alarm/View and use alarm logs.md#) and [Modify an alarm configuration](reseller.en-US/User Guide/Alarm/Modify and view an alarm/Modify an alarm configuration.md#).

