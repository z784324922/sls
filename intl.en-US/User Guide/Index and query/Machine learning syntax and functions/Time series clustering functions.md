# Time series clustering functions {#concept_fvn_wtq_kfb .concept}

You can use a time series clustering function to cluster multiple pieces of time series data and obtain different curve shapes. Then, you can quickly find the corresponding cluster center and curves with shapes that are different from the curve shapes in the cluster.

## Function list {#section_erf_qt3_kfb .section}

|Function|Description|
|:-------|:----------|
|`[ts\_density\_cluster](#)`|Uses a density-based clustering method to cluster multiple pieces of time series data.|
|`[ts\_hierarchical\_cluster](#)`|Uses a hierarchical clustering method to cluster multiple pieces of time series data.|
|`[ts\_similar\_instance](#)`|Queries curves that are similar to a specified curve.|

## ts\_density\_cluster {#section_n3p_qlq_kfb .section}

Function format:

``` {#codeblock_1w7_nfh_279}
select ts_density_cluster(x, y, z) 
```

The following table describes the parameters.

|Parameter|Description|Value|
|:--------|:----------|:----|
|x|The sequence of time in ascending order.|Unix timestamp. Unit: seconds.|
|y|The sequence of numeric data corresponding to each specified time point.|N/A|
|z|The metric name corresponding to the data at each specified time point.|String type, for example, machine01.cpu\_usr.|

Example:

-   The statement for query and analysis is as follows:

``` {#codeblock_8qf_g2y_snw}
* and (h: "machine_01" OR h: "machine_02" OR h : "machine_03") | select ts_density_cluster(stamp, metric_value, metric_name) from (select __time__ - __time__ % 600 as stamp, avg(v) as metric_value, h as metric_name from log GROUP BY stamp, metric_name order BY metric_name, stamp) 
```

-   The following figure shows the output result.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23359/156204890113558_en-US.png)


The following table describes the display items.

|Display item|Description|
|:-----------|:----------|
|cluster\_id|The category of the cluster. The value -1 indicates that the cluster is not categorized in any cluster centers.|
|rate|The proportion of instances in the cluster.|
|time\_series|The timestamp sequence of the cluster center.|
|data\_series|The data sequence of the cluster center.|
|instance\_names|The collection of instances included in the cluster center.|
|sim\_instance|The name of an instance in the cluster.|

## ts\_hierarchical\_cluster {#section_mst_x5q_kfb .section}

Function format:

``` {#codeblock_5xv_r2v_0tx}
select ts_hierarchical_cluster(x, y, z) 
```

The following table describes the parameters.

|Parameter|Description|Value|
|:--------|:----------|:----|
|x|The sequence of time in ascending order.|Unix timestamp. Unit: seconds.|
|y|The sequence of numeric data corresponding to each specified time point.|N/A|
|z|The metric name corresponding to the data at each specified time point.|String type, for example, machine01.cpu\_usr.|

Example:

-   The statement for query and analysis is as follows:

``` {#codeblock_wx3_x60_gic}
* and (h: "machine_01" OR h: "machine_02" OR h : "machine_03") | select ts_hierarchical_cluster(stamp, metric_value, metric_name) from (select __time__ - __time__ % 600 as stamp, avg(v) as metric_value, h as metric_name from log GROUP BY stamp, metric_name order BY metric_name, stamp)
```

-   The following figure shows the output result.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23359/156204890113559_en-US.png)


The following table describes the display items.

|Display item|Description|
|:-----------|:----------|
|cluster\_id|The category of the cluster. The value -1 indicates that the cluster is not categorized in any cluster centers.|
|rate|The proportion of instances in the cluster.|
|time\_series|The timestamp sequence of the cluster center.|
|data\_series|The data sequence of the cluster center.|
|instance\_names|The collection of instances included in the cluster center.|
|sim\_instance|The name of an instance in the cluster.|

## ts\_similar\_instance {#section_e5v_3vq_kfb .section}

Function format:

``` {#codeblock_yg1_5ls_9oy}
select ts_similar_instance(x, y, z, instance\_name, topK, metricType) 
```

The following table describes the parameters.

|Parameter|Description|Value|
|:--------|:----------|:----|
|x|The sequence of time in ascending order.|Unix timestamp. Unit: seconds.|
|y|The sequence of numeric data corresponding to each specified time point.|N/A|
|z|The metric name corresponding to the data at each specified time point.|String type, for example, machine01.cpu\_usr.|
|instance\_name|The name of the specified metric to be queried in the z collection.|String type, for example, machine01.cpu\_usr. **Note:** The metric must be an existing one.

 |
|topK|The curves similar to a given curve. A maximum of K curves are returned.|N/A|
|metricType|The metric used to measure the similarity between time series curves.|\{'shape', 'manhattan', 'euclidean'\}|

For example, the statement for query and analysis is as follows:

``` {#codeblock_bdo_ter_9tb}
* and m: NET and m: Tcp and (h: "nu4e01524.nu8" OR  h: "nu2i10267.nu8" OR  h : "nu4q10466.nu8") | select ts_similar_instance(stamp, metric_value, metric_name, 'nu4e01524.nu8') from (select __time__ - __time__ % 600 as stamp, sum(v) as metric_value, h as metric_name from log GROUP BY stamp, metric_name order BY  metric_name, stamp)
```

The following table describes the display items.

|Display item|Description|
|:-----------|:----------|
|instance\_name|The list of metrics that are similar to the specified metric.|
|time\_series|The timestamp sequence of the cluster center.|
|data\_series|The data sequence of the cluster center.|

