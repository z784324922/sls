# MNS日志 {#concept_v3m_v3q_zdb .concept}

MNS 的日志管理功能将用户的消息操作日志推送到指定 LoggingBucket 中。用户在控制台上配置将日志推送到日志服务，然后开启该地域队列/主题的日志管理功能，MNS 将自动推送该队列/主题消息的操作日志到指定的 LoggingBucket 中。

-   如果用户将 LoggingBucket 对应的LogService的Project、Logstore删除，或者将授予 MNS 的权限取消，日志将无法正常推送到日志服务。
-   日志延迟时间约5分钟。
-   每个地域配置一个 LoggingBucket，该地域所有开通日志管理功能的队列/主题的消息操作日志均推送到该 LoggingBucket中。
-   每个队列/主题可以独立设置是否开启日志管理功能，默认不开启。

## 注意事项 {#section_gxn_sss_zdb .section}

-   如果用户将 LoggingBucket 对应的LogService的Project、Logstore删除，或者将授予 MNS 的权限取消，日志将无法正常推送到日志服务。
-   日志延迟时间约5分钟。
-   每个地域配置一个 LoggingBucket，该地域所有开通日志管理功能的队列/主题的消息操作日志均推送到该 LoggingBucket中。
-   每个队列/主题可以独立设置是否开启日志管理功能，默认不开启。

## 前提条件 {#section_j5m_5ss_zdb .section}

1.  您已开通日志服务LogService和消息服务MNS。
2.  您的消息服务日志仅能推送到对应Region下的日志服务Project，请确认您已创建了对应Region的Project和Logstore。

## 操作步骤 {#section_jks_wss_zdb .section}

1.  开启**队列**和**主题**的日志功能。

    此处以开启**队列**的日志功能为例。

    1.  在消息服务控制台左侧单击**队列**，并选择地域。
    2.  选择需要采集日志的队列，单击**操作**栏中的**修改设置**。
    3.  在**修改队列**对话框中，打开**开启logging**开关。
    **说明：** 

    -   该功能默认关闭，请确保所有需要采集日志的队列已开启该功能。
    -   **主题**的日志功能开启步骤类似，请参考**队列**的操作步骤，打开**主题**的日志功能。
    ![](images/5411_zh-CN.png "开启logging")

2.  进入日志管理页面。

    在消息服务控制台单击左侧 **日志管理**，进入日志管理页面。

3.  选择Region。

    在页面上方选择需要推送日志的队列或主题所在的Region，并单击**操作**列的**配置**。

    ![](images/5412_zh-CN.png "选择Region")

