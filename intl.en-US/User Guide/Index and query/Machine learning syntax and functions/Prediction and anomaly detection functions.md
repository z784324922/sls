# Prediction and anomaly detection functions {#concept_dzq_pkq_kfb .concept}

To detect anomalies, you can use a prediction and anomaly detection function to predict time series curves and identify the Ksigma and quantiles of the errors between a predicted curve and an actual curve.

## Function list {#section_fd1_qlq_kfb .section}

|Function|Description|
|:-------|:----------|
|`[ts\_predicate\_simple](#)`|Uses default parameters to model time series data and performs simple time series prediction and anomaly detection.|
|`[ts\_predicate\_ar](#)`|Uses an autoregressive model to model time series data and performs simple time series prediction and anomaly detection.|
|`[ts\_predicate\_arma](#)`|Uses an autoregressive moving average model to model time series data and performs simple time series prediction and anomaly detection.|
|`[ts\_predicate\_arima](#)`|Uses an autoregressive moving average model with differences to model time series data and performs simple time series prediction and anomaly detection.|
|`[ts\_regression\_predict]()`|Accurately predicts the long-run trend for a single periodic time series. Scenario: This function can be used to predict metering data, network traffic, financial data, and different business data that follows certain rules.

 |

**Note:** The display items are the same for all prediction and anomaly detection functions. For more information about the output result and relevant description, see [the output result and display item description of the ts\_predicate\_simple function](#).

## ts\_predicate\_simple {#section_n3p_qlq_kfb .section}

Function format:

``` {#codeblock_mv6_x52_5wi}
select ts_predicate_simple(x, y, nPred, isSmooth, samplePeriod, sampleMethod) 
```

The following table describes the parameters.

|Parameter|Description|Value|
|:--------|:----------|:----|
|x|The sequence of time in ascending order.|Unix timestamp. Unit: seconds.|
|y|The sequence of numeric data corresponding to each specified time point.|N/A|
|nPred|The number of points for prediction.|Long type. Valid values: \[1, 5 × p\].|
|isSmooth|Specifies whether to filter the raw data. If you do not set this parameter, the raw data is filtered by default.

 |Boolean type. Valid values: -   true: filters the raw data.
-   false: does not filter the raw data.

 Default value: true.|
|samplePeriod|The period during which the current time series data is sampled.|Long type. Valid values: \[1, 86399\].|
|sampleMethod|The method for sampling the data in the sampling window.|Valid values: -   avg: samples the average value of the data in the window.
-   max: samples the maximum value of the data in the window.
-   min: samples the minimum value of the data in the window.
-   sum: samples the sum of the data in the window.

 |

Example:

-   The statement for query and analysis is as follows:

``` {#codeblock_7ps_5ge_4ve}
* | select ts_predicate_simple(stamp, value, 6, 1, 'avg') from (select __time__ - __time__ % 60 as stamp, avg(v) as value from log GROUP BY stamp order by stamp)
```

-   The following figure shows the output result.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23351/156204877113556_en-US.png)


The following table describes the display items.

|Display item|Description|
|:-----------|:----------|
|Horizontal axis|unixtime|The Unix timestamp of the data. Unit: seconds.|
|Vertical axis|src|The raw data.|
|predict|The data after filtering.|
|upper|The upper limit of the prediction. The current confidence is 0.85 by default, which cannot be modified.|
|lower|The lower limit of the prediction. The current confidence is 0.85 by default, which cannot be modified.|
|anomaly\_prob|The probability that a point is an anomaly. Valid values: \[0, 1\].|

## ts\_predicate\_ar {#section_zxc_rlq_kfb .section}

Function format:

``` {#codeblock_fm2_vb3_nhk}
select ts_predicate_ar(x, y, p, nPred, isSmooth, samplePeriod, sampleMethod) 
```

The following table describes the parameters.

|Parameter|Description|Value|
|:--------|:----------|:----|
|x|The sequence of time in ascending order.|Unix timestamp. Unit: seconds.|
|y|The sequence of numeric data corresponding to each specified time point.|N/A|
|p|The order of the autoregressive model.|Long type. Valid values: \[2, 8\].|
|nPred|The number of points for prediction.|Long type. Valid values: \[1, 5 × p\].|
|isSmooth|Specifies whether to filter the raw data. If you do not set this parameter, the raw data is filtered by default.

 |Boolean type. Valid values: -   true: filters the raw data.
-   false: does not filter the raw data.

 Default value: true.|
|samplePeriod|The period during which the current time series data is sampled.|Long type. Valid values: \[1, 86399\].|
|sampleMethod|The method for sampling the data in the sampling window.|Valid values: -   avg: samples the average value of the data in the window.
-   max: samples the maximum value of the data in the window.
-   min: samples the minimum value of the data in the window.
-   sum: samples the sum of the data in the window.

 |

For example, the statement for query and analysis is as follows:

``` {#codeblock_tv4_hop_unc}
* | select ts_predicate_ar(stamp, value, 3, 4, 1, 'avg') from (select __time__ - __time__ % 60 as stamp, avg(v) as value from log GROUP BY stamp order by stamp)
```

## ts\_predicate\_arma {#section_cg4_rlq_kfb .section}

Function format:

``` {#codeblock_64i_f6h_q6b}
select ts_predicate_arma(x, y, p, q, nPred, isSmooth, samplePeriod, sampleMethod) 
```

The following table describes the parameters.

