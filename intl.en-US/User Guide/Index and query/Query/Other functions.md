# Other functions {#concept_iqm_bjq_zdb .concept}

Search and analysis functions help you to query various statements in logs, [query raw logs](#section_zdd_2xj_5cb), [view graphs](#section_s3m_lxj_5cb), [query context](#section_bnf_1gk_5cb), [perform quick analysis](#section_g2t_cgk_5cb), [perform quick queries](#section_p2y_dgk_5cb), [create a dashboard](#section_hfq_hgk_5cb), and [save a graph as an alarm](#section_g4v_3gk_5cb).

## Query raw logs {#section_zdd_2xj_5cb .section}

After the index is enabled, enter the keywords in the search box and select the search time range. Then, click **Search** to view the histogram of the log quantity, the raw logs, and the statistical graph.

The histogram of the log quantity displays the time-based distribution of log search hit counts. With the histogram, you can view the log quantity changes over a certain period of time. By clicking the rectangular area to narrow down the time range, you can view the information about the log hits within the specified time range to refine the display of the log search results.

On the Raw Data tab, you can view the hit logs in chronological order.

-   By clicking the triangle symbol next to **Time**, you can switch between the **chronological** and reverse **chronological** orders.
-   By clicking **Display Content Column**, you can switch between **Display with Line Breaks** and **Display in One Line**, or you can set **Truncate Character String**.
-   By clicking the value keyword in the log content, you can view all logs containing this keyword.
-   By clicking the **Download**button in the upper-right corner of the Raw Data tab, you can download the query results in CSV format. By clicking the **Config**button, you can add fields as displayed columns in the display results of raw logs so that you can view the target field content of each raw log in the new columns in a more intuitive way.
-   By clicking **Context**, you can view 15 logs before and after the current log entry. For more information, see [Context query](reseller.en-US/User Guide/Index and query/Query/Context query.md).

    **Note:** Currently, the context query function supports only the data uploaded with Logtail.


![](images/5527_en-US.png "Raw logs")

## View graphs {#section_s3m_lxj_5cb .section}

After enabling the index and entering a statement for query and analysis, you can view the statistics of logs under the **Graph** tab.

-   Data can be displayed in tables, line charts, or other types of graphs.

    You can choose an appropriate statistical graph and custom graph settings as needed.

-   You can add a graph to the **Dashboard**. For more information, see [Dashboard](reseller.en-US/User Guide/Query and visualization/Dashboard/Dashboard.md).
-   You can set the [drill-down analysis](reseller.en-US/User Guide/Query and visualization/Dashboard/Drill-down analysis.md) action for a graph. Then, after a graph is added to the dashboard, any click to a data point on the graph will trigger the drill-down analysis action, allowing you to review queries in more details.

![](images/5528_en-US.png "Statistical graphs")

## Query context {#section_bnf_1gk_5cb .section}

The Log Service console provides a query page, you can view the context information of the specified log in the original file in the console. It is similar to paging up and down in the original log file. By viewing the context information of the specified log, you can quickly locate the failure information during the business troubleshooting.  For more information, see [Context query](reseller.en-US/User Guide/Index and query/Query/Context query.md).

## Perform quick analysis {#section_g2t_cgk_5cb .section}

The quick analysis function of Log Service supports an interactive query with only one click, allowing you to quickly analyze the distribution of a field over a period of time and reduce the cost of indexing key data. For more information, see [Quick analysis](reseller.en-US/User Guide/Index and query/Query/Quick analysis.md).

## Perform quick queries {#section_p2y_dgk_5cb .section}

You can save the current query condition as a saved search. To perform this query again, you simply need to go to the saved search page. For more information, see [Saved search](reseller.en-US/User Guide/Index and query/Query/Saved search.md).

You can also apply the saved search condition to alarm rules. After you set an alarm rule, Log Service will automatically run the saved search on a regular basis. If query results meet the preset threshold, Log Service will send an alarm message.

## Create a dashboard {#section_hfq_hgk_5cb .section}

Log Service provides the dashboard function, which can visualize the query and analysis statements. For more information, see [Dashboard](reseller.en-US/User Guide/Query and visualization/Dashboard/Dashboard.md).

![](images/5530_en-US.png "Dashboard")

## Save a graph as an alarm {#section_g4v_3gk_5cb .section}

Log Service can generate an alarm based on your **LogSearch Results**. You can configure the alarm rules so that specific alarm content can be sent to you by using in-site notifications or DingTalk messages.

For more information, see [Set alarms](reseller.en-US/User Guide/Alarm and notification/Set alarms.md).

