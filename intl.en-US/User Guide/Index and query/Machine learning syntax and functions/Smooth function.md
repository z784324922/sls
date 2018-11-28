# Smooth function {#concept_ofg_1sh_kfb .concept}

The smooth function is used to smooth and filter input time series curves. Filtering is the first step to discover the shape of a time series curve.

## Function list {#section_erf_qt3_kfb .section}

|Function|Description|
|:-------|:----------|
|`[ts\_smooth\_simple](#)`|This function is the default smooth function, which uses the Holt Winters algorithm to smooth time series data.|
|`[ts\_smooth\_fir](#)`|This function uses the FIR filter to smooth time series data.|
|`[ts\_smooth\_iir](#)`|This function uses the IIR filter to smooth time series data.|

## ts\_smooth\_simple {#section_f11_q53_kfb .section}

Function format:

```
select ts_smooth_simple(x, y)
```

The following table describes the parameters.

|Parameter|Description|Value|
|:--------|:----------|:----|
|x|Time column in ascending order|Unixtime timestamp in seconds|
|y|Numeric column corresponding to the data at a specified time point|--|

Example:

-   Statement for query and analysis:

    ```
    * | select ts_smooth_simple(stamp, value) from ( select __time__ - __time__ % 120 as stamp, avg(v) as value from log GROUP BY stamp order by stamp )
    ```

-   Result:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/22661/154338431013521_en-US.png)


The following table describes the display items.

|Display item|Description|
|:-----------|:----------|
|Horizontal axis|unixtime|Data timestamp in seconds|
|Longitudinal axis|src|Data before filtering|
|filter|Data after filtering|

## ts\_smooth\_fir {#section_ksl_v53_kfb .section}

Function format:

-   When filter parameters are undetermined, you can use the parameters in the built-in window to filter time series data:

    ```
    select ts_smooth_fir(x, y,winType,winSize,samplePeriod,sampleMethod)
    ```

-   When filter parameters are specified, you can set them as needed:

    ```
    select ts_smooth_fir(x, y,array\[\],samplePeriod,sampleMethod)
    ```


The following table describes the parameters.

|Parameter|Description|Value|
|:--------|:----------|:----|
|x|Time column in ascending order|Unixtime timestamp in seconds|
|y|Numeric column corresponding to the data at a specified time point|-|
|winType|Filter window type|Value range:-   rectangle: rectangle window
-   hanning: hanning window
-   hamming: hamming window
-   blackman: blackman window

**Note:** We recommend that you set this parameter to rectangle for better display.

|
|winSize|Length of the filter window|Long type values ranging form 2 to 15|
|array\[\]|Specific FIR filter parameters|Values in array format, sum of Array is 1.0. For example, \[0.2, 0.4, 0.3, 0.1\].|
|samplePeriod|Period during which the current time series data is sampled|Long type values ranging from 1 to 86399 seconds|
|sampleMethod|Method for sampling the data in the sampling window|Value range:-   avg: average value of the data in the window
-   max: maximum value of the data in the window
-   min: minimum value of the data in the window
-   sum: sum of the data in the window

|

Example:

-   Statement for query and analysis:

    ```
    * | select ts_smooth_fir(stamp, value, 'rectangle', 4, 1, 'avg') from ( select __time__ - __time__ % 120 as stamp, avg(v) as value from log GROUP BY stamp order by stamp ) 
    ```

    Result:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/22661/154338431013522_en-US.png)

-   Statement for query and analysis:

    ```
    * | select ts_smooth_fir(stamp, value, array[0.2, 0.4, 0.3, 0.1], 1, 'avg') from ( select __time__ - __time__ % 120 as stamp, avg(v) as value from log GROUP BY stamp order by stamp ) 
    ```

    Result:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/22661/154338431013523_en-US.png)


The following table describes the display items.

|Display item|Description|
|:-----------|:----------|
|Horizontal axis|unixtime|Unixtime timestamp in seconds|
|Longitudinal axis|src|Data before filtering|
|filter|Data after filtering|

## ts\_smooth\_iir {#section_h42_w53_kfb .section}

Function format:

```
select ts_smooth_iir(x, y, array\[\], array\[\], samplePeriod, sampleMethod) 
```

The following table describes the parameters.

|Parameter|Description|Value|
|:--------|:----------|:----|
|x|Time column in ascending order|Unixtime timestamp in seconds|
|y|Numeric column corresponding to the data at a specified time point|-|
|array\[\]|Specific parameter of xi in the IIR filter algorithm|Array values with a length ranging from 2 to 15. Sum of Array is 1.0. For example, array\[0.2, 0.4, 0.3, 0.1\]|
|array\[\]|Specific parameter of yi−1 in the IIR filter algorithm|Array values with a length ranging from 2 to 15. Sum of Array is 1.0. For example, array\[0.2, 0.4, 0.3, 0.1\]|
|samplePeriod|Period during which the current time series data is sampled|Long type values ranging from 1 to 86399 seconds|
|sampleMethod|Method for sampling the data in the sampling window|Value range:-   avg: average value of the data in the sampling window
-   max: maximum value of the data in the sampling window
-   min: minimum value of the data in the sampling window
-   sum: sum of the data in the sampling window

|

Example:

-   Statement for query and analysis:

```
* | select ts_smooth_iir(stamp, value, array[0.2, 0.4, 0.3, 0.1], array[0.4, 0.3, 0.3], 1, 'avg') from ( select __time__ - __time__ % 120 as stamp, avg(v) as value from log GROUP BY stamp order by stamp )
```

-   Result:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/22661/154338431013524_en-US.png)


The following table describes the display items.

|Display item|Description|
|:-----------|:----------|
|Horizontal axis|unixtime|Unixtime timestamp in seconds|
|Longitudinal axis|src|Data before filtering|
|filter|Data after filtering|

