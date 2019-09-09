# Correlation analysis functions {#concept_827007 .concept}

You can use a correlation analysis function to quickly find the metrics that are correlated with a specified metric or time series data among multiple observed metrics in the system.

## Function list {#section_tj4_xq9_yb8 .section}

|Function|Description|
|--------|-----------|
|`[ts\_association\_analysis](#section_u2h_5ug_g33)`|Quickly finds the metrics that are correlated with a specified metric among multiple observed metrics in the system.|
|`[ts\_similar](#section_fwq_10u_uyv)`|Quickly finds the metrics that are correlated with specified time series data among multiple observed metrics in the system.|

## ts\_association\_analysis {#section_u2h_5ug_g33 .section}

Function format:

``` {#codeblock_0jn_1r2_g94}
select ts_association_analysis(stamp, params, names, indexName, threshold)
```

The following table describes the parameters.

|Parameter|Description|Value|
|---------|-----------|-----|
|stamp|The Unix timestamp.|Long type.|
|params|The dimensions of the metrics to be analyzed.|Array of the double type. For example, Latency, QPS, and NetFlow.|
|names|The names of the metrics to be analyzed.|Array of the varchar type. For example, Latency, QPS, and NetFlow.|
|indexName|The name of the target metric.|Varchar type, for example, Latency.|
|threshold|The threshold of correlation between the metrics to be analyzed and the target metric.|Double type. Valid values: \[0, 1\].|

Result:

-   name: the name of the analyzed metric.
-   score: the value of correlation between the analyzed metric and the target metric. Valid values: \[0, 1\].

Sample code:

``` {#codeblock_5iw_m7s_ofl}
* | select ts_association_analysis(
              time, 
              array[inflow, outflow, latency, status], 
              array['inflow', 'outflow', 'latency', 'status'], 
              'latency', 
              0.1) from log;
```

Sample result:

``` {#codeblock_e4w_7zz_uet}
| results               |
| --------------------- |
| ['latency', '1.0']    |
| ['outflow', '0.6265'] |
| ['status', '0.2270']  |
```

## ts\_similar {#section_fwq_10u_uyv .section}

Function format 1:

``` {#codeblock_17f_jvl_n60}
select ts_similar(stamp, value, ts, ds)
select ts_similar(stamp, value, ts, ds, metricType)
```

The following table describes the parameters.

|Parameter|Description|Value|
|---------|-----------|-----|
|stamp|The Unix timestamp.|Long type.|
|value|The value of the specified metric.|Double type.|
|ts|The sequence of time for the specified curve.|Array of the double type.|
|ds|The sequence of numeric data for the specified curve.|Array of the double type.|
|metricType|The type of correlation between the measured curves.|Varchar type. Valid values: SHAPE, RMSE, PEARSON, SPEARMAN, R2, and KENDALL

 |

Function format 2:

``` {#codeblock_8ou_nxf_v7k}
select ts_similar(stamp, value, startStamp, endStamp, step, ds)
select ts_similar(stamp, value, startStamp, endStamp, step, ds, metricType )
```

The following table describes the parameters.

|Parameter|Description|Value|
|---------|-----------|-----|
|stamp|The Unix timestamp.|Long type.|
|value|The value of the specified metric.|Double type.|
|startStamp|The start timestamp of the specified curve.|Long type.|
|endStamp|The end timestamp of the specified curve.|Long type.|
|step|The time interval between two adjacent points in the sequence of time.|Long type.|
|ds|The sequence of numeric data for the specified curve.|Array of the double type.|
|metricType|The type of correlation between the measured curves.|Varchar type. Valid values: SHAPE, RMSE, PEARSON, SPEARMAN, R2, and KENDALL

 |

Result:

score: the value of correlation between the analyzed metric and the target metric. Valid values: \[-1, 1\].

Sample code:

``` {#codeblock_bbn_6au_azz}
* | select vhost, metric, ts_similar(time, value, 1560911040, 1560911065, 5, array[5.1,4.0,3.3,5.6,4.0,7.2], 'PEARSON') from log  group by vhost, metric;
```

Sample result:

``` {#codeblock_r3v_nix_bn7}
| vhost  | metric          | score                |
| ------ | --------------- | -------------------- |
| vhost1 | redolog         | -0.3519082537204182  |
| vhost1 | kv_qps          | -0.15922168009772697 |
| vhost1 | file_meta_write | NaN                  |
```

