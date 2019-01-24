# Maximum value detection function {#concept_iry_xd4_1gb .concept}

The maximum value detection function is used to identify the maximum value of a sequence in a specified window.

Function format:

```
select ts_find_peaks(x, y, winSize, samplePeriod, sampleMethod)
```

The following table describes the function parameters.

|Parameter|Description|Value|
|:--------|:----------|:----|
|x|Time column in ascending order|Unixtime timestamp in seconds|
|y|Numeric column corresponding to the data at a specified time point|-|
|winSize|Minimum length of a window|Long-type values range from 1 to the actual data length. We recommend that you specify one tenth of the actual data length as winSize.|
|samplePeriod|Period during which the current time series data is sampled|Long type values ranging from 1 to 86399|
|sampleMethod|Method for sampling the data in the sampling window|Value range:-   avg: average value of the data in the window
-   max: maximum value of the data in the window
-   min: minimum value of the data in the window
-   sum: sum of the data in the window

|

Examples:

-   Statement for query and analysis:

```
* and h : nu2h05202.nu8 and m: NET |  select ts_find_peaks(stamp, value, 30, 1, 'avg') from (select __time__ - __time__ % 10 as stamp, avg(v) as value from log GROUP  BY  stamp order by stamp)
```

-   Result:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/77488/154831767733817_en-US.png)


The following table describes the display items.

|Display item|Description|
|:-----------|:----------|
|Horizontal axis|unixtime|Data timestamp in seconds, for example, 1537071480|
|Longitudinal axis|src|Data before filtering, for example, 1956092.7647745228|
|peak\_flag|Whether the current point has the maximum value. Value range:-   1.0: The point has the maximum value.
-   0.0: The point does not have the maximum value.

|

