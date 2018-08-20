# Logstore {#reference_q2p_r5q_12b .reference}

The Logstore naming rules are as follows:

-   The name can only contain lowercase letters, numbers, hyphens \(-\), and underscores \(\_\).
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

|Parameter Name|Type|Required|Description|
|:-------------|:---|:-------|:----------|
|logstoreName|string|Yes|The Logstore name, which must be unique in the same project.|
|ttl|Integer|Yes|The Time to Live \(TTL\) of log data. The unit is in days and the minimum value is 1 day.|
|Shardcount|Integer|Yes|The log data service unit.|
|createTime（OutputOnly）|Integer|否|The time when the resource is created in Log Service \(output only\).|
|lastModifyTime（OutputOnly）|Integer|否|The time when the resource is updated in Log Service \(output only\).|

