# Set alarms {#concept_uzy_snq_zdb .concept}

## Workflow  {#section_cm3_v3k_5cb .section}

-   [Step 1. Configure a saved search ](#section_byf_w3k_5cb)
-   [Step 2. Create an alarm rule](#section_o15_rlk_5cb)
-   [Step 3. Configure alarm action](#section_d35_1mk_5cb)
-   [Step 4. View alarm records](#section_kdv_jmk_5cb)

## Step 1. Configure a saved search  {#section_byf_w3k_5cb .section}

**Mode of returning results **

Log query results can be displayed in the following two modes: **direct return of results** and **result statistics**.  In the **direct return of results** mode, the number of logs that meet the query condition can be directly returned. In the **result statistics** mode, the distribution of the number of logs that meet the query condition for a specified time range is returned.

-   Direct return of results 

    For example, if you query the data containing error in the recent 15 minutes, the condition is error and a total of 154 records are found. The distribution is as follows:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13160/5772_en-US.png "Raw log ")

    The content of each record is a combination of key and value. You can set an alarm condition for the value in a specific key.

    **Note:** If the number of query results exceeds  10 in a single query, only the first 10 results are judged by the alarm rules. An alarm is triggered when any of the 10 results meets the condition. 

-   Result statistics \(histogram query\)

    For example, query the ratio of logs with the status code 200 to all the logs. The query statement is as follows \(detailed query syntax[Query syntax](intl.en-US/User Guide/Index and query/Query syntax.md)\):

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13160/5773_en-US.png "Query Result Statistics ")

    ```
    * | select sum(case when status=200 then 1 else 0 end) *1.0/count(1) as ratio 
    ```

    Therefore, you can set an alarm condition as `ratio < 0.9` , indicating to give an alarm notification when the ratio of logs with the  status code 200 to all the logs is less than 90%. 


**Procedure **

1.  Log on to the Log Service console.
2.  On the Project List page, click the project name. 
3.  On the **Logstore List** page, click   **Search** at the right of the Logstore.
4.  Specify the Logstore, topic, and query statement as needed. Then, query the specified logs. 
5.  Click Saved to Saved search in the upper-right corner **to save the query parameters ** \> **as a saved search** .  On the Saved Search Details page, 
6.   and then click **OK**.

    -   **Operation:** **Select Create Saved Search**. 
    -   **Saved Search Name**: The name of the saved search. 
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13160/5775_en-US.png "Saved search details")


## Step 2. Create an alarm rule {#section_o15_rlk_5cb .section}

You can create an alarm rule after saving the query parameters as a saved search.

1.  Click **Saved to alarm** in the upper-right corner. The Alarm Rule page appears.
2.  Configure the alarm rule and then click **OK**. 

    Currently, alarm notifications are sent by using in-site notifications or WebHook.  

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13160/5776_en-US.png "Alarm rules")


**Rule description**

-   **Saved Search Name**: Select a created saved search. 
-   **Time Range \(minute\)**: The data time range \(in minutes\) to be read when the server performs the alarm check. For example, if the value is one, data from the last one minute to the current time of performing an alarm check is queried. 

    **Note:** Currently, the server only processes the first 10 data records in the time range as a sample when performing an alarm check. 

-   **Check Interval \(min\)**: The time interval \(in minutes\) for the server to perform an alarm check. Currently, the minimum interval is five minutes. 
-   **Number of Triggers**: The number of times to trigger the alarm checks consecutively. For example, if the check interval is five minutes, then here two indicates an alarm notification is sent when two consecutive checks meet the alarm conditions \(the minimum interval of an alarm is 10  minutes\). 
-   **Key**: The key used for alarms in the log contents. 
-   **Operator**: Supports numeric class \(Greater Than/Greater Than or Equal to/Less Than/Less Than or Equal to\) and character class \(Include and RegEx\) as follows. 

    |Operation|Description |Example |
    |:--------|:-----------|:-------|
    |\>|Whether the column value is greater than a value.|$ count \> 0|
    |< |Whether the column value is less than a value. |$count<200 |
    |\>=|Greater than or equal to a value. |$count\>=0 |
    |<=|Less than or equal to a value. |$count<=0|
    |like |A matched substring. |$project like “admin”|
    |regex |A string that matches with the regular expression.|$project regex match “^/S+$” |


