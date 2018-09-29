# Table {#concept_e3d_bnq_zdb .concept}

Table, as the most common display type of data, is the most basic method to organize data. By organizing the data, table references and analyzes the data quickly. Log Service provides a function similar to the SQL aggregate computing. By default, the results obtained by using the query and analysis syntax are displayed in a table.

## Basic components {#section_mr3_tnv_tdb .section}

-   Header
-   Row
-   Column

Wherein:

-   The number of `SELECT` items is the number of columns.
-   The number of rows is determined by the number of logs after being computed in the current time interval. The default value is `LIMIT 100` .

## Procedure { .section}

1.  On the query page, enter the query statement in the search box, select the time interval, and then click **Search**.
2.  Click the Graph tab, the query results are displayed in a table ![](https://cdn.yuque.com/lark/2018/png/60648/1523154665568-24efeb11-b7d4-4d71-9fd5-2a26139a3180.png) by default.

## Example {#section_p1c_k4v_tdb .section}

The raw log is as follows. 

![](images/5701_en-US.png "Original log")

1.  To obtain the columns hostname , remote\_addr , and request\_uri of the latest 10 logs, the statement is as follows:

    ```
    * | SELECT hostname, remote_addr, request_uri GROUP BY hostname, remote_addr, request_uri LIMIT 10
    ```

    ![](images/5702_en-US.png "case 1")

2.  To compute a single data, for example, the average request\_time \(the average request time\) in the current time interval, and retain three decimal places, the statement is as follows:

    ```
    * | SELECT round(avg(request_time), 3) as average_request
    ```

    ![](images/5703_en-US.png "case 2")

3.  To compute grouped data, for example, the request\_method distribution in the current time interval, and display the distribution in descending order, the statement is as follows:

    ```
    * | SELECT request_method, count(*) as count GROUP BY request_method ORDER BY count DESC
    ```

    ![](images/5704_en-US.png "case 3")


