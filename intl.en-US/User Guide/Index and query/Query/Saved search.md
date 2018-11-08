# Saved search {#task_uv5_kdj_y2b .task}

Saved search is a one-click query and analysis function provided by Log Service.

You have enabled and configured **Index**.

If you need to frequently view the results of a query and analysis statement, save the statement as a saved search. In later result searches, you only need to click the name of the saved search on the left side of the search page. You can also use this saved search condition in alarm rules. Log Service executes the statement of this saved search periodically and sends an alarm notification when the search result meets the pre-configured condition of the statement.

To configure a drill-down event to **jump to a saved search** when configuring [Drill-down analysis](reseller.en-US/User Guide/Query and visualization/Analysis graph/Drill-down analysis.md), you must pre-configure a saved search and set a **placeholder** in the query statement.

1.  Log on to the [Log Service console](https://partners-intl.console.aliyun.com/#/sls) and click the project name. 
2.  Click **Search** in the **LogSearch** column on the Logstores page. 
3.  Enter your query analysis statement, set the time range, and click **Search & Analysis**. 
4.  Click **Save Search** in the upper-right corner of the page. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18787/154166221810769_en-US.png)

5.  Configure saved search attributes. 
    1.  Set **Saved Search Name**. 
        -   The name can only contain lowercase letters, numbers, hyphens \(-\), and underscores \(\_\).
        -   The name must start and end with a lowercase letter or a number.
        -   The name must be a string of 3 to 63 characters.
    2.  Confirm **Logstores**, **Topic**, and **Query**. If **Logstore** and **Topic** do not meet your requirements, return to the search page to access the proper Logstore and enter your query statement, and then click **Save Search** again.
    3.  Select part of the query statement and click **Generate Variable**. The generated variable is a placeholder variable. Name the placeholder in the **Variable Name** box. **Default Value** is the selected word.

        **Note:** If the drill-down event of a chart is to jump to the saved search and the chart has the same **variable** as this saved search, clicking the chart triggers the jump. Additionally, the **default value** of the placeholder variable is replaced with the chart value that triggers the drill-down event, and the query statement with the variable replaced is used for querying. For more information, see [Drill-down analysis](reseller.en-US/User Guide/Query and visualization/Analysis graph/Drill-down analysis.md).

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18787/154166221810770_en-US.png)

6.  Click **OK** to end the configuration. 

