# Multi-period estimation function {#concept_dws_yw4_kfb .concept}

The multi-period estimation function can estimate time series in different time periods and perform a series of operations, such as Fourier transform, to extract period data.

## ts\_period\_detect {#section_xhs_1x4_kfb .section}

The function estimates time series data on a period basis.

Function format:

```
select ts_period_detect(x, y,minPeriod,maxPeriod,samplePeriod,sampleMethod)
```

The following table describes the parameters.

|Parameter|Description|Value|
|:--------|:----------|:----|
|x|Time column in ascending order|Unixtime timestamp in seconds|
|y|Numeric column corresponding to the data at a specified time point|-|
|minPeriod|Ratio of the minimum length of the pre-estimation period to the total length of the time series|Value range: \(0, 1\]|
|maxPeriod|Ratio of the maximum length of the pre-estimation period to the total length of the time series**Note:** The value of maxPeriod must be greater than that of minPeriod.

|Value range: \(0, 1\]|
|samplePeriod|Period during which the current time series data is sampled|Long type values ranging from 1 to 86399 seconds|
|sampleMethod|Method for sampling the data in the sampling window|Value range:-   avg: average value of the data in the window
-   max: maximum value of the data in the window
-   min: minimum value of the data in the window
-   sum: sum of the data in the window

|

Example:

-   Statement for query and analysis:

    ```
    * | select ts_period_detect(stamp, value, 0.2, 1.0, 1, 'avg') from ( select __time__ - __time__ % 120 as stamp, avg(v) as value from log GROUP BY stamp order by stamp )
    ```

-   Result:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23161/154338427113549_en-US.png)


The following table describes the display items.

|Display item|Description|
|:-----------|:----------|
|period\_id|Array composed of period IDs with an array length of 1. The value 0.0 indicates the original series.|
|time\_series|Timestamp sequence|
|data\_series|Result for each timestamp-   It indicates the original sequence when period\_id is 0.0.
-   It indicates the period estimation result after filtering when period\_id is not 0.0.

|

