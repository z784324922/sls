# GROUP BY 语法 {#concept_i3j_5lq_zdb .concept}

GROUP BY 支持多列。GROUP BY支持通过SELECT的列的别名来表示对应的KEY。

样例：

```
method:PostLogstoreLogs |select avg(latency),projectName,date_trunc('hour',__time__) as hour   group by projectName,hour
```

别名hour代表第三个SELECT列`date_trunc('hour',__time__)`。这类用法对于一些非常复杂的query非常有帮助。

GROUP BY 支持GROUPING SETS、CUBE、ROLLUP。

样例：

```
method:PostLogstoreLogs |select avg(latency)   group by cube(projectName,logstore)
method:PostLogstoreLogs |select avg(latency)   group by GROUPING SETS ( ( projectName,logstore), (projectName,method))
method:PostLogstoreLogs |select avg(latency)   group by rollup(projectName,logstore)
```

## 实践样例 {#section_hvz_qzk_5cb .section}

**按照时间进行GROUP BY**

每条日志都内置了一个时间列`__time__`，当打开任意一列的统计功能后，会自动给时间列打开统计。

使用`date_trunc`函数，可以把时间列对齐到小时\(hour\)、分钟\(minute\)、天\(day\)、月\(month\)、年\(year\)。`date_trunc`接受一个对齐单位，和一个unix time或者timestamp类型的列，例如`__time__`。

-   按照每小时、每分钟统计计算PV

    ```
    * | SELECT count(1) as pv , date_trunc('hour',__time__) as hour group by hour order by hour limit 100
    * | SELECT count(1) as pv , date_trunc('minute',__time__) as minute group by minute order by minute limit 100
    ```

    **说明：** limit 100表示最多获取100行，如果不加LIMIT语句，默认最多获取10行数据。

-   按照灵活的时间维度进行统计，例如统计每5分钟的，date\_trunc只能在按照一些固定时间间隔统计，这种场景下，我们需要按照数学取模方法进行GROUP BY。

    ```
    * | SELECT count(1) as pv,  __time__ - __time__% 300 as minute5 group by minute5 limit 100
    ```

    上述公式中的`%300`表示按照5分钟进行取模对齐。


**在GROUP BY 中提取非agg列**

在标准SQL中，如果使用了GROUP BY语法，那么在SELECT时，只能选择SELECT GROUP BY的列原始内容，或者对任意列进行聚合计算，不允许获取非GROUP BY列的内容。

例如，以下语法是非法的，因为b是非GROUP BY的列，在按照a进行GROUP BY时，有多行b可供选择，系统不知道该选择哪一行输出。

```
*|select a, b , count(c) group by a
```

为了达到以上目的，可以使用`arbitrary`函数输出b：

```
*|select a, arbitrary(b), count(c) group by a
```

