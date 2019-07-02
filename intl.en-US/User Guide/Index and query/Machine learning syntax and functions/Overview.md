# Overview {#concept_cgz_n4h_kfb .concept}

Log Service provides a machine learning feature that supports multiple algorithms and calling methods. During log query and analysis, you can use SELECT statements and machine learning functions to call machine learning algorithms and analyze the characteristics of a field or fields within a period of time.

In particular, Log Service offers diversified time series analysis algorithms to help you quickly solve problems related to time series prediction, time series anomaly detection, time series decomposition, and multi-time series clustering. In addition, the algorithms are compatible with standard SQL functions. This greatly simplifies the use of the algorithms and improves troubleshooting efficiency.

## Features {#section_qr4_q4h_kfb .section}

-   Supports various smooth operations on single-time series sequences.
-   Supports algorithms related to the prediction, anomaly detection, change point detection, inflexion point detection, and time series forecasting of single-time series sequences.
-   Supports decomposition operations on single-time series sequences.
-   Supports various clustering algorithms of multi-time series sequences.
-   Supports multi-field pattern mining \(based on the sequence of numeric data or text\).

## Limits {#section_xxk_glj_kfb .section}

-   The input time series data must be sampled from the same interval.
-   The input time series data cannot contain data repeatedly sampled from the same time point.

|Item|Limit|
|:---|:----|
|Valid capacity of time-series data processing|Data collected from a maximum of 150,000 consecutive time pointsIf the limit is exceeded, you need to aggregate the data or reduce the sampling amount.

|
|Clustering capacity of the density-based clustering algorithm|A maximum of 5,000 time series curves, each of which cannot contain more than 1,440 time points|
|Clustering capacity of the hierarchical clustering algorithm|A maximum of 2,000 time series curves, each of which cannot contain more than 1,440 time points|

## Machine learning functions {#section_mgs_3ph_kfb .section}

| |Category|Function|Description|
|:-|:-------|:-------|:----------|
|Time series|[Smooth function](reseller.en-US/User Guide/Index and query/Machine learning syntax and functions/Smooth function.md)|ts\_smooth\_simple|Uses the Holt Winters algorithm to smooth time series data.|
|ts\_smooth\_fir|Uses the finite impulse response \(FIR\) filter to smooth time series data.|
|ts\_smooth\_iir|Uses the infinite impulse response \(IIR\) filter to smooth time series data.|
|[Time series forecasting function](reseller.en-US/User Guide/Index and query/Machine learning syntax and functions/Multi-period estimation function.md)|ts\_period\_detect|Forecasts time series data by period.|
|[Change point detection function](reseller.en-US/User Guide/Index and query/Machine learning syntax and functions/Change point detection function.md)|ts\_cp\_detect|Finds intervals with different statistical characteristics from time series data. The interval endpoints are change points.|
|ts\_breakout\_detect|Finds the time point when statistics steeply increase or decrease from time series data.|
|[Maximum value detection function](reseller.en-US/User Guide/Index and query/Machine learning syntax and functions/Maximum value detection function.md)|ts\_find\_peaks|Finds the locally maximum value of time series data in a specified window.|
|[Prediction and anomaly detection function](reseller.en-US/User Guide/Index and query/Machine learning syntax and functions/Prediction and anomaly detection functions.md)|ts\_predicate\_simple|Uses default parameters to model time series data and performs simple time series prediction and anomaly detection.|
|ts\_predicate\_ar|Uses an autoregressive model to model time series data and performs simple time series prediction and anomaly detection.|
|ts\_predicate\_arma|Uses an autoregressive moving model to model time series data and performs simple time series prediction and anomaly detection.|
|ts\_predicate\_arima|Uses an autoregressive moving model with differences to model time series data and performs simple time series prediction and anomaly detection.|
|ts\_regression\_predict|Accurately predicts the long-run trend for a single periodic time series.|
|[Time series decomposition function](reseller.en-US/User Guide/Index and query/Machine learning syntax and functions/Sequence decomposition function.md)|ts\_decompose|Uses the Seasonal and Trend decomposition using Loess \(STL\) algorithm to decompose time series data.|
|[Time series clustering function](reseller.en-US/User Guide/Index and query/Machine learning syntax and functions/Time series clustering functions.md)|ts\_density\_cluster|Uses a density-based clustering method to cluster multiple pieces of time series data.|
|ts\_hierarchical\_cluster|Uses a hierarchical clustering method to cluster multiple pieces of time series data.|
|ts\_similar\_instance|Queries curves that are similar to a specified curve.|
|Pattern mining|[Frequent pattern statistics](reseller.en-US/User Guide/Index and query/Machine learning syntax and functions/Frequent pattern statistical function.md)|pattern\_stat|Mines representative combinations of attributes among the given multi-attribute field samples to obtain the frequent pattern in statistical patterns.|
|[Differential pattern statistics](reseller.en-US/User Guide/Index and query/Machine learning syntax and functions/Differential pattern statistical function.md)|pattern\_diff|Finds the pattern that causes differences between two collections under specified conditions.|

