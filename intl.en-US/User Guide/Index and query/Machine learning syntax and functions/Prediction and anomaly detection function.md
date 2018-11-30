# Prediction and anomaly detection function {#concept_dzq_pkq_kfb .concept}

The prediction and anomaly detection function detects anomalies by predicting time series curves and identifying Ksigma and quantiles of the errors between a predicted curve and an actual curve.

## Function list {#section_fd1_qlq_kfb .section}

|Function|Description|
|:-------|:----------|
|`[ts\_predicate\_simple](#)`|This function models time series data by using default parameters and performs simple time series prediction and anomaly detection.|
|`[ts\_predicate\_ar](#)`|This function models time series data by using an autoregressive model and performs simple time series prediction and anomaly detection.|
|`[ts\_predicate\_arma](#)`|This function models time series data by using an autoregressive moving model and performs simple time series prediction and anomaly detection.|
|`[ts\_predicate\_arima](#)`|This function models time series data by using an autoregressive moving model with differences and performs simple time series prediction and anomaly detection.|

**Note:** The display items of the prediction and anomaly detection function are consistent. For details about the results and descriptions, see [Example of the ts\_predicate\_simple output result](#).

## ts\_predicate\_simple {#section_n3p_qlq_kfb .section}

Function format:

```
select ts_predicate_simple(x, y, nPred, samplePeriod, sampleMethod) 
```

The following table describes the parameters.

|Parameter|Description|Value|
|:--------|:----------|:----|
|x|Time column in ascending order|Unixtime timestamp in seconds|
|y|Numeric column corresponding to the data at a specified time point|-|
|nPred|Number of points for prediction|Long type values ranging from 1 to 5 x p|
|samplePeriod|Period during which the current time series data is sampled|Long type values ranging from 1 to 86399 seconds|
|sampleMethod|Method for sampling the data in the sampling window|Value range:-   avg: average value of the data in the window
-   max: maximum value of the data in the window
-   min: minimum value of the data in the window
-   sum: sum of the data in the window

|

Example:

-   Statement for query and analysis:

```
* | select ts_predicate_simple(stamp, value, 6, 1, 'avg') from (select __time__ - __time__ % 60 as stamp, avg(v) as value from log GROUP BY stamp order by stamp)
```

-   Result:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23351/154354522413556_en-US.png)


The following table describes the display items.

|Display item|Description|
|:-----------|:----------|
|Horizontal axis|unixtime|Unixtime timestamp in seconds|
|Longitudinal axis|src|Raw data|
|predict|Data after filtering|
|upper|Upper limit of the prediction. By default, the current confidence is 0.85, which is unmodifiable.|
|lower|Lower limit of the prediction. By default, the current confidence is 0.85, which is unmodifiable.|
|anomaly\_prob|Probability that a point is an anomaly point. Its value ranges from 0 to 1.|

## ts\_predicate\_ar {#section_zxc_rlq_kfb .section}

Function format:

```
select ts_predicate_ar(x, y, p, nPred, samplePeriod, sampleMethod) 
```

The following table describes the parameters.

|Parameter|Description|Value|
|:--------|:----------|:----|
|x|Time column in ascending order|Unixtime timestamp in seconds|
|y|Numeric column corresponding to the data at a specified time point|-|
|p|Order of the autoregressive model|Long type values ranging from 2 to 8|
|nPred|Number of points for prediction|Long type values ranging from 1 to 5 x p|
|samplePeriod|Period during which the current time series data is sampled|Long type values ranging from 1 to 86399|
|sampleMethod|Method for sampling the data in the sampling window|Value range:-   avg: average value of the data in the window
-   max: maximum value of the data in the window
-   min: minimum value of the data in the window
-   sum: sum of the data in the window

|

Statement for query and analysis:

```
* | select ts_predicate_ar(stamp, value, 3, 4, 1, 'avg') from (select __time__ - __time__ % 60 as stamp, avg(v) as value from log GROUP BY stamp order by stamp)
```

## ts\_predicate\_arma {#section_cg4_rlq_kfb .section}

Function format:

```
select ts_predicate_arma(x, y, p, q, nPred, samplePeriod, sampleMethod) 
```

The following table describes the parameters.

|Parameter|Description|Value|
|:--------|:----------|:----|
|x|Time column in ascending order|Unixtime timestamp in seconds|
|y|Numeric column corresponding to the data at a specified time point|-|
|p|Order of the autoregressive model|Long type values ranging from 2 to 100|
|q|Order of the moving average model|Long type values ranging from 2 to 8|
|nPred|Number of points for prediction|Long type values ranging from 1 to 5 x p|
|samplePeriod|Period during which the current time series data is sampled|Long type values ranging from 1 to 86399|
|sampleMethod|Method for sampling the data in the sampling window|Value range:-   avg: average value of the data in the window
-   max: maximum value of the data in the window
-   min: minimum value of the data in the window
-   sum: sum of the data in the window

|

Statement for query and analysis:

```
* | select ts_predicate_arma(stamp, value, 3, 2, 4, 1, 'avg') from (select __time__ - __time__ % 60 as stamp, avg(v) as value from log GROUP BY stamp order by stamp) 
```

## ts\_predicate\_arima {#section_iby_rlq_kfb .section}

Function format:

```
select ts_predicate_arima(x, y, p, d, q nPred, samplePeriod, sampleMethod) 
```

The following table describes the parameters.

|Parameter|Description|Value|
|:--------|:----------|:----|
|x|Time column in ascending order|Unixtime timestamp in seconds|
|y|Numeric column corresponding to the data at a specified time point|-|
|p|Order of the autoregressive model|Long type values ranging from 2 to 8|
|d|Order of the difference model|Long type values ranging from 1 to 3|
|q|Order of the moving average model|Long type values ranging from 2 to 8|
|nPred|Number of points for prediction|Long type values ranging from 1 to 5 x p|
|samplePeriod|Period during which the current time series data is sampled|Long type values ranging from 1 to 86399 seconds|
|sampleMethod|Method for sampling the data in the sampling window|Value range:-   avg: average value of the data in the window
-   max: maximum value of the data in the window
-   min: minimum value of the data in the window
-   sum: sum of the data in the window

|

Statement for query and analysis:

```
* | select ts_predicate_arima(stamp, value, 3, 1, 2, 4, 1, 'avg') from (select __time__ - __time__ % 60 as stamp, avg(v) as value from log GROUP BY stamp order by stamp)
```

