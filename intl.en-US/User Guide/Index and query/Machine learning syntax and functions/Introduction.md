# Introduction {#concept_cgz_n4h_kfb .concept}

Log Service provides a machine learning feature that supports multiple algorithms and calling methods. During log query and analysis, you can use SELECT statements and machine learning functions to call machine learning algorithms to analyze the characteristics of a field or fields within a period of time.

In particular, Log Service offers diversified time series analysis algorithms to help you quickly solve problems related to time series prediction, time series anomaly detection, sequence decomposition, and multi-time series clustering. Additionally, the algorithms are compatible with standard SQL interfaces, which greatly simplifies use of the algorithms and improves troubleshooting efficiency.

## Features {#section_qr4_q4h_kfb .section}

-   Various smooth operations on single-time series sequences are supported.
-   Algorithms related to prediction, anomaly detection, change point detection, inflexion point detection, and multi-period estimation of single-time series sequences are supported.
-   Decomposition operations on single-time series sequences are supported.
-   Various clustering algorithms of multi-time series sequences are supported.
-   Multi-field pattern mining \(based on numeric or text columns\) are supported.

## Limits {#section_xxk_glj_kfb .section}

-   The input time series data must be sampled from the same interval.
-   The input time series data cannot contain data repeatedly sampled from the same time point.

|Item|Limit|
|:---|:----|
|Valid capacity of time-series data processing|Data collected from a maximum of 150,000 consecutive time pointsIf the limit is exceeded, you need to aggregate the data or reduce the sample data amount.

|
|Clustering capacity of the density-based clustering algorithm|A maximum of 5,000 time series curves, each of which cannot contain more than 1,440 time points|
|Clustering capacity of the hierarchical clustering algorithm|A maximum of 2,000 time series curves, each of which cannot contain more than 1,440 time points|

## Functions {#section_mgs_3ph_kfb .section}

| |Category|Function|Description|
|:-|:-------|:-------|:----------|
|Time series|[Smooth function](reseller.en-US/User Guide/Index and query/Machine learning syntax and functions/Smooth function.md)|ts\_smooth\_simple|This function uses the Holt Winters algorithm to smooth time series data.|
|ts\_smooth\_fir|This function uses the FIR filter to smooth time series data.|
|ts\_smooth\_iir|This function uses the IIR filter to smooth time series data.|
|[Multi-period estimation function](reseller.en-US/User Guide/Index and query/Machine learning syntax and functions/Multi-period estimation function.md)|ts\_period\_detect|This function estimates period information in a time series.|
|[Change point detection function](reseller.en-US/User Guide/Index and query/Machine learning syntax and functions/Change point detection function.md)|ts\_cp\_detect|This function finds intervals with different statistical characteristics within a time series. The interval endpoints are change points.|
|ts\_breakout\_detect|This function finds the time point when statistics abruptly increase or decrease within a time series.|
|[Prediction and anomaly detection function](reseller.en-US/User Guide/Index and query/Machine learning syntax and functions/Prediction and anomaly detection function.md)|ts\_predicate\_simple|This function models time series data by using default parameters and performs simple time series prediction and anomaly detection.|
|ts\_predicate\_ar|This function models time series data by using an autoregressive model and performs simple time series prediction and anomaly detection.|
|ts\_predicate\_arma|This function models time series data by using the autoregressive moving average model and performs simple time series prediction and anomaly detection.|
|ts\_predicate\_arima|This function models time series data by using the autoregressive integrated moving average model with differences and performs simple time series prediction and anomaly detection.|
|[Sequence decomposition function](reseller.en-US/User Guide/Index and query/Machine learning syntax and functions/Sequence decomposition function.md)|ts\_decompose|This function uses the STL algorithm to decompose the sequence of time series data.|
|[Time series clustering function](reseller.en-US/User Guide/Index and query/Machine learning syntax and functions/Time series clustering function.md)|ts\_dencity\_cluster|This function clusters time series data by using the density-based clustering method.|
|ts\_hierarchical\_cluster|This function clusters time series data by using the hierarchical clustering method.|
|ts\_similar\_instance|This function queries curves that are similar to a specified curve.|
|Pattern mining|[Frequent pattern statistics](reseller.en-US/User Guide/Index and query/Machine learning syntax and functions/Frequent pattern statistical function.md)|pattern\_stat|This function indicates the frequent pattern in statistical patterns. It is used to mine representative combinations of attributes among the given multi-attribute field samples.|
|[Differential pattern statistics](reseller.en-US/User Guide/Index and query/Machine learning syntax and functions/Differential pattern statistical function.md)|pattern\_diff|This function finds the pattern that causes differences between two sets under specified conditions.|

