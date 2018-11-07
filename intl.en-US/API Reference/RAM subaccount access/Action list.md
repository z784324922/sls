# Action list {#reference_zq5_m5q_12b .reference}

## Actions in RAM that can be performed on Log Service resources {#section_avr_q35_12b .section}

In Resource Access Management \(RAM\), you can perform the following actions on Log Service resources. Each action corresponds to one or two APIs.

|Action|Description|
|:-----|:----------|
|[log:CreateProject](reseller.en-US/API Reference/Project interfaces/CreateProject.md)|Create a project.|
|[log:GetLogStoreLogs](reseller.en-US/API Reference/Logstore related APIs/GetLogs.md)|Query logs in a Logstore of a specific project.|
|[log:GetLogStoreHistogram](reseller.en-US/API Reference/Logstore related APIs/GetHistograms.md)|Query the distribution of logs in a Logstore of a specified project.|
|[log:GetLogStore](reseller.en-US/API Reference/Logstore related APIs/GetLogstore .md)|View Logstore attributes.|
|[log:ListLogStores](reseller.en-US/API Reference/Logstore related APIs/ListLogstore.md)|List names of all Logstore of a specified project.|
|[log:CreateLogStore](reseller.en-US/API Reference/Logstore related APIs/CreateLogstore.md)|Create a Logstore in a project.|
|[log:DeleteLogStore](reseller.en-US/API Reference/Logstore related APIs/DeleteLogstore.md)|Delete a Logstore, including all shards and indexes in the Logstore.|
|[log:UpdateLogStore](reseller.en-US/API Reference/Logstore related APIs/UpdateLogstore.md)|Update Logstore attributes.|
|log:GetCursorOrData \([GetCursor](reseller.en-US/API Reference/Logstore related APIs/GetCursor.md)，[PullLogs](reseller.en-US/API Reference/Logstore related APIs/PullLogs.md)\)|Get the cursor by time; get logs by the cursor and quantity.|
|[log:ListShards](reseller.en-US/API Reference/Logstore related APIs/ListShards.md)|List all available shards in a Logstore.|
|[log:PostLogStoreLogs](reseller.en-US/API Reference/Logstore related APIs/PostLogstoreLogs.md)|Write log data to a specified Logstore.|
|[log:CreateConfig](reseller.en-US/API Reference/Logtail configuration related interfaces/CreateConfig.md)|Create log configuration in a project.|
|[log:UpdateConfig](reseller.en-US/API Reference/Logtail configuration related interfaces/UpdateConfig.md)|Update configuration.|
|[log:DeleteConfig](reseller.en-US/API Reference/Logtail configuration related interfaces/DeleteConfig.md)|Remove the specified configuration.|
|[log:GetConfig](reseller.en-US/API Reference/Logtail configuration related interfaces/GetConfig.md)|Obtain configuration details.|
|[log:ListConfig](reseller.en-US/API Reference/Logtail configuration related interfaces/ListConfig.md)|List all configurations in a project. You can use the parameter to flip pages.|
|[log:CreateMachineGroup](reseller.en-US/API Reference/Logtail machine group related interfaces/Createmachinegroup.md)|You can create a group of machines to collect logs and deliver configuration.|
|[log:UpdateMachineGroup](reseller.en-US/API Reference/Logtail machine group related interfaces/UpdateMachineGroup.md)|Update machine group information.|
|[log:DeleteMachineGroup](reseller.en-US/API Reference/Logtail machine group related interfaces/DeleteMachineGroup.md)|Delete a machine group|
|[log:GetMachineGroup](reseller.en-US/API Reference/Logtail machine group related interfaces/GetMachineGroup.md)|View specific machine group information.|
|[log:ListMachineGroup](reseller.en-US/API Reference/Logtail machine group related interfaces/ListMachineGroup.md)|View the machine group name list.|
|[log:ListMachines](reseller.en-US/API Reference/Logtail machine group related interfaces/ListMachines.md)|Get the status information of machines that belong to the user and connect to the server under the machine group.|
|[log:ApplyConfigToGroup](reseller.en-US/API Reference/Logtail machine group related interfaces/ApplyConfigToMachineGroup.md)|Apply the configuration to the machine group.|
|[log:RemoveConfigFromGroup](reseller.en-US/API Reference/Logtail machine group related interfaces/RemoveConfigFromMachineGroup.md)|Remove configuration from a machine group.|
|[log:GetAppliedMachineGroups](reseller.en-US/API Reference/Logtail machine group related interfaces/GetAppliedConfigs.md)|Get the name of the configuration that has been applied on the machine group.|
|[log:GetAppliedConfigs](reseller.en-US/API Reference/Logtail machine group related interfaces/GetAppliedConfigs.md)|Obtain the name of the configuration applied to a machine group.|
|[log:GetShipperStatus](reseller.en-US/API Reference/Logstore related APIs/GetShipperStatus.md)|Query the LogShipper task status.|
|[log:RetryShipperTask](reseller.en-US/API Reference/Logstore related APIs/Retryshippertask.md)|Rerun failed LogShipper tasks.|
|[log:CreateConsumerGroup](reseller.en-US/API Reference/Consumer group interfaces/CreateConsumerGroup.md)|Create a consumer group in a specified Logstore.|
|[log:UpdateConsumerGroup](reseller.en-US/API Reference/Consumer group interfaces/UpdateConsumerGroup.md)|Modify the attributes of a specified consumer group.|
|[log:DeleteConsumerGroup](reseller.en-US/API Reference/Consumer group interfaces/DeleteConsumerGroup.md)|Delete a specified consumer group.|
|[log:ListConsumerGroup](reseller.en-US/API Reference/Consumer group interfaces/ListConsumerGroup.md)|Query all consumer groups of a specified Logstore.|
|[log:ConsumerGroupUpdateCheckPoint](reseller.en-US/API Reference/Consumer group interfaces/UpdateCheckPoint.md)|Update the checkpoints of a shard of consumer groups under a specified project and Logstore.|
|[log:ConsumerGroupHeartBeat](reseller.en-US/API Reference/Consumer group interfaces/HeartBeat.md)|Send a heartbeat packet to the server for a specified consumer.|
|[log:GetConsumerGroupCheckPoint](reseller.en-US/API Reference/Consumer group interfaces/GetCheckPoint.md)|Obtain the checkpoints of one or all shards consumed by a specified consumer group.|
|[log:CreateIndex](reseller.en-US/API Reference/Logstore related APIs/CreateIndex.md)|Create an index for a specified Logstore.|
|[log:DeleteIndex](reseller.en-US/API Reference/Logstore related APIs/DeleteIndex.md)|Create an index for a specified Logstore.|
|[log:GetIndex](reseller.en-US/API Reference/Logstore related APIs/GetIndex.md)|Query the index of a specified Logstore.|
|[log:UpdateIndex](reseller.en-US/API Reference/Logstore related APIs/UpdateIndex.md)|Update an index for a specified Logstore.|