## Step 3. Configure alarm action {#section_d35_1mk_5cb .section}

Currently, Log Service supports the following notification methods: 

-   [In-site notifications \(recommended\)](#section_hrf_3zz_zdb)
-   [WebHook-DingTalk Bot](#section_l15_111_12b)
-   [WebHook-Custom ](#section_flh_db1_12b)

When your configured alarm rule is triggered, Log Service sends you an alarm notification by using the specified notification method. 

**Note:** Currently, the logging service alarm SMS Notification method is about to be applied, and the message service \(MNS\) method is no longer supported to send alarm alerts.

1.  In the Log Service alarm settings, select **Webhook-custom** to add webhook links to webhook addresses, enter the notification content \(up to 50 characters, only English letters are supported\). 
2.  The post sends the following to the webhook address, an example of the content is as follows:

    ```
    {"uid": "13415134513","project":"ali-cn","trigger":"oplog_alert","condition":"3413 > 3000", "message":"PV count down 30%", "context':"c:3413"} 
    ```


## In-site notifications \(recommended\) {#section_hrf_3zz_zdb .section}

1.  In the **Action section ** \> **of the Alarm Rule page, **select **Notifications** from the **Action Type** drop-down list and  then configure the **notification content**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13160/5777_en-US.png "Action")

2.  Go to the Common Message Settings page in the Alibaba Cloud Message Center. Click **Modify** **at the right of Log Service Alarm Notification.** \> **Log Service ****Alarm Notification**. The **Modify Contact** dialog box appears. Select the receiver. ti 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13160/5778_en-US.png "Modify the Modify Contact and select the receiver ")

3.  in the **Modify Contact** dialog box.

    To add a receiver, click **Add Receiver** in the lower-left corner and then configure the name, email,  and occupation for the contact to receive the alarm notification.

    **Note:** 

    -   The system automatically sends the verification information to the entered email address. The contact can receive the alarm notification after the verification.
    -   At least one receiver is needed. 
    -   The default notification method is by email and cannot be modified.
    -   At most **50 alarm notifications are sent to each email one day**.

## DingTalk Bot {#section_l15_111_12b .section}

1.  Add a robot in a DingTalk group. Select Customize to access custom services by using WebHook.
2.  Enter a name for the robot \(optional\) and copy the WebHook link.
3.  In the Action section of the Alarm Rule page, select **WebHook-DingTalk Bot** from the Action Type drop-down list and then enter the WebHook link in the WebHook URL field. Enter the notification content in the Content field .

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13160/5783_en-US.png "Notification content ")


## Custom WebHook  {#section_flh_db1_12b .section}

1.  In the Action section of the Alarm Rule page, select **WebHook-Custom** from the Action Type drop-down list and then enter the WebHook link in the WebHook URL field. Enter the notification content in the Content field \(up to 50 characters, only English letters are supported\).

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13160/5785_en-US.png "Notification Type ")

2.  The following contents is sent to the WebHook URL in the Post mode after an alarm is triggered.

    Sample of the sent contents:

    ```
    {"uid": 
                "13415134513","project":"ali-cn","trigger":"oplog_alert","condition":"3413 >
                3000", "message":"PV count down 30%",
          "context":"c:3413"}
    ```


## Step 4. View alarm records {#section_kdv_jmk_5cb .section}

You can view the specific alarm records after creating alarm rules.

1.  Log on to the Log Service console.
2.  On the Project List page, click the project name.
3.  On the **Logstore List** page, click **LogSearch/Analytics \> ** \> **Alarm in the left-side navigation pane**.
4.  Click **View** at the right of the alarm rule to view the specific alarm records.

**Alarn status** :

-   **Success**: The rule is successfully executed and the standard to trigger the alarm is displayed in the Trigger Details.
-   **Failure**: Failed in the phase of query, alarm rule, or notification. View the Trigger Details for more information. 
    -   Failed to query the logs, which is generally caused by incorrect syntax. 
    -   Failed to call the query statement. Open a ticket. 
    -   Failed to call the rule. Check whether the format of rule parameters and that of returned data are consistent.

