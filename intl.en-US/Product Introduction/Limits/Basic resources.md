# Basic resources {#concept_abm_gdd_p2b .concept}

|Resource|Limit|Remarks|
|:-------|:----|:------|
|Project|You can create a maximum of 50 projects for an Alibaba Cloud account.|If you request more quotas, open a ticket.|
|Logstore|You can create a maximum of 200 Logstores for a project.|If you request more quotas, open a ticket.|
|Shard| -   You can create a maximum of 200 shards for a project.
-   When creating a Logstore in the console, you can create a maximum of 10 shards in the Logstore. When creating a Logstore by using the API, you can create a maximum of 100 shards in the Logstore. However, you can split shards to increase the number of shards no matter how you create a Logstore.

 |If you request more quotas, open a ticket.|
|Logtail configuration|You can create a maximum of 100 Logtail configurations for a project.|If you request more quotas, open a ticket.|
|Log storage time|You can store logs permanently. You can also customize the log storage time in the range of 1-3,000 days.

 |N/A|
|Machine group|You can create a maximum of 100 machine groups for a project.|If you request more quotas, open a ticket.|
|Consumer group|You can create a maximum of 20 consumer groups for a Logstore.|You can delete consumer groups that are no longer used.|
|Saved search|You can create a maximum of 100 saved search items for a project.|N/A|
|Dashboard| -   You can create a maximum of 50 dashboards for a project.
-   Each dashboard can contain a maximum of 50 charts.

 |N/A|
|Log item|The maximum length is 1 MB.|This limit applies if you call API operations to collect logs. If you configure Logtail to collect logs, the maximum length for a single log item is 512 KB.|
|Log item \(Key\)|The maximum length is 128 bytes.|N/A|
|Log item \(Value\)|The maximum length is 1 MB.|N/A|
|Log group|The maximum length of a log group is 5 MB.|N/A|
|Alert|You can create a maximum of 100 alerts for a project.|If you request more quotas, open a ticket.|

