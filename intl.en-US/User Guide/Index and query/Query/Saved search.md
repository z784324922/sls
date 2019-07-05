# Saved search {#task_uv5_kdj_y2b .task}

Saved search is a one-click query and analysis feature provided by Log Service.

**Indexes** are enabled and set.

If you need to frequently view the results of a query and analysis statement, you can save the statement as a saved search. In next searches, you only need to click the name of the saved search on the left side of the search page, instead of entering the statement again. You can also use this saved search in alert rules. Log Service runs the statement of this saved search periodically and sends an alert notification when the search result meets the preset condition of the statement.

To configure a drill-down event to **jump to a saved search** when configuring [Drill-down analysis](reseller.en-US/User Guide/Query and visualization/Dashboard/Drill-down analysis.md), you must preset a saved search and set a **placeholder** in the query and analysis statement.

**Note:** 

**Q: How can I modify a saved search?**

A: To modify a saved search, enter a new query and analysis statement, click **Search & Analysis** to run the statement, and then click **Modify Saved Search**.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18787/156232065336656_en-US.png)

1.  Log on to the [Log Service console](https://partners-intl.console.aliyun.com/#/sls), and then click the target project name. 
2.  On the Logstores page, find the target Logstore and click **Search** in the **LogSearch** column. 
3.  Enter a query and analysis statement in the search box, set the time range, and then click **Search & Analysis**. 
4.  Click **Save Search** in the upper-right corner of the page. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18787/156232065410769_en-US.png)

5.  Configure saved search attributes. 
    1.  Set **Saved Search Name**. 
        -   The name can contain only lowercase letters, digits, hyphens \(-\), and underscores \(\_\).
        -   The name must start and end with a lowercase letter or digit.
        -   The name must be 3 to 63 characters in length.
    2.  Check the values of **Logstores**, **Topic**, and **Query**. If the values of **Logstores** and **Topic** do not meet your requirements, return to the search page, go to the target Logstore, enter the query and analysis statement, and then click **Save Search** again.
    3.  Select part of the query statement and click **Generate Variable**. The generated variable is a placeholder variable. Set the placeholder variable name in the **Variable Name** field. **Default Value** indicates the selected word.

        **Note:** If the drill-down event of a chart is configured to jump to this saved search and the chart has the same variable as the value of **Variable Name** of this saved search, clicking the chart triggers the jump. ****Additionally, the value of **Default Value** of the placeholder variable is replaced with the chart value that triggers the drill-down event. The query statement with the replaced variable replaced is run. For more information, see [Drill-down analysis](reseller.en-US/User Guide/Query and visualization/Dashboard/Drill-down analysis.md).

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18787/156232065410770_en-US.png)

6.  Click **OK**. 

