# Limits {#concept_abm_gdd_p2b .concept}

## Basic resources {#section_zbw_51s_q2b .section}

|Resources|Limits|Note|
|:--------|:-----|:---|
|Project|Up to 50 projects can be created for each account.|If you have an extra demand, please open a ticket to apply for support.|
|Logstore|Up to 200 Logstores can be created in each project.|If you have an extra demand, please open a ticket to apply for support.|
|Shard| -   Up to 200 shards can be created in each project.
-   Up to 10 shards can be created in each Logstore. You can increase the number of shards by splitting shards.
-   The maximum number of shards in each Logstore is 512.

 |If you have an extra demand, please open a ticket to apply for support.|
|LogtailConfig|Up to 100 LogtailConfigs can be created for each project.|If you have an extra demand, please open a ticket to apply for support.|
|Log storage time|Permanent storage is supported.You can also customize the log storage time in the range of 1 to 3000.

|-|
|Machine group|Up to 100 machine groups can be created for each project.|If you have an extra demand, please open a ticket to apply for support.|
|Consumer group|Up to 10 consumer groups can be created for each project.|You can delete unused consumer groups.|
|Quick query|Up to 20 quick queries can be created for each project.|-|
|Dashboard| -   Up to 20 dashboards can be created for each project.
-   Each dashboard can contain up to 50 analysis charts.

 |-|
|LogItem|The maximum length of a LogItem is 1 MB.|1 MB is for the API parameter. If Logtail is used to collect logs, the maximum length for a single LogItem is 512 KB.|
|LogItem \(Key\)|The maximum length is 128 bytes.|-|
|LogItem \(Value\)|The maximum length is 1 MB.|-|
|Log group|Each log group contains up to 4096 logs and the maximum length of a log is 10 MB.|-|

## Data read and write {#section_ifv_gcs_q2b .section}

|Resource|Limit|Description|Note|
|:-------|:----|:----------|:---|
|Project|Write traffic protection|The write traffic is up to 30 GB/min.|If the limit is exceeded, the status code of 403 is returned, prompting Inflow Quota Exceed. If you have an extra demand, please open a ticket to apply for support.|
|Number of writes protection|The maximum number of writes is 600000 per minute.|If the limit is exceeded, the status code of 403 is returned, prompting Write QPS Exceed. If you have an extra demand, please open a ticket to apply for support.|
|Number of reads protection|The maximum number of reads is 600000 per minute.|If the limit is exceeded, the status code of 403 is returned, prompting Read QPS Exceed. If you have an extra demand, please open a ticket to apply for support.|
|Shard|Write traffic|The maximum write traffic is 5 MB/s.|Not required. When the limit is exceeded, the system serves as much as possible, but does not guarantee the service quality.|
|Number of writes.|The maximum number of writes is 500 per second.|Not required. When the limit is exceeded, the system serves as much as possible, but does not guarantee the service quality.|
|Read traffic|The maximum read traffic is 10 MB/s.|Not required. When the limit is exceeded, the system serves as much as possible, but does not guarantee the service quality.|
|Number of reads|The maximum number of reads is 100 per second.|Not required. When the limit is exceeded, the system serves as much as possible, but does not guarantee the service quality.|

## Search, analysis, and visualization {#section_oqm_xds_q2b .section}

|Function|Item|Limit|Note|
|:-------|:---|:----|:---|
|Query|Number of keywords|The number of conditions specified for querying words besides Boolean logical operators. You can query up to 30 keywords each time.|For example, "a and b or c and d...".|
|The length of a single value.|The maximum length of a single value is 10 KB. The excess part of the value is not queried.|If the length of a single value is greater than 10 KB, the log might not be found through keywords, but the data is still complete.|
|Single project concurrency|The number of single project concurrency is up to 100.|-|
|Number of entries of returned query results|By default, a maximum of 100 entries of query results are returned each time.|You can read the full query results by turning pages.|
|SQL analysis|Maximum length of a single value|The maximum length of a single value is 2 KB. The excess part of the value is not queried.|Query results might not be accurate when the limit is exceeded, but the data is still complete.|
|Single project concurrency|The number of single project concurrency is up to 30.|-|
|Number of entries of results in each analysis|Results returned by each analysis are up to 100 MB or 100000 entries.|-|

