# Optimized queries {#concept_vv5_qmq_zdb .concept}

This topic describes how to optimize queries to improve query efficiency. You can use the following methods to optimize queries:

-   Add shards.
-   Reduce the query time range and data volume.
-   Repeat queries multiple times.
-   Optimize the SQL statement for query.

## Add shards {#section_elf_5wf_hhb .section}

More shards represent more computing resources and faster computing. You can add shards to ensure that the number of logs to be scanned in each shard does not exceed 50 million on average. You can [split shards](reseller.en-US/User Guide/Preparation/Manage shards.md#) to add shards.

**Note:** Splitting shards incurs more fees and only accelerates queries of new data. Old data is still stored in old shards.

## Reduce the query time range and data volume {#section_qml_jxf_hhb .section}

-   The larger the time range, the slower the query. If you query data within a year or a month, data is computed by day. Therefore, you can reduce the time range for faster computing.
-   The larger the data volume, the slower the query. Reduce the amount of data to be queried as much as possible.

## Repeat queries multiple times {#section_c2h_kyf_hhb .section}

If you find that the result of a query is inaccurate, you can repeat the query multiple times. During each query, the underlying acceleration mechanism makes full use of the previous query result for analysis. Therefore, multiple queries make the query result more accurate.

## Optimize the SQL statement for query {#section_mrh_vyf_hhb .section}

A time-consuming query statement has the following characteristics:

-   Runs GROUP BY on string columns.
-   Runs GROUP BY on more than five columns of fields.
-   Includes the operation that generates strings.

You can use the following methods to optimize the query statement:

-   **Avoid any operation that generates strings if possible.**
    -   If you use the date\_format function to generate a formatted timestamp, the query efficiency is low.

        ```
        * | select date_format(from_unixtime(__time__) , '%H_%i') as t, count(1) group by t
        ```

    -   If you use the substr\(\) method, strings are generated. We recommend that you use the date\_trunc or time\_series function to analyze timestamps.
-   **Avoid running GROUP BY on string columns if possible.**

    Running GROUP BY on strings may result in a large number of hash calculations, which account for more than 50% of total calculations. For example:

    ```
    * | select count(1) as pv , date_trunc('hour',__time__) as time group by time
    * | select count(1) as pv , from_unixtime(__time__-__time__%3600) as time group by __time__-__time__%3600
    ```

    Both query 1 and query 2 calculate the log count per hour. However, query 1 converts the time into a string, for example, 2017-12-12 00:00:00, and then runs GROUP BY on this string. Query 2 calculates the on-the-hour time value, runs GROUP BY on the result, and then converts the value into a string. Query 1 is less efficient than query 2 because the former one needs to hash strings.

-   **List fields alphabetically based on the initial letter when running GROUP BY on multiple columns.**

    For example, there are 13 provinces with 100 million users.

    ```
    Fast: * | select province,uid,count(1)groupby province,uid
    Slow: * | select province,uid,count(1)groupby uid,province
    ```

-   **Use estimating functions.**

    Estimating functions provide much better performance than accurate calculation. Estimation achieves fast calculation by sacrificing accuracy to some acceptable extent.

    ```
    Fast: * |select approx_distinct(ip)
    Slow: * | select count(distinct(ip))
    ```

-   **Retrieve only required columns in the SQL statement and avoid reading all columns if possible.**

    Use the query syntax to retrieve all columns. To speed up calculation, retrieve only required columns in the SQL statement if possible.

    ```
    Fast: * |select a,b c 
    Slow: * |select*
    ```

-   **Place non-GROUP BY columns in an aggregate function if possible.**

    For example, a user ID exactly matches a username. Therefore, run GROUP BY on only userid instead of on both userid and username.

    ```
    Fast: * | select userid, arbitrary(username), count(1)groupby userid 
    Slow: * | select userid, username, count(1)groupby userid,username
    ```

-   **Avoid using the IN operator if possible.**

    Do not use the IN operator in SQL statements if possible. Instead, use the OR operator.

    ```
    Fast: key : a or key :b or key:c | select count(1)
    Slow: * | select count(1) where key in ('a','b')
    ```