|Parameter|Description|Value|
|:--------|:----------|:----|
|x|The sequence of time in ascending order.|Unix timestamp. Unit: seconds.|
|y|The sequence of numeric data corresponding to each specified time point.|N/A|
|p|The order of the autoregressive model.|Long type. Valid values: \[2, 100\].|
|q|The order of the moving average model.|Long type. Valid values: \[2, 8\].|
|nPred|The number of points for prediction.|Long type. Valid values: \[1, 5 × p\].|
|isSmooth|Specifies whether to filter the raw data. If you do not set this parameter, the raw data is filtered by default.

 |Boolean type. Valid values: -   true: filters the raw data.
-   false: does not filter the raw data.

 Default value: true.|
|samplePeriod|The period during which the current time series data is sampled.|Long type. Valid values: \[1, 86399\].|
|sampleMethod|The method for sampling the data in the sampling window.|Valid values: -   avg: samples the average value of the data in the window.
-   max: samples the maximum value of the data in the window.
-   min: samples the minimum value of the data in the window.
-   sum: samples the sum of the data in the window.

 |

For example, the statement for query and analysis is as follows:

``` {#codeblock_l55_yik_trk}
* | select ts_predicate_arma(stamp, value, 3, 2, 4, 1, 'avg') from (select __time__ - __time__ % 60 as stamp, avg(v) as value from log GROUP BY stamp order by stamp) 
```

## ts\_predicate\_arima {#section_iby_rlq_kfb .section}

Function format:

``` {#codeblock_u0q_ehe_8l5}
select ts_predicate_arima(x, y, p, d, qnPred, isSmooth, samplePeriod, sampleMethod) 
```

The following table describes the parameters.

|Parameter|Description|Value|
|:--------|:----------|:----|
|x|The sequence of time in ascending order.|Unix timestamp. Unit: seconds.|
|y|The sequence of numeric data corresponding to each specified time point.|N/A|
|p|The order of the autoregressive model.|Long type. Valid values: \[2, 8\].|
|d|The order of the difference model.|Long type. Valid values: \[1, 3\].|
|q|The order of the moving average model.|Long type. Valid values: \[2, 8\].|
|nPred|The number of points for prediction.|Long type. Valid values: \[1, 5 × p\].|
|isSmooth|Specifies whether to filter the raw data. If you do not set this parameter, the raw data is filtered by default.

 |Boolean type. Valid values: -   true: filters the raw data.
-   false: does not filter the raw data.

 Default value: true.|
|samplePeriod|The period during which the current time series data is sampled.|Long type. Valid values: \[1, 86399\].|
|sampleMethod|The method for sampling the data in the sampling window.|Valid values: -   avg: samples the average value of the data in the window.
-   max: samples the maximum value of the data in the window.
-   min: samples the minimum value of the data in the window.
-   sum: samples the sum of the data in the window.

 |

For example, the statement for query and analysis is as follows:

``` {#codeblock_rd2_1te_2qh}
* | select ts_predicate_arima(stamp, value, 3, 1, 2, 4, 1, 'avg') from (select __time__ - __time__ % 60 as stamp, avg(v) as value from log GROUP BY stamp order by stamp)
```

## ts\_regression\_predict {#section_pwg_ff4_1gb .section}

Function format:

``` {#codeblock_eox_9z7_7e8}
select ts_regression_predict(x, y, nPred, algo\_type,processType, samplePeriod, sampleMethod)
```

The following table describes the parameters.

|Parameter|Description|Value|
|:--------|:----------|:----|
|x|The sequence of time in ascending order.|Unix timestamp. Unit: seconds.|
|y|The sequence of numeric data corresponding to each specified time point.|N/A|
|nPred|The number of points for prediction.|Long type. Valid values: \[1, 500\].|
|algo\_type|The algorithm type for prediction.|Valid values: -   origin: uses the Gradient Boosted Regression Tree \(GBRT\) algorithm for prediction.
-   forest: uses the GBRT algorithm for prediction based on the trend component decomposed by Seasonal and Trend decomposition using Loess \(STL\), and then uses the additive model to sum up the decomposed components and obtains the predicted data.
-   linear: uses the Linear Regression algorithm for prediction based on the trend component decomposed by STL, and then uses the additive model to sum up the decomposed components and obtains the predicted data.

 |
|processType|The preprocessing of the data.| -   0: does not process the data before the data is used for prediction.
-   1: deletes abnormal data before the data is used for prediction.

 |
|samplePeriod|The period during which the current time series data is sampled.|Long type. Valid values: \[1, 86399\].|
|sampleMethod|The method for sampling the data in the sampling window.|Valid values: -   avg: samples the average value of the data in the window.
-   max: samples the maximum value of the data in the window.
-   min: samples the minimum value of the data in the window.
-   sum: samples the sum of the data in the window.

 |

Example:

-   The statement for query and analysis is as follows:

``` {#codeblock_a42_0x7_k6o}
* and h : nu2h05202.nu8 and m: NET |  select ts_regression_predict(stamp, value, 200, 'origin', 1, 'avg') from (select __time__ - __time__ % 60 as stamp, avg(v) as value from log GROUP  BY  stamp order by stamp)
```

-   The following figure shows the output result.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23351/156204877233813_en-US.png)


The following table describes the display items.

|Display item|Description|
|:-----------|:----------|
|Horizontal axis|unixtime|The Unix timestamp of the data. Unit: seconds.|
|Vertical axis|src|The raw data.|
|predict|The predicted data.|

