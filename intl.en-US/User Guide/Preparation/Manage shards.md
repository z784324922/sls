# Manage shards {#concept_yks_jj5_vdb .concept}

A Logstore stores logs in shards. It reads logs from and writes logs into a shard. Each Logstore can have several shards. You must specify the number of shards when creating a Logstore. After creating a Logstore, you can also split or merge shards to increase or decrease the number of shards.

For existing shards, you can:

-   [Split a shard](#)
-   [Automatically split shards](#)
-   [Merge shards](#)
-   [Delete a shard](#)

## Split a shard {#section_awx_35f_vdb .section}

Each shard can write data at 5 Mbit/s and read data at 10 Mbit/s. When the data traffic exceeds the service capacity of existing shards, we recommend that you add shards immediately. A shard can be split to scale out its service capacity.

**Instructions**

When splitting a shard, you must specify the ID of a shard in the read/write state and an MD5 value. The MD5 value must be greater than the BeginKey of the specified shard and less than the EndKey of the specified shard.

A split operation can split a shard into two shards. After the split operation, the original shard still exists and two more shards are added. After a shard is split, the status of the original shard is changed from read/write to read-only. In this shard, data can still be consumed, but new data cannot be written. The two new shards are in the read/write state and arranged behind the original shard in the shard list. The MD5 value range of these two shards covers that of the original shard.

1.  Log on to the Log Service console.
2.  On the Projects page, click the target project name.
3.  On the Logstores page, find the target Logstore and click **Modify** in the Actions column.
4.  In the Shard Management section of the dialog box that appears, select a shard to be split and click **Split** in the Actions column.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13025/15620497292594_en-US.png)

5.  Confirm the splitting position of the shard.

    By default, a shard is evenly split. The splitting position is in the middle of the MD5 value range of the current shard. You can also select another value to specify the number of shards that the current shard is split into.

6.  Click **Confirm** and close the dialog box.

    After the shard is split, the status of the original shard is changed from read/write to read-only. The MD5 value range of two new shards covers that of the original shard.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13025/15620497292595_en-US.png)


## Automatically split shards {#section_zw3_x4v_22b .section}

**Instructions**

Apart from manual sharding, Log Service also supports automatic sharding. You can enable automatic sharding when creating or modifying a Logstore. You can also specify the maximum number of shards after automatic sharding.

After automatic sharding is enabled, shards are automatically split if the following conditions are met:

1.  The data traffic exceeds the service capacity of existing shards and the situation lasts for more than 5 minutes.
2.  The number of shards in the read/write state in the Logstore does not exceed the specified maximum number of shards.

**Note:** New shards that were split in the last 15 minutes are not automatically split.

![](images/6426_en-US.png "Automatically split shards")

|Parameter|Description|
|:--------|:----------|
|Automatic Sharding|The switch used to enable or disable automatic sharding. After this feature is enabled, the shards that meet the specified conditions are automatically split when the data traffic exceeds the service capacity of existing shards.|
|Maximum Shards|The maximum number of shards after automatic sharding. After automatic sharding is enabled, a shard can be split into a maximum of 64 shards.|

## Merge shards {#section_tqj_l5f_vdb .section}

Shards can be merged to scale in their service capacity. You can merge shards whose MD5 value range is adjacent into a new shard in the read/write state. The MD5 value range of the new shard covers the total range of the original two shards. The original two shards are now in the read-only state.

**Instructions**

When merging shards, you must specify a shard in the read/write state. You cannot specify the last shard in the read/write state in the shard list. The server automatically finds the shard whose MD5 value range is adjacent to that of the specified shard and merges these two shards. After the shards are merged, the status of the original shards is changed from read/write to read-only. In these shards, data can still be consumed, but new data cannot be written. In the meantime, a shard in the read/write state is generated and its MD5 value range covers the total range of the original two shards.

**Procedure**

1.  Log on to the Log Service console.
2.  On the Projects page, click the target project name.
3.  On the Logstores page, find the target Logstore and click **Modify** in the Actions column.
4.  In the Shard Management section of the dialog box that appears, select a shard to be merged and click **Merge** in the Actions column. Then, close the dialog box.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13025/15620497292596_en-US.png)

    After the shards are merged, the status of the original shards is changed from read/write to read-only. The MD5 value range of the new shard in the read/write state covers the total range of the original two shards.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13025/15620497292597_en-US.png)


## Delete a shard {#section_m3g_45f_vdb .section}

The lifecycle of a Logstore, namely, the data retention period can be configured in the range of 1 to 3,000 days. Shards and log data in the shards are automatically deleted after the specified data retention period expires. Shards in the read-only state are free of charge. You can also enable Permanent Storage to permanently store the log data in a Logstore. The shards and log data in the shards are never deleted automatically. For more information about how to modify the data retention period of log data, see [Manage a Logstore](intl.en-US/User Guide/Preparation/Manage a Logstore.md).

You can also delete a Logstore to delete all the shards in the Logstore.

