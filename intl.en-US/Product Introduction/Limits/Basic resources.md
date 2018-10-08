# Basic resources {#concept_abm_gdd_p2b .concept}

|Resources|Limit|Note|
|:--------|:----|:---|
|Project|Up to 50 projects can be created for each account.|If you have an extra demand, please open a ticket to apply for support.|
|Logstore|Up to 200 Logstores can be created in each project.|If you have an extra demand, please open a ticket to apply for support.|
|Shard| -   Up to 200 shards can be created in each project.
-   Up to 10 shards can be created in each Logstore. You can increase the number of shards by splitting shards.

 |If you have an extra demand, please open a ticket to apply for support.|
|LogtailConfig|Up to 100 LogtailConfigs can be created for each project.|If you have an extra demand, please open a ticket to apply for support.|
|Log storage time|Permanent storage is supported.You can also customize the log storage time in the range of 1 to 3000.

|-|
|Machine group|Up to 100 machine groups can be created for each project.|If you have an extra demand, please open a ticket to apply for support.|
|Consumer group|Up to 10 consumer groups can be created for eachLogstore.|You can delete unused consumer groups.|
|Quick query|Up to 100 quick queries can be created for each project.|-|
|Dashboard| -   Up to 50 dashboards can be created for each project.
-   Each dashboard can contain up to 50 analysis charts.

 |-|
|LogItem|The maximum length of a LogItem is 1 MB.|1 MB is for the API parameter. If Logtail is used to collect logs, the maximum length for a single LogItem is 512 KB.|
|LogItem \(Key\)|The maximum length is 128 bytes.|-|
|LogItem \(Value\)|The maximum length is 1 MB.|-|
|Log group|Each log group contains up to 4096 logs and the maximum length of a log is 10 MB.|-|

