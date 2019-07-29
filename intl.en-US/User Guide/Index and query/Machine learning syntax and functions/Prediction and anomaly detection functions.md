# Prediction and anomaly detection functions {#concept_dzq_pkq_kfb .concept}

To detect anomalies, you can use a prediction and anomaly detection function to predict time series curves and identify the Ksigma and quantiles of the errors between a predicted curve and an actual curve.

-   Log Service Machine Learning Introduction \(01\): [Time Series Statistics Modeling](https://yq.aliyun.com/articles/658497)
-   Log Service Machine Learning Introduction \(03\): [Time Series Anomaly Detection Modeling](https://yq.aliyun.com/articles/669164)
-   Log Service Machine Learning Introduction \(05\): [Time Series Prediction](https://yq.aliyun.com/articles/684169)
-   Log Service Machine Learning Best Practices: [Time Series Anomaly Detection and Alert](https://yq.aliyun.com/articles/670718)

## Functions {#section_fd1_qlq_kfb .section}

|Function|Description|
|:-------|:----------|
|`[ts\_predicate\_simple](#)`|Uses default parameters to model time series data and performs simple time series prediction and anomaly detection.|
|`[ts\_predicate\_ar](#)`|Uses an autoregressive \(AR\) model to model time series data and performs simple time series prediction and anomaly detection.|
|`[ts\_predicate\_arma](#)`|Uses an autoregressive moving average \(ARMA\) model to model time series data and performs simple time series prediction and anomaly detection.|
|`[ts\_predicate\_arima](#)`|Uses an autoregressive integrated moving average \(ARIMA\) model to model time series data and performs simple time series prediction and anomaly detection.|
|`[ts\_regression\_predict]()`|Accurately predicts the long-run trend for a single periodic time series with a certain tendency. Scenario: This function can be used to predict metering data, network traffic, financial data, and different business data that follows certain rules.

 |
|`[ts\_anomaly\_filter](#section_4f0_cph_mdq)`|Filters the anomalies detected during time series anomaly detection on multiple curves based on the custom anomaly mode. This function helps you quickly find abnormal instance curves.|

**Note:** The display items for all prediction and anomaly detection functions are the same. For more information about the output result and relevant description, see [the output result and display item description of the ts\_predicate\_simple function](#).

## ts\_predicate\_simple {#section_n3p_qlq_kfb .section}

Function format:

``` {#codeblock_5sz_aeq_sqa}
select ts_predicate_simple(x, y, nPred, isSmooth, samplePeriod, sampleMethod) 
```

The following table describes the parameters.

|Parameter|Description|Value|
|:--------|:----------|:----|
|x|The time sequence. The time points along the x axis are sorted in ascending order.|Each time point is a Unix timestamp. Unit: second.|
|y|The sequence of the numeric values of the property under observation, corresponding to the specified time points.|N/A|
|nPred|The number of points for prediction.|The value is of the Long type. Valid values: \[1, 5 × p\].|
|isSmooth|Specifies whether to filter the raw data. If you do not set this parameter, the default value true is used, indicating that the raw data will be filtered.

 |The value is of the Boolean type. Valid values: -   true: filters the raw data.
-   false: does not filter the raw data.

 Default value: true.|
|samplePeriod|The period during which the current time series data is sampled.|The value is of the Long type. Valid values: \[1, 86399\].|
|sampleMethod|The method for sampling the data in the sampling window.|Valid values: -   avg: samples the average value of the data in the window.
-   max: samples the maximum value of the data in the window.
-   min: samples the minimum value of the data in the window.
-   sum: samples the sum of the data in the window.

 |

Example:

-   The statement for query and analysis is as follows:

``` {#codeblock_x20_8fo_bqc}
* | select ts_predicate_simple(stamp, value, 6, 1, 'avg') from (select __time__ - __time__ % 60 as stamp, avg(v) as value from log GROUP BY stamp order by stamp)
```

-   The following figure shows the output result:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23351/156439279913556_en-US.png)


The following table describes the display items.

|Display item|Description|
|:-----------|:----------|
|Horizontal axis|unixtime|The Unix timestamp of the data. Unit: second.|
|Vertical axis|src|The raw data.|
|predict|The data after filtering.|
|upper|The upper limit of the prediction. By default, the confidence level is 0.85, which cannot be modified.|
|lower|The lower limit of the prediction. By default, the confidence level is 0.85, which cannot be modified.|
|anomaly\_prob|The probability that the point is an anomaly. Valid values: \[0, 1\].|

## ts\_predicate\_ar {#section_zxc_rlq_kfb .section}

Function format:

``` {#codeblock_r5z_iad_s0d}
select ts_predicate_ar(x, y, p, nPred, isSmooth, samplePeriod, sampleMethod) 
```

The following table describes the parameters.

|Parameter|Description|Value|
|:--------|:----------|:----|
|x|The time sequence. The time points along the x axis are sorted in ascending order.|Each time point is a Unix timestamp. Unit: second.|
|y|The sequence of the numeric values of the property under observation, corresponding to the specified time points.|N/A|
|p|The order of the AR model.|The value is of the Long type. Valid values: \[2, 8\].|
|nPred|The number of points for prediction.|The value is of the Long type. Valid values: \[1, 5 × p\].|
|isSmooth|Specifies whether to filter the raw data. If you do not set this parameter, the default value true is used, indicating that the raw data will be filtered.

 |The value is of the Boolean type. Valid values: -   true: filters the raw data.
-   false: does not filter the raw data.

 Default value: true.|
|samplePeriod|The period during which the current time series data is sampled.|The value is of the Long type. Valid values: \[1, 86399\].|
|sampleMethod|The method for sampling the data in the sampling window.|Valid values: -   avg: samples the average value of the data in the window.
-   max: samples the maximum value of the data in the window.
-   min: samples the minimum value of the data in the window.
-   sum: samples the sum of the data in the window.

 |

For example, the statement for query and analysis is as follows:

``` {#codeblock_gah_mor_2y7}
* | select ts_predicate_ar(stamp, value, 3, 4, 1, 'avg') from (select __time__ - __time__ % 60 as stamp, avg(v) as value from log GROUP BY stamp order by stamp)
```

## ts\_predicate\_arma {#section_cg4_rlq_kfb .section}

Function format:

``` {#codeblock_7l5_m0l_1j2}
select ts_predicate_arma(x, y, p, q, nPred, isSmooth, samplePeriod, sampleMethod) 
```

The following table describes the parameters.

|Parameter|Description|Value|
|:--------|:----------|:----|
|x|The time sequence. The time points along the x axis are sorted in ascending order.|Each time point is a Unix timestamp. Unit: second.|
|y|The sequence of the numeric values of the property under observation, corresponding to the specified time points.|N/A|
|p|The order of the AR model.|The value is of the Long type. Valid values: \[2, 100\].|
|q|The order of the ARMA model.|The value is of the Long type. Valid values: \[2, 8\].|
|nPred|The number of points for prediction.|The value is of the Long type. Valid values: \[1, 5 × p\].|
|isSmooth|Specifies whether to filter the raw data. If you do not set this parameter, the default value true is used, indicating that the raw data will be filtered.

 |The value is of the Boolean type. Valid values: -   true: filters the raw data.
-   false: does not filter the raw data.

 Default value: true.|
|samplePeriod|The period during which the current time series data is sampled.|The value is of the Long type. Valid values: \[1, 86399\].|
|sampleMethod|The method for sampling the data in the sampling window.|Valid values: -   avg: samples the average value of the data in the window.
-   max: samples the maximum value of the data in the window.
-   min: samples the minimum value of the data in the window.
-   sum: samples the sum of the data in the window.

 |

For example, the statement for query and analysis is as follows:

``` {#codeblock_pj7_j2z_brh}
* | select ts_predicate_arma(stamp, value, 3, 2, 4, 1, 'avg') from (select __time__ - __time__ % 60 as stamp, avg(v) as value from log GROUP BY stamp order by stamp) 
```

## ts\_predicate\_arima {#section_iby_rlq_kfb .section}

Function format:

``` {#codeblock_dfl_6cl_sac}
select ts_predicate_arima(x, y, p, d, qnPred, isSmooth, samplePeriod, sampleMethod) 
```

The following table describes the parameters.

|Parameter|Description|Value|
|:--------|:----------|:----|
|x|The time sequence. The time points along the x axis are sorted in ascending order.|Each time point is a Unix timestamp. Unit: second.|
|y|The sequence of the numeric values of the property under observation, corresponding to the specified time points.|N/A|
|p|The order of the AR model.|The value is of the Long type. Valid values: \[2, 8\].|
|d|The order of the ARIMA model.|The value is of the Long type. Valid values: \[1, 3\].|
|q|The order of the ARMA model.|The value is of the Long type. Valid values: \[2, 8\].|
|nPred|The number of points for prediction.|The value is of the Long type. Valid values: \[1, 5 × p\].|
|isSmooth|Specifies whether to filter the raw data. If you do not set this parameter, the default value true is used, indicating that the raw data will be filtered.

 |The value is of the Boolean type. Valid values: -   true: filters the raw data.
-   false: does not filter the raw data.

 Default value: true.|
|samplePeriod|The period during which the current time series data is sampled.|The value is of the Long type. Valid values: \[1, 86399\].|
|sampleMethod|The method for sampling the data in the sampling window.|Valid values: -   avg: samples the average value of the data in the window.
-   max: samples the maximum value of the data in the window.
-   min: samples the minimum value of the data in the window.
-   sum: samples the sum of the data in the window.

 |

For example, the statement for query and analysis is as follows:

``` {#codeblock_5pk_quk_nmg}
* | select ts_predicate_arima(stamp, value, 3, 1, 2, 4, 1, 'avg') from (select __time__ - __time__ % 60 as stamp, avg(v) as value from log GROUP BY stamp order by stamp)
```

## ts\_regression\_predict {#section_pwg_ff4_1gb .section}

Function format:

``` {#codeblock_0og_0z2_5ze}
select ts_regression_predict(x, y, nPred, algo\_type,processType, samplePeriod, sampleMethod)
```

The following table describes the parameters.

|Parameter|Description|Value|
|:--------|:----------|:----|
|x|The time sequence. The time points along the x axis are sorted in ascending order.|Each time point is a Unix timestamp. Unit: second.|
|y|The sequence of the numeric values of the property under observation, corresponding to the specified time points.|N/A|
|nPred|The number of points for prediction.|The value is of the Long type. Valid values: \[1, 500\].|
|algo\_type|The algorithm type for prediction.|Valid values: -   origin: uses the Gradient Boosted Regression Tree \(GBRT\) algorithm for prediction.
-   forest: uses the GBRT algorithm for prediction based on the trend components decomposed by Seasonal and Trend decomposition using Loess \(STL\), and then uses the additive model to sum up the decomposed components and obtains the predicted data.
-   linear: uses the Linear Regression algorithm for prediction based on the trend components decomposed by STL, and then uses the additive model to sum up the decomposed components and obtains the predicted data.

 |
|processType|Specifies whether to preprocess the data.| -   0: does not preprocess the data before the data is used for prediction.
-   1: removes abnormal data before the data is used for prediction.

 |
|samplePeriod|The period during which the current time series data is sampled.|The value is of the Long type. Valid values: \[1, 86399\].|
|sampleMethod|The method for sampling the data in the sampling window.|Valid values: -   avg: samples the average value of the data in the window.
-   max: samples the maximum value of the data in the window.
-   min: samples the minimum value of the data in the window.
-   sum: samples the sum of the data in the window.

 |

Example:

-   The statement for query and analysis is as follows:

``` {#codeblock_fuu_qee_gvl}
* and h : nu2h05202.nu8 and m: NET |  select ts_regression_predict(stamp, value, 200, 'origin', 1, 'avg') from (select __time__ - __time__ % 60 as stamp, avg(v) as value from log GROUP  BY  stamp order by stamp)
```

-   The following figure shows the output result:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23351/156439279933813_en-US.png)


The following table describes the display items.

|Display item|Description|
|:-----------|:----------|
|Horizontal axis|unixtime|The Unix timestamp of the data. Unit: second.|
|Vertical axis|src|The raw data.|
|predict|The predicted data.|

## ts\_anomaly\_filter {#section_4f0_cph_mdq .section}

Function format:

``` {#codeblock_l16_xuq_r02}
select ts_anomaly_filter(lineName, ts, ds, preds, probs, nWatch, anomalyType)
```

The following table describes the parameters.

|Parameter|Description|Value|
|---------|-----------|-----|
|lineName|The name of each curve. The value is of the Varchar type.|N/A|
|ts|The time sequence of the curve. The value is an array of the Double type. The time points are sorted in ascending order.|N/A|
|ds|The actual value sequence of the curve. The value is an array of the Double type. The actual values correspond to the time points specified by the ts parameter in one-to-one mode.|N/A|
|preds|The predicted value sequence of the curve. The value is an array of the Double type. The predicted values correspond to the time points specified by the ts parameter in one-to-one mode.|N/A|
|probs|The anomaly detection result sequence of the curve. The value is an array of the Double type. The anomaly detection results correspond to the time points specified by the ts parameter in one-to-one mode.|N/A|
|nWatch|The number of the recently observed actual values on the curve. The value is of the Long type. The value must be smaller than the number of time points on the curve.|N/A|
|anomalyType|The type of anomaly to be filtered. The value is of the Long type.| -   0: all anomalies.
-   1: positive anomalies.
-   -1: negative anomalies.

 |

Example:

-   The statement for query and analysis is as follows:

``` {#codeblock_zoz_z9w_um3}
* | select res.name, res.ts, res.ds, res.preds, res.probs 
     from ( 
         select ts_anomaly_filter(name, ts, ds, preds, probs, cast(5 as bigint), cast(1 as bigint)) as res 
       from (
         select name, res[1] as ts, res[2] as ds, res[3] as preds, res[4] as uppers, res[5] as lowers, res[6] as probs 
     from (
         select name, array_transpose(ts_predicate_ar(stamp, value, 10)) as res 
         from (
           select name, stamp, value from log where name like '%asg-%') group by name)) );
```

-   The output result is as follows:

``` {#codeblock_mii_sed_l1y}
| name                     | ts                                                               | ds          | preds     | probs       |
| ------------------------ | ---------------------------------------------------------------- | ----------- | --------- | ----------- |
| asg-bp1hylzdi2wx7civ0ivk | [1.5513696E9, 1.5513732E9, 1.5513768E9, 1.5513804E9] | [1,2,3,NaN] | [1,2,3,4] | [0,0,1,NaN] |
```


