# ORDER BY syntax {#concept_zks_zlq_zdb .concept}

ORDER BY is used to sort the output results. Currently, you can only sort the results by one column.

  **Syntax format**:

```
order by Column name [desc|asc]
```

**Example**:

```
method :PostLogstoreLogs |select avg(latency) as avg_latency,projectName group by projectName 
HAVING avg(latency) > 5700000
order by avg_latency desc
```

