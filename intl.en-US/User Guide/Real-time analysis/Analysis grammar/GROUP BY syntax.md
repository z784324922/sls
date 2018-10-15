# GROUP BY syntax {#concept_i3j_5lq_zdb .concept}

GROUP BY supports multiple columns  and indicating the corresponding KEY by using the SELECT column alias.

Example:

```
method:PostLogstoreLogs |select avg(latency),projectName,date_trunc('hour',__time__) as hour group by projectName,hour
```

The alias hour represents the third SELECT column `date_trunc('hour',__time__)`\('hour',\_\_time\_\_\).  This kind of usage is very helpful for some very complicated queries.

GROUP BY supports GROUPING SETS, CUBE, and ROLLUP.

Example:

```
method:PostLogstoreLogs |select avg(latency) group by cube(projectName,logstore)
method:PostLogstoreLogs |select avg(latency)   group by GROUPING SETS ( ( projectName,logstore), (projectName,method))
method:PostLogstoreLogs |select avg(latency) group by rollup(projectName,logstore)
```

## Practical example {#section_hvz_qzk_5cb .section}

**Perform GROUP BY according to time**

Each log has a built-in time column `__time__`. When the statistical function of any column is activated, the statistics will be automatically made for the time column.

Use the `date_trunc` function to align the time column to hour, minute, day, month, and year.  `date_trunc` accepts an aligned unit and a UNIX  time or timestamp type column, such as`__time__`.

-   Count and compute PV every hour or minute

    ```
    * | SELECT count(1) as pv , date_trunc('hour',__time__) as hour group by hour order by hour limit 100
    * | SELECT count(1) as pv , date_trunc('minute',__time__) as minute group by minute order by minute limit 100
    ```

    **Note:** limit limit 100 indicates to obtain 100 rows at most. If the LIMIT statement is not added, at most 10 rows of data can be obtained by default.

-   Make statistics according to flexible time dimension. For example, make the statistics every five minutes. date\_trunc can only make statistics every fixed time period. In this situation, perform GROUP BY according to the mathematical modulus  method.

    ```
    * | SELECT count(1) as pv, __time__ - __time__% 300 as minute5 group by minute5 limit 100
    ```

    The %300 indicates to make the modulus and alignment every five minutes.


**Extract non-agg column in GROUP BY**

In the standard SQL, if you use the GROUP BY syntax, you can only select the original contents of the SELECT GROUP BY columns when you perform SELECT or you are not allowed to obtain the contents of non-GROUP BY columns when you perform aggregation  calculation on any column.

For example, the following syntax is illegal. This is because b is the non-GROUP BY column and multiple rows of b are available when you perform GROUP BY according to a, the system does not know which row of output is to be selected.

```
*|select a, b , count(c) group by a
```

To achieve the preceding aim, use the arbitrary function to output b:

```
*|select a, arbitrary(b), count(c) group by a
```

