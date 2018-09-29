# Other functions {#concept_iqm_bjq_zdb .concept}

In addition to the statement-based query capability, the query and analysis function of Log Service provides the following extended functions for the query optimization:

-   [Raw logs](#section_zdd_2xj_5cb)
-   [Graph](#section_s3m_lxj_5cb)
-   [Contextual Query](#section_bnf_1gk_5cb)
-   [Quick analysis](#section_g2t_cgk_5cb)
-   [Quick query](#section_p2y_dgk_5cb)
-   [Tag](#section_g1h_ggk_5cb)
-   [Dashboard](#section_hfq_hgk_5cb)
-   [Save as an alarm](#section_g4v_3gk_5cb)

## Raw logs {#section_zdd_2xj_5cb .section}

After the index is enabled, enter the keywords in the search box and select the search time range. Then, click **Search** to view the histogram of the log quantity, the raw logs, and the statistical graph.

The histogram of the log quantity displays the time-based distribution of log search hit counts. With the histogram, you can view the log quantity changes over a certain period of time. By clicking the rectangular area to narrow down the time range, you can view the information about the log hits within the specified time range to refine the display of the log search results.

On the Raw Data tab, you can view the hit logs in chronological order.

-   By clicking the triangle symbol next to **Time**, you can switch between the **chronological** and reverse **chronological** orders.

-   By clicking the triangle symbol next to **Content**, you can switch between **Display with Line Breaks** and **Display in One Line**.

-   By clicking the value keyword in the log content, you can view all logs containing this keyword.

-   By clicking the **Download**button in the upper-right corner of the Raw Data tab, you can download the query results in CSV format. By clicking the **Config**button, you can add fields as displayed columns in the display results of raw logs so that you can view the target field content of each raw log in the new columns in a more intuitive way.

-   By clicking **Context**, you can view 15 logs before and after the current log entry. For more information, see [Context query](reseller.en-US/User Guide/Index and query/Context query.md).

**Note:** Currently, the context query function supports only the data uploaded with Logtail.


![](images/5527_en-US.png "Raw logs")

## Graph {#section_s3m_lxj_5cb .section}

After enabling the index and entering a statement for query and analysis, you can view the statistics of logs under the **Graph** tab.

-   Data can be displayed in the following ways: tables, line charts, column charts, bar charts, pie charts, numeric values, area charts, and maps.

    You can select an appropriate statistical graph type based on the actual statistical analysis needs.

-   You can adjust the display content of axes X and Y to obtain the display results that meet your needs.
-   Add the analysis results to **Dashboard**. For more information, see [Dashboard](reseller.en-US/User Guide/Query and visualization/Analysis graph/Dashboard.md).

![](images/5528_en-US.png "Dashboard")

## Contextual Query {#section_bnf_1gk_5cb .section}

The Log Service console provides a query page, you can view the context information of the specified log in the original file in the console. It is similar to paging up and down in the original log file. By viewing the context information of the specified log, you can quickly locate the failure information during the business troubleshooting.  For more information, see [Context query](reseller.en-US/User Guide/Index and query/Context query.md).

## Quick analysis {#section_g2t_cgk_5cb .section}

The quick analysis function of Log Service supports an interactive query with only one click, allowing you to quickly analyze the distribution of a field over a period of time and reduce the cost of indexing key data. For more information, see [Quick analysis](reseller.en-US/User Guide/Real-time analysis/Quick analysis.md).

## Quick query {#section_p2y_dgk_5cb .section}

By clicking **Saved Search** in the upper-right corner of the query page, you can save the current query action as a quick query. To perform this query again, you can quickly complete it on the **Saved Search** tab on the left without manually entering the query statement.

You can also use this quick query condition in alarm rules. If you have added this quick query to **Tag**, you can directly access it in tags.

## Tag {#section_g1h_ggk_5cb .section}

You can add the following three types of data pages to the tag list on the left of the Log Service query homepage:

-   Logstore
-   Quick query
-   Dashboard

The tag list allows you to open pages easily and quickly. You can directly click to open the Logstore, saved quick queries, and dashboards in the tag list.  To add the Logstore, quick query, or dashboard as tags, click **Add Tag** in the tag list and select the Logstore, quick query, or dashboard you want to add in the menu appeared in the right side of the page. To delete a tag, click the remove \(X\) button at the right of the tag name to be deleted in the tag list.

![](images/5529_en-US.png "Tag")

## Dashboard {#section_hfq_hgk_5cb .section}

Log Service provides the dashboard function, which can visualize the query and analysis statements. For more information, see [Dashboard](reseller.en-US/User Guide/Query and visualization/Analysis graph/Dashboard.md).

![](images/5530_en-US.png "Dashboard")

## Save as an alarm {#section_g4v_3gk_5cb .section}

Log Service can generate an alarm based on your **LogSearch Results**. You can configure the alarm rules so that specific alarm content can be sent to you by using in-site notifications or DingTalk messages.

The basic process is as follows:

1.  Configure the quick query
2.   Configure the alarm rules.
3.  Configure notification type.
4.  The system sends an SMS/email so that you can view the alarm result.

For more information, see [Set alarms](reseller.en-US/User Guide/Alarm and notification/Set alarms.md).

