# MNS logs {#concept_v3m_v3q_zdb .concept}

Message Service \(MNS\) log feature pushes your message operation logs to the specified  Logging Bucket. You can configure to push logs to Log Service in the console, and enable the log feature for a queue/topic in the region. MNS automatically pushes operation logs of the queue/topic messages to the specified Logging Bucket. Logging Bucket.

-   If you delete a Log Service project or Logstore corresponding to the Logging Bucket, or revoke the permission granted to MNS,  logs cannot be pushed to Log Service normally.
-   The log delay time is about five minutes.
-   A Logging Bucket is configured for each region. All the message operation logs of the queues/topics with the log feature enabled in the region are pushed to this Logging Bucket.
-   You can set whether to enable the log feature for each queue/topic separately. The function is disabled by default.

## Precautions {#section_gxn_sss_zdb .section}

-   If you delete a Log Service project or Logstore corresponding to the Logging Bucket, or revoke the permission granted to MNS,  logs cannot be pushed to Log Service normally.
-   The log delay time is about five minutes.
-   A Logging Bucket is configured for each region. All the message operation logs of the queues/topics with the log feature enabled in the region are pushed to this Logging Bucket.
-   You can set whether to enable the log feature for each queue/topic separately. The function is disabled by default.

## Prerequisites {#section_j5m_5ss_zdb .section}

1.  You have already activated Log Service and Message Service MNS.
2.  Message service logs can only be pushed to Log Service project in the corresponding region. Make sure that you have created a project and Logstore in the appropriate region.

## Procedure {#section_jks_wss_zdb .section}

1.  Enable the log feature for **Queue** and **Topic**.

    Take the log feature for **Queue** as an example.

    1.  In the MNS console, click **Queues** in the left-side navigation pane, and select a region.
    2.  Click **Modify Settings** at the right of **queue** for log collecting.
    3.  In the displayed **Modify Queue** dialog box, turn on the **Enable Logging** switch.
    **Note:** 

    -   This feature is disabled by default. Make sure this feature is enabled for all the queues where logs are to be collected.
    -   As the procedure for enabling the log feature for **Topic** is similar to one for **Queue**, follow the preceding steps to enable the log feature for **Topic**.
2.  enter the log query page.

    In the MNS console, click Logs in the left-side navigation pane to enter the **Log Management** page.

3.  Select the region

    At the top of the page, select the region for the Queue or Topic. Then, click **Configure** in the **Actions column**.

