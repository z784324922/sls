# Overview {#concept_wjl_x3q_zdb .concept}

Log Service enables you to query and analyze massive amounts of logs in real time by using the LogSearch and Analytics functions. If the index function is disabled, raw data can be used in the order that is defined by Kafka based on Shards. If the index function is enabled, data statistics and query are also supported.

## Functional advantages {#section_byk_btd_5cb .section}

-   Real-time: Logs can be analyzed immediately after they are written.
-   Fast:
    -   Query: Billions of data can be processed and queried within one second \(with five conditions\).
    -   Analysis: Hundreds of millions of data can be aggregated and analyzed within one second \(with aggregation by five dimensions and the GroupBy condition\).
-   Flexible: Query and analysis conditions can be changed as required to obtain results in real time.
-   Extensive: Besides functions such as reports, dashboards, and quick analysis provided in the console, Log Service seamlessly interconnects with products such as Grafana, DataV, and Jaeger, and supports protocols such as RESTful API and JDBC.

## Indexes {#section_rft_gy3_ffb .section}

The index function is designed to sort a specific column or multiple columns in logs. By using indexes, you can quickly access the collected logs. However, before using the LogSearch and Analytics functions, you must collect logs and [enable the index function and configure indexes](intl.en-US/Index and query/Enable and set indexes.md) for the logs.

Log Service provides **full text indexes** and **field indexes**.

-   **Full text indexes**: In this mode, the entire log is configured with indexes. The default index is used to query all keys in the log. The log can be queried even if only one key is matched.
-   **field indexes**: In this mode, indexes are configured for specific keys. This allows you to query a specific key to narrow down the query range.

The data type of fields must be specified when you use **field indexes**. Log Service supports [text](intl.en-US/Index and query/Data type of index/Text type.md), [json](intl.en-US/Index and query/Data type of index/JSON type.md). [long](intl.en-US/Index and query/Data type of index/Value type.md), and [double](intl.en-US/Index and query/Data type of index/Value type.md). For more information, see [Index data type overview](intl.en-US/Index and query/Data type of index/Overview.md).

## Query methods {#section_b3r_4sw_cfb .section}

-   Query logs in the console:

    You can log on to the Log Service console and specify a query time range and enter a query statement on the query and analysis page. For more information, see [Query logs](intl.en-US/Index and query/Query logs.md) and [Query syntax](intl.en-US/Index and query/Query/Query syntax.md).

-   Query logs through API calls:

    You can use the [GetLogs](../../../../intl.en-US/API Reference/Logstore related APIs/GetLogs.md) and [GetHistograms](../../../../intl.en-US/API Reference/Logstore related APIs/GetHistograms.md) APIs to query logs.


**Note:** Before querying logs, you must collect logs and [enable the index function and configure indexes](intl.en-US/Index and query/Enable and set indexes.md) for the logs.

## Query and analysis statement format {#section_ghl_h55_zdb .section}

To query and analyze logs in real time, you need to enter a query and analysis statement. The statement consists of a query statement and an analysis statement, and the two statements are separated by a vertical bar \(`|`\). The following shows an example:

```
$Search |$Analytics
```

|Statement type|Required?|Description|
|:-------------|:--------|:----------|
|Query statement|No|The query condition, which can contain keywords, blur values, numbers, ranges, and combined conditions If the query statement is empty or "\*", no filter condition is set for the current data. That is, all data will be returned. For more information, see [Query syntax](intl.en-US/Index and query/Query/Query syntax.md).

 |
|Analysis statement|No|The analysis statement, which is used to calculate and collect query results or full data. If the analysis statement is empty, only query results will be returned but no statistical analysis will be performed. For more information, see [Syntax description](intl.en-US/Index and query/Syntax description.md).

 |

## Other information {#section_zgq_pr4_tdb .section}

If you query a large amount of log data \(such as a long query time span, where the data volume is over 10 billion\), one request cannot query all the data. In this case, Log Service returns the existing data and notifies you that the query result is incomplete.

At the same time, the server caches the results of the query within 15 minutes. When the query result is partially cached, the server continues to scan log data that has not been cached. To reduce the workload of merging multiple query results, Log Service merges the result of the cache hit with the result of the new query and returns it to you.

Therefore, Log Service enables you to get the final result by calling the interface repeatedly with the same parameters.

