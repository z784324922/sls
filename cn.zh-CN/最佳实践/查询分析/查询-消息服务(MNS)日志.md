# 查询-消息服务\(MNS\)日志 {#concept_35374_zh .concept}

阿里云[消息服务\(MNS\)](https://www.aliyun.com/product/mns)支持将日志推送到日志服务，本文为您介绍日志成功推送后，如何通过日志查询特定信息。

消息服务支持队列消息操作日志以及主题消息操作日志，其中日志包含了消息生命周期的所有内容，时间、地点、操作和上下文等。您可以通过实时查询、实时计算和离线计算三种方法对日志进行分析。

## 实时查询 {#section_zz2_x1q_5fb .section}

本文档仅介绍几种常用场景的查询，用户可以通过组合多个关键字来实现更加复杂的查询。

## 查看队列消息的消息轨迹 {#section_xq7_fxb_hh3 .section}

-   **步骤** 
    1.  在搜索框中输入队列名称和MessageId。格式为`$queuename and $messageid`。
    2.  选择合适的时间范围后，单击**查询/分析**即可查看该消息的详细操作日志。
-   **示例** 

    查看loglog队列中MessageId为`12682720A1B271D0-1-1635DD12B1C-200000004`的消息轨迹。

    -   查询语句：`loglog and 12682720A1B271D0-1-1635DD12B1C-200000004`

    -   查询结果：如下图所示，查询结果中展示了该消息从发送到删除的过程。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13207/156136220532448_zh-CN.jpg)


## 查看队列消息写入量 {#section_h3a_19r_fio .section}

-   **步骤** 
    1.  在搜索框中输入队列名称和写入操作。格式为`$queuename and (SendMessage or BatchSendMessage)`。
    2.  选择合适的时间范围后，单击**查询/分析**即可查看该队列的所有写入消息。鼠标移至绿色柱状图上，可以查看当前时间段内的具体消息数量。
-   **示例** 

    查看MyQueue队列中的消息写入量。

    -   查询语句：`MyQueue and (SendMessage or BatchSendMessage)`

    -   查询结果：如下图所示，当前查询时段内，有4条写入操作。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13207/156136220532449_zh-CN.jpg)


## 查看队列消息消费量 {#section_qfu_bne_t53 .section}

-   **步骤** 
    1.  在搜索框中输入队列名称和消费操作。格式为`$queuename and (ReceiveMessage or BatchReceiveMessage)`。
    2.  选择合适的时间范围后，单击**查询/分析**即可查看该队列消息消费量。
-   **示例** 

    查看loglog队列中的消息消费量。

    -   查询语句：`loglog and (ReceiveMessage or BatchReceiveMessage)`

    -   查询结果：如下图所示，当前查询时段内，有5条消费记录。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13207/156136220532450_zh-CN.jpg)


## 查看队列消息删除量 {#section_frz_kjh_dkt .section}

-   **步骤** 
    1.  在搜索框中输入队列名称和删除操作。格式为：`$queuename and (DeleteMessage or BatchDeleteMessage)`。
    2.  选择合适的时间范围后，单击**查询/分析**即可查看该队列消息删除量。
-   **示例** 

    查看名为loglog的队列消息删除量。

    -   查询语句：`loglog and (DeleteMessage or BatchDeleteMessage)`

    -   查询结果：如下图所示，搜索结果中展示了loglog队列消息的删除日志，您可以查看删除量。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13207/156136220632451_zh-CN.jpg)


## 查看主题消息的消息轨迹 {#section_oq2_53p_88w .section}

-   **步骤** 
    1.  在搜索框中输入主题名称和messageid。格式为`$topicname and $messageid`。
    2.  选择合适的时间范围后，单击**查询/分析**即可查看该主题消息的消息轨迹。
-   **示例** 

    查看名为logtesttt主题中MessageId为`BD692F55DED88AF6-1-1635DFEAF3B-200000008`的消息轨迹。

    -   查询语句：`logtesttt and BD692F55DED88AF6-1-1635DFEAF3B-200000008`

    -   查询结果：如下图所示，搜索结果中展示了logtesttt主题中消息从发送到通知的过程。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13207/156136220632452_zh-CN.jpg)


## 查看主题消息发布量 {#section_ptp_9eh_o8d .section}

-   **步骤** 
    1.  在搜索框中输入主题名称和发布操作。格式：`$topicname and PublishMessage`。
    2.  选择合适的时间范围后，单击**查询/分析**即可查看该主题消息发布量。
-   **示例**：查询名为logtesttt的主题消息发布量。

-   查询语句：`logtesttt and PublishMessage`

-   查询结果：如下图所示，当前查询时段内，有5条消息发布记录。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13207/156136220632453_zh-CN.jpg)


## 查看某个客户端消息处理量 {#section_73g_s4b_c1z .section}

-   **步骤** 
    1.  索框中输入客户端IP。格式：`$ClientIP`，如果希望查询某个客户端的某类操作日志，搜索框中增加具体操作即可，例如：`$ClientIP and (SendMessage or BatchSendMessage)`。

    2.  选择合适的时间范围后，单击搜索按钮即可查看该客户端所有的消息操作日志。

-   **示例**：

    查询IP为10.10.XX.XX的客户端消息处理量。

    -   查询语句：`10.10.XX.XX`，

    -   查询结果：如下图所示，当前查询时段内，有3条消息处理记录。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13207/156136220632454_zh-CN.jpg)


## 实时计算 & 离线计算 {#section_7rm_rca_bqx .section}

-   实时计算：使用Spark、Storm或StreamCompute，Consumer Library等方式可以实时对消息服务日志进行分析。例如：
    -   对一个队列而言，Top 10 消息的产生者、消费者分别是谁哪些IP？
    -   生产和消费的速度是否均衡？某些消费者在处理延时上是否有瓶颈？
-   离线：使用MaxCompute 或 E-MapReduce/Hive进行大时间跨度的计算。
    -   最近一周内，消息从发布到被消费平均延迟是什么？
    -   对比升级前和升级后两个时间段内性能变化如何？

