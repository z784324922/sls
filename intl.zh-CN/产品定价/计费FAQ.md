# 计费FAQ {#concept_cwh_kvn_vdb .concept}

**问题列表**：

1.  [因日志服务造成账号欠费，应如何处理？](#section_ybw_lvn_vdb)
2.  [只创建了Project和Logstore，为什么会有账单？](#section_zbw_lvn_vdb)
3.  [如何关闭日志服务？](#section_acw_lvn_vdb)

## 1. 因日志服务造成账号欠费，应如何处理？ {#section_ghh_svn_vdb .section}

日志服务为使用后计费的模式，每日为您输出账单并自动扣费，账单内容为您昨日的资源使用项目。如您欠费时间超过24小时，则您的服务将自动停止，而您所占用的存储空间的这部分资源仍会继续扣费， 因此欠费余额会累计。建议您在欠费后24H内补足欠费，以免服务停止，对您的业务造成损失。在您补足金额后，可以继续操作日志服务。

## 2. 只创建了Project和Logstore，为什么会有账单？ {#section_adq_svn_vdb .section}

您如果创建过Project和Logstore，则会默认创建Shard预留资源。正如创建Logstore时的页面提示，日志服务对Shard收取少量的资源预留费用。根据当前的计费策略，Shard的免费额度为31天\*个，如您创建了2个Shard，则15天后开始计费。如果您不再需要该Shard，您可以直接删除Project和Logstore。删除当日的资源使用费用将会在次日为您发送账单，隔日后将不再有该Project的账单。

**详细计费项目请参考[计费方式](intl.zh-CN/产品定价/计费方式.md)。**

## 3. 如何关闭日志服务？ {#section_acw_lvn_vdb .section}

如果您不再需要使用日志服务，您可以直接删除账号下所有Project，相当于关闭了日志服务，次日开始不再计费。如果您的账号处于欠费状态，请补足欠费后再删除Project。如您账号下已没有任何日志服务业务和资源，隔日后您不会收到日志服务的账单。