4.  确认授权与日志服务Project。
    -   如您是首次操作MNS日志推送，请按照页面提示进行[快捷授权](https://ram.console.aliyun.com/?spm=a2c4g.11186623.2.6.IlFP8O#/role/authorize?request=%7B%22Requests%22:%20%7B%22request1%22:%20%7B%22RoleName%22:%20%22AliyunMNSLoggingRole%22,%20%22TemplateId%22:%20%22Logging%22%7D%7D,%20%22ReturnUrl%22:%20%22https:%2F%2Fmns.console.aliyun.com%2F%3Fspm%3D5176.6660585.774526198.1.97zTJs%23%2Flogging%2Fcn-hangzhou%22,%20%22Service%22:%20%22MNS%22%7D)。

    -   如您没有合适的Project和Logstore，请按照页面提示前往日志服务控制台新建一个Project和Logstore。具体步骤请参考[准备流程](intl.zh-CN/准备工作/准备流程.md#)。

5.  配置推送。

    在推送日志到LogService页签中选择对应Region的日志服务Project和Logstore，并单击确认。

    **说明：** 

    -   请勿取消授权或删除RAM角色，否则会造成MNS日志无法正常推送到日志服务。
    -   请保证LoggingBucket和日志服务Project地域一致。
    -   创建Project和Logstore后，返回MNS控制台配置页面，单击**刷新**即可看到新建的Project和Logstore。
    配置完成后，单击**确认**。

    ![](images/5413_zh-CN.png "配置推送")


## 日志格式 {#section_hhr_mts_zdb .section}

## 队列消息操作日志 {#section_uzz_mts_zdb .section}

队列消息操作日志是指操作队列消息所产生的日志，比如发送消息、消费消息、删除消息等操作。

一条消息操作日志中包含多个字段，每个字段都有自己的含义。根据操作的不同，消息操作日志所包含的字段也不相同。

**日志字段说明**

一条消息操作日志中包含多个字段，各个字段的含义如下表所示。

|字段|含义|
|:-|:-|
|Time|本次操作的发生时间。|
|MessageId|消息的 MessageId，标识本次操作处理的消息。|
|QueueName|本次操作对应的队列名称。|
|AccountId|本次操作对应队列的账号。|
|RemoteAddress|发起该操作的客户端地址。|
|NextVisibleTime|该操作执行完成后，这条消息的下次可见时间。|
|ReceiptHandleInRequest|用户执行该操作时传入的 ReceiptHandle 参数。|
|ReceiptHandleInResponse|该操作执行完成后，返回给用户的 ReceiptHandle。|

**各个操作的字段说明**

不同操作的日志包含的字段信息各不相同，具体每个操作包含的字段请参考表格。

|操作|Time|QueueName|AccountId|MessageId|RemoteAddress|NextVisibleTime|ReceiptHandleInResponse|ReceiptHandleInRequest|
|:-|:---|:--------|:--------|:--------|:------------|:--------------|:----------------------|:---------------------|
|SendMessage/BatchSendMessage|有|有|有|有|有|有|-|-|
|PeekMessage/BatchPeekMessage|有|有|有|有|有|-|-|-|
|ReceiveMessage/BatchReceiveMessage|有|有|有|有|有|有|有|-|
|ChangeMessageVisibility|有|有|有|有|有|有|有|有|
|DeleteMessage/BatchDeleteMessage|有|有|有|有|有|有|-|有|

## 主题消息操作日志 {#section_j11_nts_zdb .section}

主题消息操作日志是指操作主题消息产生的日志，主要有两类：发布消息和推送消息。

主题消息操作日志各个字段的含义，以及不同的操作所包含的字段信息如下。

**日志字段说明**

一条消息操作日志中包含多个字段，各个字段的含义如表格所示。

|字段|含义|
|:-|:-|
|Time|本次操作的发生时间。|
|MessageId|消息的 MessageId，标识本次操作处理的消息。|
|TopicName|本次操作对应的主题名称。|
|SubscriptionName|本次操作对应的订阅名称。|
|AccountId|本次操作对应主题的账号。|
|RemoteAddress|发起该操作的客户端地址。|
|NotifyStatus|MNS 将消息推送给用户时，用户返回的状态码或者相应的错误信息。|

**各个操作的字段说明**

不同操作的日志包含的字段信息各不相同，具体每个操作包含的字段请参考下表。

|操作|Time|MessageId|TopicName|SubscriptionName|AccountId|RemoteAddress|NotifyStatus|SubscriptionName|
|:-|:---|:--------|:--------|:---------------|:--------|:------------|:-----------|:---------------|
|PublishMessage|有|有|有|-|有|有|-|-|
|Notify|有|有|有|有|有|-|有|有|

**NotifyStatus字段**

NotifyStatus是推送消息日志特有的字段，可以协助您调查MNS推送消息到Endpoint失败的原因。

根据不同的 NotifyStatus，您可以按照下表建议的处理方法进行处理。

|错误码|描述|建议处理方法|
|:--|:-|:-----|
|2xx|消息推送成功。|-|
|其它Http状态码|消息推送给用户，Endpoint 返回了非2xx的状态码。|检查 Endpoint 端处理逻辑。|
|InvalidHost|订阅指定的 Endpoint 不合法。|确认订阅中 Endpiont 是否真实有效，可使用curl/telnet进行确认。|
|ConnectTimeout|连接订阅指定的 Endpoint 超时。|确认订阅中 Endpoint 当前是否可访问，可使用curl/telnet进行确认。|
|ConnectFailure|连接订阅指定的 Endpoint 失败 。|确认订阅中 Endpoint 当前是否可访问，可使用curl/telnet进行确认。|
|UnknownError|未知错误。|请工单联系 MNS 技术人员。|

