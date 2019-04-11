# Overview {#reference_ud1_jxr_zdb .reference}

Log Service \(abbreviated to LOG\) is a platform service specific to logs, which supports real-time collection, storage, and delivery of various types of logs. Besides, Log Service synchronizes data between MaxCompute tables and can ship logs to MaxCompute for big data analysis.

Besides the Log Service console, you can use Application Programming Interfaces \(APIs\) to write and query logs, and manage your projects and Logstores. Currently, the following APIs are available. Currently, the following APIs are available:

|Objects|Method|
|:------|:-----|
|[Log](reseller.en-US/API Reference/Common resources/Data model.md)|Basic concepts of logs and log groups|
|  [Project](reseller.en-US/API Reference/Common resources/Data model.md)|  [List](reseller.en-US/API Reference/Project interfaces/ListProject.md), [Create](reseller.en-US/API Reference/Project interfaces/CreateProject.md), [Delete](reseller.en-US/API Reference/Project interfaces/DeleteProject.md), [Get](reseller.en-US/API Reference/Project interfaces/GetProject.md)|
|Project|[GetProjectLogs](reseller.en-US/API Reference/Project interfaces/GetProjectLogs.md). Count all the logs in a project.|
|[Config](reseller.en-US/API Reference/Common resources/Logtail configuration.md) \(Configuration\)|[List](reseller.en-US/API Reference/Logtail configuration related interfaces/ListConfig.md), [Create](reseller.en-US/API Reference/Logtail configuration related interfaces/CreateConfig.md),  [Delete](reseller.en-US/API Reference/Logtail configuration related interfaces/DeleteConfig.md), [Get](reseller.en-US/API Reference/Logtail configuration related interfaces/GetConfig.md), [Update](reseller.en-US/API Reference/Logtail configuration related interfaces/UpdateConfig.md)|
| |[GetAppliedMachineGroups](reseller.en-US/API Reference/Logtail configuration related interfaces/GetAppliedMachineGroups.md)\(apply/remove a configuration\)|
|[MachineGroup](reseller.en-US/API Reference/Common resources/Machine group.md) \(Machine Group\)|[List](reseller.en-US/API Reference/Logtail machine group related interfaces/ListMachineGroup.md), [Create](reseller.en-US/API Reference/Logtail machine group related interfaces/Createmachinegroup.md), [Delete](reseller.en-US/API Reference/Logtail machine group related interfaces/DeleteMachineGroup.md), [Get](reseller.en-US/API Reference/Logtail machine group related interfaces/GetMachineGroup.md), [Update](reseller.en-US/API Reference/Logtail machine group related interfaces/UpdateMachineGroup.md)|
| |[Apply](reseller.en-US/API Reference/Logtail machine group related interfaces/ApplyConfigToMachineGroup.md)/[Remove](reseller.en-US/API Reference/Logtail machine group related interfaces/RemoveConfigFromMachineGroup.md)|
| |[GetAppliedConfigs](reseller.en-US/API Reference/Logtail machine group related interfaces/GetAppliedConfigs.md) \(query the list of applied configurations\)|
|[LogStore](reseller.en-US/API Reference/Common resources/Logstore.md) \(Logstore\)|[List](reseller.en-US/API Reference/Logstore related APIs/ListLogstore.md), [Create](reseller.en-US/API Reference/Logstore related APIs/CreateLogstore.md),  [Delete](reseller.en-US/API Reference/Logstore related APIs/DeleteLogstore.md), [Get](reseller.en-US/API Reference/Logstore related APIs/GetLogstore.md), [Update](reseller.en-US/API Reference/Logstore related APIs/UpdateLogstore.md)|
| |[GetLogs](reseller.en-US/API Reference/Logstore related APIs/GetLogs.md) \(query logs\) and [GetHistograms](reseller.en-US/API Reference/Logstore related APIs/GetHistograms.md) \(query log distribution\)|
|Index|[Create](reseller.en-US/API Reference/Logstore related APIs/CreateIndex.md), [Update](reseller.en-US/API Reference/Logstore related APIs/UpdateIndex.md), [Delete](reseller.en-US/API Reference/Logstore related APIs/DeleteIndex.md)[GetIndex](reseller.en-US/API Reference/Logstore related APIs/GetIndex.md)|
|[Shard](../../../../../reseller.en-US/Product Introduction/Basic concepts/Shard.md#) \(Partition\)|[List](reseller.en-US/API Reference/Logstore related APIs/ListShards.md), [Split](reseller.en-US/API Reference/Logstore related APIs/SplitShard.md), [Merge](reseller.en-US/API Reference/Logstore related APIs/MergeShards.md)|
| |  [PostLogStoreLogs](reseller.en-US/API Reference/Logstore related APIs/PostLogstoreLogs.md)\(write a log\)|
| |  [GetCursor](reseller.en-US/API Reference/Logstore related APIs/GetCursor.md) \(locate the log location\)|
| |[PullLogs](reseller.en-US/API Reference/Logstore related APIs/PullLogs.md)\(consume a log\)|
|Shipper|[GetShipperStatus](reseller.en-US/API Reference/Logstore related APIs/GetShipperStatus.md)\(query the status of a LogShipper task\)|
| |[RetryShipperTask](reseller.en-US/API Reference/Logstore related APIs/Retryshippertask.md)\(retry a failed LogShipper task\)|
|  ConsumerGroup|[Create](reseller.en-US/API Reference/Consumer group interfaces/CreateConsumerGroup.md), [Update](reseller.en-US/API Reference/Consumer group interfaces/UpdateConsumerGroup.md), [Delete](reseller.en-US/API Reference/Consumer group interfaces/DeleteConsumerGroup.md), [List](reseller.en-US/API Reference/Consumer group interfaces/ListConsumerGroup.md)|
| |[HeartBeat](reseller.en-US/API Reference/Consumer group interfaces/HeartBeat.md), [GetCheckpoint](reseller.en-US/API Reference/Consumer group interfaces/GetCheckPoint.md), [UpdateCheckpoint](reseller.en-US/API Reference/Consumer group interfaces/UpdateCheckPoint.md)|

You can use the APIs to:

-   Collect logs based on [Logtail configuration](reseller.en-US/API Reference/Common resources/Logtail configuration.md) and [Machine group](reseller.en-US/API Reference/Common resources/Machine group.md).
-   Create [Logstore](reseller.en-US/API Reference/Common resources/Logstore.md). Then, write and read logs to/from the Logstore.
-   Set [access control rules](reseller.en-US/API Reference/RAM__STS/Overview.md) for different users.

**Note:** 

-   Currently, APIs provide the [Rest](http://en.wikipedia.org/wiki/Representational_state_transfer) style.
-   To use APIs, you must know [Service endpoint](reseller.en-US/API Reference/Service endpoint.md).
-   Security verification is required for all API requests. For more information about the signature and process of API requests, see [Request signature](reseller.en-US/API Reference/Request signature.md) .
-   Log Service supports Resource Access Management \(RAM\) and Security Token Service \(STS\).  Similar to common cloud accounts, RAM sub-accounts can use APIs by using their AccessKey signature. To use the STS temporary identity, you must use the temporary AccessKey and enter a special HTTP header.  For HTTP header, see[Public request header](reseller.en-US/API Reference/Public request header.md), This HTTP header must participate in the signature. For more information, see  [Request signature](reseller.en-US/API Reference/Request signature.md).

