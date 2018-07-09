# LIMIT syntax {#reference_kfl_2mq_d2b .reference}

LIMIT is followed by a number to indicate the maximum number of rows in the output results.  If no LIMIT statement is added, only 100 rows are output by default.

**Note:** Limit offset and lines syntaxes are not supported.

  **Example**:

```
*| select avg(latency) as avg_latency , methodgroupbymethodorderbyavg_latencydesclimit100
```

