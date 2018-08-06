# Quick analysis {#task_eqm_dvj_5cb .task}

The quick analysis  function of Log Service supports an interactive query with only one click, allowing you to quickly analyze the distribution of a field over a period of time and reduce the cost of indexing key data.

## Functions and features {#section_hct_fvj_5cb .section}

-   Support grouping statistics for the first 10 of the first 100,000 pieces of data of `Text`fields.
-   Support generating `approx_distinct`statements quickly for `Text`fields.
-   Support histogram statistics for the approximate distribution of `long` or `double`fields.
-   Support the quick search for the maximum, minimum, average, or sum of `long` or `double` fields.
-   Support generating query statements based on quick analysis and query.

## Prerequisite {#prereq_qkk_pvj_5cb .section}

You must specify the field query properties before using the quick analysis.

1.  For specified field query, you must enable the index to activate the query and analysis function. For how to enable the index, see [Query and analysis](intl.en-US/User Guide/Index and query/Overview.md).
2.  Set the `key` in the log as the field name and set the type, alias, and separator.

    If the access log contains the `request_method` and `request_time`, you can configure the following settings.

    ![](images/5590_en-US.png "Prerequisites")


## User Guide {#section_lly_xvj_5cb .section}

After setting the specified field query, you can see the fields in Quick Analysis under the **Raw Data** tab on the query page. By clicking the 1 button above the serial number, you can fold the page.  By clicking the **eye** button, you can perform quick analysis based on the **Current Temporal Interval** and**Current $Search conditions**.

![](images/5591_en-US.png "Original log")

## Text {#section_o22_4wj_5cb .section}

-   **Grouping statistics for Text fields**

    Click the **eye**button at the right of the filed to quickly group the first 100,000 pieces of data of this **Text** field and return the ratio of the first 10 pieces.

    Query statement:

    ```
    $Search | select ${keyName} , pv, pv *1.0/sum(pv) over() as percentage from( select count(1) as pv , "${keyName}" from (select "${keyName}" from log limit 100000) group by "${keyName}" order by pv desc) order by pv desc limit 10
    ```

    `request_method` returns the following result based on the grouping statistics, where GET requests are in the majority.

    ![](images/5593_en-US.png "Group statistics")

-   **Check the number of unique entries of the field**

    Under the target fields in **Quick Analysis**, click**approx\_distinct** to check the number of unique entries for `${keyName}`.

      `request_method` can get the following result by grouping statistics, and `GET` requests account for the majority:

-   **Extend the query statement of grouping statistics to the search box**

    Click the button at the right of **approx\_distinct** to extend the query statement of grouping statistics to the search box for further operations.


## long/double {#section_ovl_pwj_5cb .section}

-   **Histogram statistics for the approximate distribution**

    Grouping statistics is of little significance for the `long/double` fields, which have multiple type values. Therefore, histogram statistics for the approximate distribution is adopted by using 10 buckets.

    ```
    $Search | select numeric_histogram(10, ${keyName})
    ```

    `request_time` returns the following result based on the histogram statistics for the approximate distribution, from which you can see that the request time is mostly distributed around 0.056.

    ![](images/5594_en-US.png "Request Distribution")

-   **Quick analysis of the`Max``Min``Avg``Sum` statements**

    Respectively click `Max`, `Min`, `Avg`, and `Sum` under the target fields to quickly search for the maximum, minimum, average, and sum of all $\{keyName\}.

-   **Extend the query statement of grouping statistics to the search box**

    Click the button at the right of `Sum` to extend the query statement of the histogram statistics for the approximate distribution to the search box for further operations.


