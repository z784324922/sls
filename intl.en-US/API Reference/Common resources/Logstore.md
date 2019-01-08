# Logstore {#reference_q2p_r5q_12b .reference}

The Logstore naming rules are as follows:

-   The name contains only lowercase letters, numbers, hyphens \(-\), and underscores \(\_\).
-   The name must begin and end with a lowercase letter or number.
-   The name must be 3–63 bytes long.

Example of the complete resource:

```
{
    "logstoreName" : "access_log",
    "ttl": 1,
    "shardCount": 2,
    "autoSplit": true,
    "maxSplitShard": 64,
    "createTime":1439538649,
    "lastModifyTime":1439538649
}
```

Parameter definitions

|Parameter name|Type|Required|Description|
|:-------------|:---|:-------|:----------|
|logstoreName|String|Yes|The Logstore name, which must be unique in the same project.|
|ttl|Integer|Yes|The Time to Live \(TTL\) of log data in days. The minimum value is 1.|
|shardCount|Integer|Yes|The log data service unit.|
|autoSplit|Bool|No|Determines whether to automatically split a shard.|
|maxSplitShard|Int|No|The maximum number of shards for automatic split, which is in the range of 1 to 64. You must specify this parameter when autoSplit is true.|
|createTime \(OutputOnly\)|Integer|No|The time when the resource is created in Log Service \(output only\).|
|lastModifyTime \(OutputOnly\)|Integer|No|The time when the resource is updated in Log Service \(output only\).|

