# Shard {#concept_wnn_rqn_vdb .concept}

Logstore read/write logs must be stored in a certain shard.  Each Logstore is divided into several shards and each shard is composed of MD5 left-closed and right-open intervals. Each interval range does not overlap with others and the total range of all the intervals is the entire MD5 value range.

## Range {#section_fvp_rgd_jbb .section}

You must specify the number of shards when creating a Logstore. Then, the entire MD5 range is automatically divided evenly according to the specified number of shards.  Each shard has a range, which can be expressed in the MD5 mode and must be within the following value range: \[00000000000000000000000000000000,ffffffffffffffffffffffffffffffff\).

All of the shard ranges are left-closed and right-open intervals, and composed of the following keys:

-   BeginKey: Indicates the start of the shard. This key is included in the shard range.
-   EndKey: Indicates the end of the shard. This key is excluded from the shard range.

With the shard range, you can write logs by specifying Hash Key, split shards, and merge shards. To read data from a shard, you must specify the corresponding shard. To write data to a shard, you can use Server Load Balancer or specify the Hash Key. By using Server Load Balancer, each data packet is written to an available shard at random. By specifying the Hash Key, data is written to the shard whose range includes the specified key. To read data from a shard, you must specify the corresponding shard. To write data to a shard, you can use Server Load Balancer or specify the Hash  Key.  By using Server Load Balancer, each data packet is written to an available shard at random. By specifying the Hash Key, data is written to the shard whose range includes the specified key.

For example, a Logstore has four shards and the MD5 value range of this Logstore is \[00,FF\).  Each shard range is as follows.

|Shard No. |Range |
|:---------|:-----|
|Shard0|\[00,40\) |
|Shard1|\[40,80\)|
|Shard2|\[80,C0\)|
|Shard3|\[C0,FF\)|

If you specify the MD5 key as 5F by specifying the Hash  If you specify the MD5 key as 5F by specifying the Hash Key when writing logs, the log data is written to Shard1 that contains the MD5 key 5F. If you specify the MD5 key as 8C, the log data is written to Shard2 that contains the MD5 key 8C.

## Read/write capacities {#section_p5r_tgd_jbb .section}

Each shard has certain service capacities:

-   Writing: 5 MB/s, 2000 times/s
-   Read: 10 MB/s, 100 times/s

We recommend that you plan the number of shards according to the actual data traffic. If the traffic exceeds the read/write capacities, split the shard in time to increase the number of shards so as to achieve greater read/write capacities. If the traffic is far less than the maximum read/write capacities of shards, we recommend that you merge the shards to reduce the number of shards so as to save the rental costs of shards.

For example, assume that you have two shards in readwrite status and can write data at 10 MB/s at maximum. If you write data at 14 MB/s in real time, we recommend that you split a shard to make the number of shards in readwrite status reach three.  If you write data at only 3 MB/s in real time, we recommend that you merge these two shards because one shard can meet the needs.

**Note:** 

-   If the API consistently reports error 403 or 500 during the writing, see Log Service monitoring metrics to determine whether to increase the number of shards.
-   For read/write operations that exceed the service capacities of shards, the system attempts to provide the needed services, but the service quality cannot be guaranteed.

## Status  {#section_amr_vgd_jbb .section}

The shard status includes:

-   readwrite: Supports reading and writing data.
-   readonly: Only supports reading data.

When a shard is created, all the shards are in readwrite status. Split or merge operations change the shard status to readonly and generate a new shard in readwrite status.  The shard status does not affect the performance of reading data. Shards in readwrite status maintain normal data writing performance, while shards in readonly status do not support writing data.

When splitting a shard, you must specify a ShardId in readwrite status and an MD5.  The MD5 must be greater than the shard BeginKey and less than the shard EndKey.  Split operations can split two other shards from one, that is, the number of shards is increased by 2 after the split.  After the split, the status of the original shard specified to be split is changed from readwrite to readonly. Data can still be consumed, while new data cannot be written.  The two newly generated shards are in readwrite status and arranged behind the original shard. The MD5 range of these two shards covers the range of the original shard.

When merging shards, you must specify a shard in readwrite status. Make sure the specified shard is not the last shard in readwrite status.  The server automatically finds the adjacent shard at the right of the specified shard and merges these two shards.  After the merge, the specified shard and the adjacent shard on the right are in readonly status. Data can still be consumed,  while new data cannot be written.  A new shard in readwrite status is generated and its MD5 range covers the total range of the original two shards.

