# Query MNS logs {#concept_35374_zh .concept}

Alibaba Cloud [Message Service \(MNS\)](https://www.aliyun.com/product/mns) pushes logs to Log Service. This topic describes how to query specified information in logs pushed from MNS.

MNS supports queue logs and topic logs. These logs contain all information about messages throughout the lifecycle, including the time, location, operation, and context. You can analyze the logs through real-time query, real-time computing, and offline computing.

## Real-time query {#section_zz2_x1q_5fb .section}

This section describes several common scenarios of real-time query. You can use multiple keywords to run complex queries.

## View the message tracing of a message in a queue {#section_xq7_fxb_hh3 .section}

-   **Procedure** 
    1.  Enter the queue name and the ID of the message in the search box. Format: `$queuename and $messageid`.
    2.  Select the time range of which you want to query logs, and click **Search & Analysis**. Then, you can view the operation logs of the message.
-   **Example** 

    View the message tracing of the message whose ID is `12682720A1B271D0-1-1635DD12B1C-200000004` in the queue named loglog.

    -   Statement: `loglog and 12682720A1B271D0-1-1635DD12B1C-200000004`

    -   Result: As shown in the following figure, the query result displays the process from message sending to message deleting.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13207/156894828132448_en-US.jpg)


## View the number of the messages that are sent to Log Service in a queue {#section_h3a_19r_fio .section}

-   **Procedure** 
    1.  Enter the queue name and the write operation in the search box. Format: `$queuename and (SendMessage or BatchSendMessage)`.
    2.  Select the time range of which you want to query logs, and click **Search & Analysis**. Then, you can view the logs about all messages sent to Log Service in the queue. Move the pointer over the green column chart. You can view the number of the messages sent in the time period you specified.
-   **Example** 

    View the number of the messages that are sent to Log Service in the queue named MyQueue.

    -   Statement: `MyQueue and (SendMessage or BatchSendMessage)`

    -   Result: As shown in the following figure, four write operations are performed in the time period you specified.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13207/156894828132449_en-US.jpg)


## View the number of the messages that are consumed by Log Service in a queue {#section_qfu_bne_t53 .section}

-   **Procedure** 
    1.  Enter the queue name and the consumption operation in the search box. Format: `$queuename and (ReceiveMessage or BatchReceiveMessage)`.
    2.  Select the time range of which you want to query logs, and click **Search & Analysis**. Then, you can view the logs about the messages consumed by Log Service in the queue.
-   **Example** 

    View the number of the messages that are consumed by Log Service in the queue named loglog.

    -   Statement: `loglog and (ReceiveMessage or BatchReceiveMessage)`

    -   Result: As shown in the following figure, five consumption operations are performed in the time period you specified.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13207/156894828232450_en-US.jpg)


## View the number of the messages that are deleted in a queue {#section_frz_kjh_dkt .section}

-   **Procedure** 
    1.  Enter the queue name and the deletion operation in the search box. Format: `$queuename and (DeleteMessage or BatchDeleteMessage)`.
    2.  Select the time range of which you want to query logs, and click **Search & Analysis**. Then, you can view the logs about the messages that are deleted in the queue.
-   **Example** 

    View the number of the messages that are deleted in the queue named loglog.

    -   Statement: `loglog and (DeleteMessage or BatchDeleteMessage)`

    -   Result: As shown in the following figure, the query result displays the logs about all messages that are deleted. You can view the number of the messages that are deleted.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13207/156894828232451_en-US.jpg)


## View the message tracing of a message in a topic {#section_oq2_53p_88w .section}

-   **Procedure** 
    1.  Enter the topic name and the ID of the message in the search box. Format: `$topicname and $messageid`.
    2.  Select the time range of which you want to query logs, and click **Search & Analysis**. Then, you can view the message tracing of the message in the topic.
-   **Example** 

    View the message tracing of the message whose ID is `BD692F55DED88AF6-1-1635DFEAF3B-200000008` in the topic named logtesttt.

    -   Statement: `logtesttt and BD692F55DED88AF6-1-1635DFEAF3B-200000008`

    -   Result: As shown in the following figure, the query result displays the process from message sending to notification in the logtesttt topic.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13207/156894828232452_en-US.jpg)


## View the number of the messages that are published in a topic {#section_ptp_9eh_o8d .section}

-   **Procedure** 
    1.  Enter the topic name and the publish operation in the search box. Format: `$topicname and PublishMessage`.
    2.  Select the time range of which you want to query logs, and click **Search & Analysis**. Then, you can view the logs about the messages that are published in the topic.
-   **Example**: View the number of the messages that are published in the topic named logtesttt.

-   Statement: `logtesttt and PublishMessage`

-   Result: As shown in the following figure, five publish operations are performed in the time period you specified.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13207/156894828232453_en-US.jpg)


## View the number of the messages processed by a client {#section_73g_s4b_c1z .section}

-   **Procedure** 
    1.  Enter the IP address of the client in the search box. Format:`$ClientIP`. If you want to query certain types of operation logs, specify the operations in the search box. Example: `$ClientIP and (SendMessage or BatchSendMessage)`.

    2.  Select the time range of which you want to query logs, and click Search & Analysis. Then, you can view all the operation logs of the client.

-   **Example**:

    View the number of the messages processed by the client whose IP address is 10.10.XX.XX.

    -   Statement: `10.10.XX.XX`

    -   Result: As shown in the following figure, three message processing operations are performed in the time period you specified.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13207/156894828232454_en-US.jpg)


## Real-time computing and offline computing {#section_7rm_rca_bqx .section}

-   Real-time computing: analyzes the MNS logs in real time through Spark, Storm, Realtime Compute, or Consumer Library. The following are some use cases:
    -   What are the IP addresses of the top 10 clients that send the most messages or consume the most messages in a queue?
    -   Is the balance kept between the speed of message production and consumption? Do any consumers have difficulties in processing latency?
-   Offline computing: analyzes the logs within a large time span through MaxCompute, E-MapReduce, or Hive.
    -   What is the average latency from message publish to message consumption over the last week?
    -   How is the performance changed after the upgrade?