4.  Confirm authorization and Log Service project.
    -   If you configure MNS logs push for the first time, follow the prompts on the [Quick authorization](https://ram.console.aliyun.com/?spm=a2c4g.11186623.2.6.IlFP8O#/role/authorize?request=%7B%22Requests%22:%20%7B%22request1%22:%20%7B%22RoleName%22:%20%22AliyunMNSLoggingRole%22,%20%22TemplateId%22:%20%22Logging%22%7D%7D,%20%22ReturnUrl%22:%20%22https:%2F%2Fmns.console.aliyun.com%2F%3Fspm%3D5176.6660585.774526198.1.97zTJs%23%2Flogging%2Fcn-hangzhou%22,%20%22Service%22:%20%22MNS%22%7D) page.

    -   If you do not have a proper project and Logstore, go to the Log Service consoleLog Service console, and follow the page prompts to create a project and Logstore.  For more information, see [Preparation](reseller.en-US/User Guide/Basic operation.md).

5.  Push configuration.

    Click Configure at the right of a region. In the Push Logs to LogService tab, select the corresponding Project and Logstore name.

    **Note:** 

    -   Do not cancel authorization or delete the Resource Access Management \(RAM\) role. Otherwise, MNS logs cannot be pushed to Log Service normally.
    -   Make sure Logging Bucket and Log Service project are in the same region.
    -   After creating a project and Logstore, return to the configuration page of the MNS console, and click **Refresh** to view the new project and Logstore.
    Click **OK** after completing the configuration.


## Log format {#section_hhr_mts_zdb .section}

## Queue message operation logs {#section_uzz_mts_zdb .section}

Queue message operation logs are generated when queue messages are operated, such as sending a message, consuming a message, or deleting a message.

A message operation log contains multiple fields.  Fields contained in message operation logs vary with operations.

**Log field description**

The description of each field is shown in the following table.

|Field|Meaning |
|:----|:-------|
|Time|The operation occurrence time.|
|MessageId|The ID of the message processed in this operation.|
|QueueName|The name of the queue corresponding to this operation.|
|AccountId|The account of the queue corresponding to this operation.|
|Remoteaddress|The address of the client that initiates this operation.|
|NextVisibleTime|The next visible time of the message after this operation is complete.|
|ReceiptHandleInRequest|The ReceiptHandle parameter passed in when this operation is performed.|
|ReceiptHandleInResponse|The ReceiptHandle parameter returned when this operation is complete.|

**Field description for each operation**

Logs of different operations contain different fields. The following table lists fields contained in each operation.

|Operation |Time|QueueName|AccountId|MessageId|RemoteAddress|NextVisibleTime|ReceiptHandleInResponse|ReceiptHandleInRequest|
|:---------|:---|:--------|:--------|:--------|:------------|:--------------|:----------------------|:---------------------|
|SendMessage/BatchSendMessage|Yes|Yes|Yes|Yes|Yes|Yes|-|-|
|PeekMessage/BatchPeekMessage|Yes|Yes|Yes|Yes|Yes|-|-|-|
|ReceiveMessage/BatchReceiveMessage|Yes|Yes|Yes|Yes|Yes|Yes|Yes|-|
|ChangeMessageVisibility|Yes|Yes|Yes|Yes|Yes|Yes|Yes|Yes|
|DeleteMessage/BatchDeleteMessage|Yes|Yes|Yes|Yes|Yes|Yes|-|Yes|

## Topic message operation logs {#section_j11_nts_zdb .section}

Topic message operation logs are generated when topic messages are operated, including publishing and pushing a message.

The following sections describe the meaning of each field in the topic message operation logs, and fields contained in different operations.

**Log field description**

A message operation log contains multiple fields. The following table lists the description of each field.

|Field|Meaning|
|:----|:------|
|Time|The operation occurrence time.|
|MessageId|The ID of the message processed in this operation.|
|TopicName|The name of the topic corresponding to this operation.|
|SubscriptionName|The name of the subscription corresponding to this operation.|
|AccountId|The account of the topic corresponding to this operation.|
|RemoteAddress|The address of the client that initiates this operation.|
|NotifyStatus|The status code or corresponding error information returned by the user when MNS pushes a message to the user.|

**Field description for each operation**

Logs of different operations contain different fields. The following table lists fields contained in each operation.

|Operation|Time|MessageId|TopicName|SubscriptionName|AccountId|RemoteAddress|NotifyStatus|SubscriptionName|
|:--------|:---|:--------|:--------|:---------------|:--------|:------------|:-----------|:---------------|
|PublishMessage|Yes|Yes|Yes|-|Yes|Yes|-|-|
|Notify|Yes|Yes|Yes|Yes|Yes|-|Yes|Yes|

**NotifyStatus**

NotifyStatus is a field contained only in logs for pushing messages. This field helps you investigate the reason why MNS fails to push messages to the endpoint.

Depending on the different  NotifyStatus values, you can perform troubleshooting according to the suggestions provided in the following table.

|Error code|Description|Troubleshooting method|
|:---------|:----------|:---------------------|
|2xx|The message is pushed successfully. |-|
|Other HTTP status code |After a message is pushed to an endpoint, the endpoint returns a non-2xx status code. |Check the processing logic of the endpoint.|
|InvalidHost|The endpoint specified in the subscription is invalid. |Use curl or telnet to check whether the endpoint in the subscription is valid.|
|ConnectTimeout|The connection to the endpoint specified in the subscription times out. |Use curl or telnet to check whether the endpoint in the subscription is accessible.|
|ConnectFailure|The connection to the endpoint specified in the subscription fails. |Use curl or telnet to check whether the endpoint in the subscription is accessible.|
|UnknownError|Unknown error. |Contact MNS technical support.|

