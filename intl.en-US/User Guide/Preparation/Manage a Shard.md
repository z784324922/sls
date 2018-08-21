# Manage a Shard {#concept_yks_jj5_vdb .concept}

Logstore read/write logs must be stored in a certain shard.  Each Logstore is divided into several shards. You must specify the number of shards when creating a Logstore. You can also split a shard or merge shards to increase or reduce the number of shards.

For existing shards, you can:

-   [Split a shard](#section_awx_35f_vdb)
-   [Merge shards](#section_tqj_l5f_vdb)
-   [Delete a shard](#section_m3g_45f_vdb)

## Split a shard {#section_awx_35f_vdb .section}

Each shard can write data at 5 MB/s and read data at 10 MB/s. When the data traffic exceeds the service capacity of the shard, we recommend that you increase the number of shards in time by splitting a shard. The expansion partition is completed by split operation.

**Instructions**

When splitting a shard, you must specify a ShardId in readwrite status and an MD5.  The MD5 must be greater than the shard BeginKey and less than the shard EndKey.

Split operations can split two other shards from one, that is, the number of shards is increased by 2 after the split. After the split, the status of the original shard specified to be split is changed from readwrite to readonly. Data can still be consumed, while new data cannot be written. The two newly generated shards are in readwrite status and arranged behind the original shard. The MD5 range of these two shards covers the range of the original shard. 

1.  Log on to the Log Service console.
2.  On the Project List page, click the project name. 
3.  On the Logstore List page, click **Modify** at the right of the Logstore.
4.  Click **Split** at the right of the shard to be split.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13025/15348425722594_en-US.png)

5.  Click **Confirm**  and close the dialog box.

    After the split, the status of the original shard is changed to readonly, and the MD5 range of the two newly generated shards covers the range of the original shard.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13025/15348425722595_en-US.png)


## Merge shards {#section_tqj_l5f_vdb .section}

You can reduce the number of shards by merging shards. The ranges of the specified shard and the adjacent shard on the right are merged. A new shard in readwrite status is generated and its MD5 range covers the total range of the original two shards. The original two shards are now in the readonly status. 

**Instructions**

When merging shards, you must specify a shard in readwrite status. Make sure the specified shard is not the last shard in readwrite status. The server automatically finds the adjacent shard at the right of the specified shard and merges these two shards. After the merge, the specified shard and the adjacent shard on the right are in readonly status. Data can still be consumed, while new data cannot be written. A new shard in readwrite status is  generated and its MD5 range covers the total range of the original two shards.

**Procedure**

1.  Log on to the Log Service console.
2.  On the Project List page, click the project name. 
3.  On the Logstore List page, click **Modify** at the right of the Logstore.
4.  Click **Merge**  at the right of the shard to be merged.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13025/15348425722596_en-US.png)

    After the merge, the specified shard and the adjacent shard on the right are changed to the readonly status, and the MD5 range of the newly generated shard in readwrite status covers the total range of the original two shards.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13025/15348425732597_en-US.png)


## Delete a shard {#section_m3g_45f_vdb .section}

The Logstore lifecycle, namely, the data retention time can be configured as permanently and 1–3000 days. Shards and log data in the shards are automatically deleted after the specified data retention time.  Shards in readonly status are free of charge.

You can also delete all the shards in a Logstore by deleting a Logstore.

