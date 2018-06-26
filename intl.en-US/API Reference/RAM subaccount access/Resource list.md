# Resource list {#reference_sdz_l5q_12b .reference}

**Types of Log Service resources that can be authorized in RAM**

The types of resources that  can be authorized in Resource Access Management \(RAM\) and the description methods are as follows.

|Resource type|Description method in authorization policy|
|:------------|:-----------------------------------------|
|Project/Logstore|acs:log:$\{regionName\}:$\{projectOwnerAliUid\}:project/$\{projectName\}/logstore/$\{logstoreName\}|
|Project/Logstore/Shipper|acs:log:$\{regionName\}:$\{projectOwnerAliUid\}:project/$\{projectName\}/logstore/$\{logstoreName\}/shipper/$\{shipperName\}|
| |ACS: Log: $ \{regionname\}: $ \{maid \}: project/$ \{project name\}/logstore /\*|
|Project/Config|acs:log:$\{regionName\}:$\{projectOwnerAliUid\}:project/$\{projectName\}/logtailconfig/$\{logtailconfig\}|
| |ACS: Log: $ \{regionname\}: $ \{maid \}: project/$ \{project name\}/logtailconfig /\*|
|Project/MachineGroup|acs:log:$\{regionName\}:$\{projectOwnerAliUid\}:project/$\{projectName\}/machinegroup/$\{machineGroupName\}|
| |acs:log:$\{regionName\}:$\{projectOwnerAliUid\}:project/$\{projectName\}/machinegroup/\*|
|Project/ConsumerGroup|acs:log:$\{regionName\}:$\{projectOwnerAliUid\}:project/$\{projectName\}/logstore/$\{logstoreName\}/consumergroup/$\{consumerGroupName\}|
| |acs:log:$\{regionName\}:$\{projectOwnerAliUid\}:project/$\{projectName\}/logstore/$\{logstoreName\}/consumergroup/\*|
|Project/SavedSearch|acs:log:$\{regionName\}:$\{projectOwnerAliUid\}:project/$\{projectName\}/savedsearch/$\{savedSearchName\}|
| |acs:log:$\{regionName\}:$\{projectOwnerAliUid\}:project/$\{projectName\}/savedsearch/\*|
|Project/Dashboard|acs:log:$\{regionName\}:$\{projectOwnerAliUid\}:project/$\{projectName\}/dashboard/$\{dashboardName\}|
| |acs:log:$\{regionName\}:$\{projectOwnerAliUid\}:project/$\{projectName\}/dashboard/\*|
|Project/alarm|acs:log:$\{regionName\}:$\{projectOwnerAliUid\}:project/$\{projectName\}/alert/$\{alarmName\}|
| |acs:log:$\{regionName\}:$\{projectOwnerAliUid\}:project/$\{projectName\}/alert/\*|
|Generic mode|acs:log:$\{regionName\}:$\{projectOwnerAliUid\}:\*|
| |acs:log:\*:$\{projectOwnerAliUid\}:\*|

**Note:**  A hierarchical relationship is in the Log Service resources. The project is a top-level resource. Logstore, configuration, and machine group are at the same level and sub-resources of the project. Log shipping rule and consumer group are sub-resources of the Logstore.

See the table below for a description of each parameter.

|Parameters|Description|
|:---------|:----------|
|`${regionName}`|indicates the name of a region.|
|`${projectOwnerAliUid}`|indicates your Alibaba Cloud account ID.|
|`${projectName}`|indicates the name of a Log Service project.|
|`$ {Logstorename}`| indicates the name of a Logstore.|
|`${logtailconfig}`| indicates the name of a configuration.|
|`${machineGroupName}`|indicates the name of a machine group.|
|`${shipperName}`| indicates the name of a log shipping rule.|
|`${consumerGroupName}`|indicates the name of a consumer group.|
|`${savedSearchName}`| indicates the name of a saved search.|
|`${dashboardName}`|indicates the name of a dashboard.|
|`${alarmName}`|indicates the name of an alarm rule.|

