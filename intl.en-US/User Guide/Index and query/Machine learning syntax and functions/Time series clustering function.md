# Time series clustering function {#concept_fvn_wtq_kfb .concept}

The time series clustering function is used to automatically cluster input time series data into different curve shapes. After that, the function quickly finds the corresponding clustering centers and curves with shapes that are different from the existing curve shapes.

## Function list {#section_erf_qt3_kfb .section}

|Function|Description|
|:-------|:----------|
|`[ts\_dencity\_cluster](#)`|This function clusters time series data by using the density-based clustering method.|
|`[ts\_hierarchical\_cluster](#)`|This function clusters time series data by using the hierarchical clustering method.|
|`[ts\_similar\_instance](#)`|This function queries curves that are similar to a specified curve.|

## ts\_dencity\_cluster {#section_n3p_qlq_kfb .section}

Function format:

```
select ts_dencity_cluster(x, y, z) 
```

The following table describes the parameters.

|Parameter|Description|Value|
|:--------|:----------|:----|
|x|Time column in ascending order|Unixtime timestamp in seconds|
|y|Numeric column corresponding to the data at a specified time point|-|
|z|Metric name corresponding to the data at a specified time point|String type values, for example, machine01.cpu\_usr|

Example:

-   Statement for query and analysis:

```
* and (h: "machine_01" OR h: "machine_02" OR h : "machine_03") | select ts_dencity_cluster(stamp, metric_value,metric_name ) from ( select __time__ - __time__ % 600 as stamp, avg(v) as metric_value, h as metric_name from log GROUP BY stamp, metric_name order BY metric_name, stamp ) 
```

-   Result:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23359/154338415713558_en-US.png)


The following table describes the display items.

|Display item|Description|
|:-----------|:----------|
|cluster\_id|Clustering type. The value -1 indicates that the clustering cannot be categorized into any clustering centers.|
|rate|Proportion of instances in the clustering|
|time\_series|Timestamp sequence of the clustering center|
|data\_series|Data sequence of the clustering center|
|instance\_names|Set of instances included in the clustering center|
|sim\_instance|Name of an instance in the clustering|

## ts\_hierarchical\_cluster {#section_mst_x5q_kfb .section}

Function format:

```
select ts_hierarchical_cluster(x, y, z) 
```

The following table describes the parameters.

|Parameter|Description|Value|
|:--------|:----------|:----|
|x|Time column in ascending order|Unixtime timestamp in seconds|
|y|Numeric column corresponding to the data at a specified time point|-|
|z|Metric name corresponding to the data at a specified time point|String type values, for example, machine01.cpu\_usr|

Example:

-   Statement for query and analysis:

```
* and (h: "machine_01" OR h: "machine_02" OR h : "machine_03") | select ts_hierarchical_cluster(stamp, metric_value, metric_name) from ( select __time__ - __time__ % 600 as stamp, avg(v) as metric_value, h as metric_name from log GROUP BY stamp, metric_name order BY metric_name, stamp )
```

-   Result:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23359/154338415713559_en-US.png)


The following table describes the display items.

|Display item|Description|
|:-----------|:----------|
|cluster\_id|Clustering type. The value -1 indicates that the clustering cannot be categorized into any clustering centers.|
|rate|Proportion of instances in the clustering|
|time\_series|Timestamp sequence of the clustering center|
|data\_series|Data sequence of the clustering center|
|instance\_names|Set of instances included in the clustering center|
|sim\_instance|Name of an instance in the clustering|

## ts\_similar\_instance {#section_e5v_3vq_kfb .section}

Function format:

```
select ts_similar_instance(x, y, z, instance\_name) 
```

The following table describes the parameters.

|Parameter|Description|Value|
|:--------|:----------|:----|
|x|Time column in ascending order|Unixtime timestamp in seconds|
|y|Numeric column corresponding to the data at a specified time point|-|
|z|Metric name corresponding to the data at a specified time point|String type values, for example, machine01.cpu\_usr|
|instance\_name|Name of a specified metric to be queried|String values in the z set, for example, machine01.cpu\_usr**Note:** The metric must be an existing one.

|

Statement example for query and analysis:

```
* and (h: "machine_01" OR h: "machine_02" OR h : "machine_03") | select ts_similar_instance(stamp, metric_value, metric_name, 'nu4e01524.nu8' ) from ( select __time__ - __time__ % 600 as stamp, avg(v) as metric_value, h as metric_name from log GROUP BY stamp, metric_name order BY metric_name, stamp )
```

The following table describes the display items.

|Display item|Description|
|:-----------|:----------|
|instance\_name|Result list containing metrics that are similar to the specified metric|
|time\_series|Timestamp sequence of the clustering center|
|data\_series|Data sequence of the clustering center|

