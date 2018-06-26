# Cost optimization {#concept_ih5_fvn_vdb .concept}

Cost is related to two factors:

-   Data volume. Data volume is determined by your business needs and cannot be optimized.The amount of user data is determined by the business and cannot be optimized.
-   •Configurations. Configurations can be optimized. With the configurations matching with the amount of data, you can select the optimal solution to minimize the cost.

## Optimizing configurations {#section_mh5_hvn_vdb .section}

Configurations can be optimized in the following two aspects:

-   Number of shards

    The price for one shard is USD 0.04 per day, with a maximum data processing capability of 5 MB/s.  Only shards in readwrite status are charged.  Adjust the number of shards so that each shard can process data exactly at 5 MB/s.  To reduce the number of shards, merge the shards. 

-   Storage cycle of indexes

    We recommend that you optimize the storage time of indexes based on your requirements for log query and storage.

    -   If you collect the logs for stream computing, we recommend that you only use LogHub, without creating indexes.

    -   If you want to query the logs within the past 90 days, and barely query the logs earlier than that, we recommend that you change the storage time of indexes to 90 days, and import the data to MaxCompute.  To query data within 90 days, use Log Service. To query data earlier than that, use MaxCompute.

    -   If you want to store and back up logs for a long time, we recommend that you configure the Object Storage Service \(OSS\) Shipper, and import the logs to OSS.


## Other optimization recommendations {#section_mkt_3vn_vdb .section}

-   Use Logtail: With the functions of batch processing and breakpoint transmission, data is transmitted with optimal algorithm while guaranteeing the real-timeliness. Logtail consumes 3/4 less resources than that of the open-source software \(such as Logstash and Fluentd\), thus reducing CPU consumption.
-   Try to use large packages \(64 KB–1 MB\) to write logs by using API, thus reducing the number of requests.
-   Only index key fields \(for example, UserID and Action\), without configuring indexes for useless fields.

