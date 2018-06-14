# LIMIT语法 {#reference_kfl_2mq_d2b .reference}

LIMIT后跟一个数字，表示最多输出的行数。如果不添加LIMIT语句，默认只输出100行。

**说明：** 不支持Limit offset、lines语法。

**样例**：

```
*| select avg(latency) as avg_latency , methodgroupbymethodorderbyavg_latencydesclimit100
```

