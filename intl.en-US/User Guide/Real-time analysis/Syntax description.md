# Syntax description {#concept_nyf_cjq_zdb .concept}

Log Service provides a function similar to the SQL aggregate computing. This function integrates with the [query](intl.en-US/User Guide/Index and query/Query syntax.md) function and the SQL computing function to compute the query results.

Syntax example:

```
status>200 |select avg(latency),max(latency) ,count(1) as c GROUP BY method ORDER BY c DESC LIMIT 20
```

Basic syntax:

```
[search query] | [sql query]
```

The SEARCH condition and computing condition are separated by a vertical bar \( `|` \). This syntax Indicates that the required logs are filtered from the log by the search query, and SQL queries are computed for these logs. The search query syntax is specific to Log Service. For details, see [Query syntax](intl.en-US/User Guide/Index and query/Query syntax.md).

## Prerequisites {#section_q2x_lz5_zdb .section}

To use the analysis function, you must click **Enable** of the SQL related fields in **Search and Analysis config**. For more information, see [Overview](intl.en-US/User Guide/Index and query/Overview.md).

-   If you do not enable analysis function, computing function of up to 10 thousand rows of data per shard  is provided, and the delay is relatively high.
-   With the Enable Analytics turned on, Log Service provides the quick analysis in seconds.
-   Only works for new data when function is enabled.
-   No additional charges are incurred after the Enable Analytics is turned on.

## Supported SQL syntax {#section_x2l_vkq_tdb .section}

Log Service supports the following SQL syntaxes. For details, click the specific links.

-   SELECT aggregate computing functions:
    -   [General aggregate functions](intl.en-US/User Guide/Real-time analysis/Analysis grammar/General aggregate functions.md)
    -   [Map map function](intl.en-US/User Guide/Real-time analysis/Analysis grammar/Map map function.md)
    -   [Estimating functions](intl.en-US/User Guide/Real-time analysis/Analysis grammar/Estimating functions.md)
    -   [Mathematical statistics functions](intl.en-US/User Guide/Real-time analysis/Analysis grammar/Mathematical statistics functions.md)
    -   [Mathematical calculation functions](intl.en-US/User Guide/Real-time analysis/Analysis grammar/Mathematical calculation functions.md)
    -   [String functions](intl.en-US/User Guide/Real-time analysis/Analysis grammar/String functions.md)
    -   [Date and time functions](intl.en-US/User Guide/Real-time analysis/Analysis grammar/Date and time functions.md)
    -   [URL functions](intl.en-US/User Guide/Real-time analysis/Analysis grammar/URL functions.md)
    -   [Regular expression functions](intl.en-US/User Guide/Real-time analysis/Analysis grammar/Regular expression functions.md)
    -   [JSON functions](intl.en-US/User Guide/Real-time analysis/Analysis grammar/JSON functions.md)
    -   [Type conversion functions](intl.en-US/User Guide/Real-time analysis/Analysis grammar/Type conversion functions.md)
    -   [IP functions](intl.en-US/User Guide/Real-time analysis/Analysis grammar/IP functions.md)
    -   [Arrays](intl.en-US/User Guide/Real-time analysis/Analysis grammar/Arrays.md)
    -   [Binary string functions](intl.en-US/User Guide/Real-time analysis/Analysis grammar/Binary string functions.md)
    -   [Bit operation](intl.en-US/User Guide/Real-time analysis/Analysis grammar/Bit operation.md)
    -   [Comparison functions and operators](intl.en-US/User Guide/Real-time analysis/Analysis grammar/Comparison functions and operators.md)
    -   [Lambda functions](intl.en-US/User Guide/Real-time analysis/Analysis grammar/Lambda functions.md)
    -   [Logical functions](intl.en-US/User Guide/Real-time analysis/Analysis grammar/Logical functions.md)
    -   [Geospatial functions](intl.en-US/User Guide/Real-time analysis/Analysis grammar/Geospatial functions.md)
    -   [Geo functions](intl.en-US/User Guide/Real-time analysis/Analysis grammar/Geo functions.md)
-   [GROUP BY syntax](intl.en-US/User Guide/Real-time analysis/Analysis grammar/GROUP BY syntax.md)
-   [Window functions](intl.en-US/User Guide/Real-time analysis/Analysis grammar/Window functions.md)
-   [HAVING syntax](intl.en-US/User Guide/Real-time analysis/Analysis grammar/HAVING syntax.md)
-   [ORDER BY syntax](intl.en-US/User Guide/Real-time analysis/Analysis grammar/ORDER BY syntax.md)
-   [LIMIT syntax](intl.en-US/User Guide/Real-time analysis/Analysis grammar/LIMIT syntax.md)
-   [Case when and if branch syntax](intl.en-US/User Guide/Real-time analysis/Analysis grammar/Case when and if branch syntax.md)
-   [UNNEST function](intl.en-US/User Guide/Real-time analysis/Analysis grammar/UNNEST function.md)
-   [Column alias](intl.en-US/User Guide/Real-time analysis/Analysis grammar/Column alias.md)
-   [Nested subquery](intl.en-US/User Guide/Real-time analysis/Analysis grammar/Nested subquery.md)

## Syntax structure {#section_rmd_fkq_tdb .section}

The SQL syntax structure is as follows:

-   The FROM clause and WHERE clause are not required in the SQL statement. By default, FROM indicates to query the data of the current Logstore, and the WHERE condition is search query.
-   The supported clauses include SELECT, GROUP BY, ORDER BY \[ASC,DESC\], LIMIT, and HAVING.

**Note:** By default, only the first 10 results are returned. To return more results, add limit n. For example, `* | select count(1) as c, ip group by ip order by c desc limit 100`.

## Built-in fields {#section_dz4_jkq_tdb .section}

Log Service has built-in fields for statistics. These built-in fields are automatically added when you configure any valid column.

|Field name|Type|Meaning|
|:---------|:---|:------|
| `__time__` |bigint|The log time.|
| `__source__` |varchar|The source IP of the log. This field is source when you query. The underscores \(\_\_\) are added before and after source only in SQL.|
| `__topic__ ` |varchar|The log topic.|

## Limits {#section_cwn_kkq_tdb .section}

1.  The highest concurrency of each project is 15.
2.  A single column varchar has the maximum length of 2048 and is truncated if the length exceeds 2048.
3.  By default, 100 rows of data are returned, and page turning is not supported.  If you want more data to be returned, use [LIMIT syntax](intl.en-US/User Guide/Real-time analysis/Analysis grammar/LIMIT syntax.md).

## Example {#section_gym_lkq_tdb .section}

Count the hourly PV, UV, and maximum delay corresponding to a user request, with the highest delay of 10:

```
*|select date_trunc('hour',from_unixtime(__time__)) as time, 
     count(1) as pv, 
     approx_distinct(userid) as uv,
     max_by(url,latency) as top_latency_url,
     max(latency,10) as top_10_latency
     group by 1
     order by time
```

