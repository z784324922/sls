# Billing method {#concept_y3m_g5n_vdb .concept}

Log Service is billed by resource usage on a tiered basis each month. **LogShipper** is free of charge. You can ship log data to MaxCompute and Object Storage Service \(OSS\) for storage and analysis at no cost.  This document details shows **Billing method**, **how to charge** and **billing examples**.

## Billing method {#section_qbv_lb1_d2b .section}

**Billing instructions**

-   **The billing cycle is one day**: The bill is sent to you every day and the service is billed by the resource usage in the day.
-   **The FreeTier quota cycle is one month** and the remaining quota will be cleared at the end of the month.  If your resource usage does not exceed the FreeTier quota, no charge is collected. Otherwise, the part exceeding the quota is charged.
-   **Any billing item of less than USD 0.01 is excluded from the bill.**

The resource billing items and unit prices are listed in the following table.

|Billing items|Meaning|for a MapReduce|FreeTier quota \(per month\)|Example |
|:------------|:------|:--------------|:---------------------------|:-------|
|Read and write traffic|The read and write traffic is calculated by the traffic for transmitting compressed logs. Logs are automatically compressed in SDK/Logtail mode, but need to be manually compressed in APIs mode. Logs are generally compressed by 5 to 10 times.|USD 0.045/GB |500 MB|If the size of the raw logs is 10 GB and the size of logs after compression is 1.5 GB, the logs are charged by 1.5 GB.|
|Storage space|The storage space is the sum of data size after compression and the indexed data size.|USD 0.002875/GB *per day*|500 MB|The data size is 1 GB per day. After compression, the data size is 200 MB. 10% of the data is used for index \(the size is 100 MB\).  After a storage period of 30 days,   the accumulative maximum storage size is 30 x \(100 + 200\) = 9 GB. The maximum fee for storage is 0.002875 x 9 ≈ USD 0.026 per day. USD 0.026 per day.|
|Indexing traffic| -   The indexing traffic is calculated by actual index fields. Storage fee is collected in full during writing.
-   The traffic of fields having both FullText indexes and KeyValue indexes is calculated only once.
-   Indexes occupy the storage space and thus the storage space fee is collected.

 |USD 0.0875/GB|500 MB|For example, if 10% fields in 10 GB logs need to be queried, only the traffic used to query the fields is charged. The indexing traffic fee is USD 0.0875.|
|**Other billing items** \(to restrict the abuse of resources, the price of the following billing items is low by default\)|
|Active shard rent|Only shards currently in readwrite status are counted. Rent of merged/split shards is not collected. |USD 0.01/day|31/day|For example, one of the three shards is in readwrite status. The other two shards are  merged and in readonly status. The rent of only one shard \(USD 0.01/day\) is collected.|
|Read/write count |The write count of logs into the Log Service is subject to the log generation speed. The background realization mechanism assures the minimum read/write count as much as possible.|USD 0.03/1 million times|1,000,000 times|You can use Logtail for automatic batch sending and a total of USD 0.03 is collected for one million write operations.|
|Internet read traffic|The data traffic generated when Internet programs read log data collected by Log Service.|USD 0.2/GB|None. |The fee of Internet read traffic is USD 0.4 when 2 GB data is shipped by Log Service to non-Alibaba Cloud products.|

## Deduction and outstanding payment {#section_l45_r5n_vdb .section}

-   A bill is generally provided within four hours after the current billing cycle is finished. The system automatically deducts the bill amount from you amount balance. If the account balance is insufficient, your account is in arrears.

-   If the overdue bill is not paid off within 24 hours, your service is automatically stopped. However, you are still charged for using the storage space,   so the overdue amount will increase. We recommend that you pay off the overdue bill within 24 hours to avoid any business loss caused by service stop.

-   If the overdue bill is paid off within six days after the service is stopped, the service is automatically restored.  Otherwise, Alibaba Cloud will assume that you have chosen to stop using the service, the project space will be reclaimed and data in it will be cleared. This data will not be recoverable.


## Billing example {#section_zfy_s5n_vdb .section}

## Case 1: FreeTier quota {#section_hq3_wb1_d2b .section}

You have three servers, of which one server generates 5 MB logs per day. You want to use a program to process the logs as follows:

1.  Query the logs in real time and create a dashboard for online Operation & Maintenance \(O&M\).
2.  Use a Java program to subscribe to log processing in real time.
3.  Ship the logs to OSS.

**Billing details**:

-   Resource: One Logstore \(one shard\) is created per day. 31 Logstores \(one shard\) are created in a month in total, not exceeding the quota.
-   Read and write traffic: The read and write traffic per day is 15/5 \(data after compression\) \* 2 \(read + write\) = 6 MB \(after compression\). The accumulated read and write traffic in a month is 6 \* 31 \(days\) = 183  MB, not exceeding the quota.
-   Indexing traffic: 15 \(raw data\) \* 31 \(days\) = 465 MB, not exceeding the quota.
-   Read/write count: The read/write count in a month is less than one million, not exceeding the quota.

**You can use Log Service for online log analysis and processing for free each month.**

## **Case 2: Real-time computing + offline computing \(Lamdba Architecture\)** {#section_vk4_1c1_d2b .section}

The website has 100 million API requests per day. Each request generates a 200-byte log, and the size of logs generated per day is 20 GB. The peak traffic is 5 times of the average traffic, 1.16 MB/s \(less than 5 Mb/s\). The logs are read once per day for real-time computing \(the life cycle is two days\) and imported to OSS for offline computing \(Hive/Spark\).

**Billing details**:

-   Active shard rent: One shard is reserved. The fee is USD 0.01/day.
-   Read/write count: Use Logtail for automatic batch sending and a total of USD 0.03 is collected for one million write operations.
-   Read and write traffic
    -   The write traffic is 20 GB. If the compression rate is 10%, the actual traffic is 2 GB and the fee is 2 \* 0.045 = USD 0.09.
    -   The read traffic is the same as the write traffic in real-time computing, which is USD 0.09.
-   Storage space: The storage size is 2 GB \* 2 \(days\) = 4 GB. The storage fee per day is 4 \* 0.002875 = USD 0.0115.

-   Importing logs to OSS is free of charge.

**The maximum fee per day is 0.01 + 0.03+ 0.09 \* 2 + 0.115 = USD 0.335.**

## Case 3: Online log query and analysis {#section_hhx_cc1_d2b .section}

The service has one million API access requests per day. Each request generates a 200-byte log, and the size of logs generated per day is 200 MB. Logs of the last 30 days are saved for query.

**Billing details**:

-   Active shard rent: One shard is reserved. The fee is USD 0.01/day.
-   Read/write count: Use Logtail for automatic batch sending and a total of USD 0.03 is collected for one million write operations.
-   Read and write traffic: The write traffic is 200 MB. The data size after compression is 0.05 GB. The read and write traffic fee per day is 0.05 \* 0.045 = USD 0.00225. 
-   Indexing traffic: The indexing fee per day is 0.2 \* 0.0875 = USD 0.0175.
-   Storage space: 200 MB + 50 MB \(compressed raw data\) = 250 MB. The storage peak size is 250 \* 30 = 7.5 GB. The storage fee per day is 7.5 \* 0.002875 =  USD 0.022.

**The maximum fee per day is 0.01 + 0.03 + 0.00225 + 0.0175 + 0.22 = USD 0.08175.**

