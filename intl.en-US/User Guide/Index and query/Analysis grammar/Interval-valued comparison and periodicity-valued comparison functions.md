# Interval-valued comparison and periodicity-valued comparison functions {#concept_ftn_3hd_p2b .concept}

Interval-valued comparison and periodicity-valued comparison functions are used to compare the calculation results of the current period with those of a specified previous period.

|Function|Description|Example|
|:-------|:----------|:------|
|`compare(value, time_window)`|This function compares the value calculated for the current period with that calculated by time\_window.The values are double or long type values. The unit of time\_window is seconds. The return values are in array format.

Possible return values include the current value, the value before time\_window, and the ratio of the current value to the value before time\_window.

|`*|select compare( pv , 86400) from (select count(1) as pv from log)`|
|`compare(value, time_window1, time_window2)`|This function compares the current value with the values of periods before time\_window1 and time\_window2. The comparison results are in JSON array format, where the values must be placed in the following sequence: \[current value, value before time\_window1, value before time\_window2, current value/value before time\_window1, current value/value before time\_window2\].|`* | select compare(pv, 86400, 172800) from ( select count(1) as pv from log)`|
|`compare(value, time_window1, time_window2, time_window3)`|This function compares the current value with the values of periods before time\_window1, time\_window2 and time\_window3. The comparison results are in JSON array format, where the values must be placed in the following sequence: \[current value, value before time\_window1, value before time\_window2, value before time\_window3, current value/Value before time\_window1, current value/value before time\_window2, current value/value before time\_window3\].|`* | select compare(pv, 86400, 172800,604800) from ( select count(1) as pv from log)`|
|`compare_result(value,time_window)`|This function works similarly to `compare(value,time_window)`. However, the return values are string type values in the format of `"Current value(Increased percentage%)"`. The increased percentage value is rounded to two decimal places by default.|`* | select compare_result(pv, 86400) from ( select count(1) as pv from log)`|
|`compare_result(value,time_window1, time_window2)`|This function works similarly to `compare(value,time_window1, time_window2)`. However, the return values are string type values in the format of `"Current value(Increased percentage compared with the first period%) (Increased percentage compared with the second period)"`. The increased percentage values are rounded to two decimal places by default.|`* | select compare_result(pv, 86400,172800) from ( select count(1) as pv from log)`|
|`ts_compare(value, time_window)`|This function compares the current value with the values of periods before `time_window1` and `time_window2`. The comparison results are in JSON array format, where the values must be placed in the following sequence: \[current value, value before time\_window1, current value/value before time\_window1, unix timestamp at the start time of the previous period\].This function is used to compare time series functions. This requires the GROUP BY operation to be included in SQL statements on the time column.

|For example, `* | select t, ts_compare(pv, 86400 ) as d from(select date_trunc('minute',__time__ ) as t, count(1) as pv from log group by t order by t ) group by t` indicates that the function compares the calculation result of every minute in the current period with that of every minute in the last period.The comparison result is `d:[1251.0,1264.0, 0.9897151898734177, 1539843780.0,1539757380.0]t:2018-10-19 14:23:00.000`.

|

## Examples {#section_hcx_rhd_p2b .section}

-   Calculate the ratio of the PV in the current hour to that in the same time period as yesterday.

    The start time is 2018-07-25 14:00:00, and the end time is 2018-07-25 15:00:00.

    Statement for query and analysis:

    ```
    * | select compare( pv , 86400) from (select count(1) as pv from log)
    ```

    where 86400 indicates that 86400 seconds are subtracted from the current period.

    Return result:

    ```
    [9.0,19.0,0.47368421052631579]
    ```

    where:

    -   9.0 is the PV value from 2018-07-25 14:00:00 to 2018-07-25 15:00:00.
    -   19.0 is the PV value from 2018-07-24 14:00:00 to 2018-07-24 15:00:00.
    -   0.47368421052631579 is the ratio of the PV value of the current period to that of a previous period.
    If you want to expand the array into three columns of numbers, the analysis statement is:

    ```
    * | select diff[1],diff[2],diff[3] from(select compare( pv , 86400) as diff from (select count(1) as pv from log))
    ```

-   Calculate the ratio of the PV in every minute of the current hour to that in the same time period as yesterday, and display the results in a line chart.

    1.  Calculate the ratio of the PV in every minute of the current hour to that in the same time period as yesterday. The start time is 2018-07-25 14:00:00, and the end time is 2018-07-25 15:00:00.

        Statement for query and analysis:

        ```
        *| select t, compare( pv , 86400) as diff from (select count(1) as pv, date_format(from_unixtime(__time__), '%H:%i') as t from log group by t) group by t order by t
        ```

        Return results:

        |t|diff|
        |:-|:---|
        |14:00|\[9520.0,7606.0,1.2516434393899554\]|
        |14:01|\[8596.0,8553.0,1.0050274757395066\]|
        |14:02|\[8722.0,8435.0,1.0340248962655603\]|
        |14:03|\[7499.0,5912.0,1.2684370771312586\]|

        where t indicates the time in the format of `Hour:Minute`. The content of the diff column is an array containing the following:

        -   The PV value of the current period.
        -   The PV value of the previous period.
        -   The ratio of the PV value in the current period to that in the previous period.
    2.  To show the query results in a line chart, use the following statement:

        ```
        *|select t, diff[1] as current, diff[2] as yestoday, diff[3] as percentage from(select t, compare( pv , 86400) as diff from (select count(1) as pv, date_format(from_unixtime(__time__), '%H:%i') as t from log group by t) group by t order by t)
        ```

        The two lines indicate the PV values of today and yesterday.

        ![](images/7639_en-US.png "Line chart")


