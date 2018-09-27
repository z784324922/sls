# Year-on-year comparison function and period-over-period comparison function {#concept_ftn_3hd_p2b .concept}

Year-on-year comparison function and period-over-period comparison function are used to compare the calculation results of the current interval with the results of a previous specified interval.

|Function|Description|Example|
|:-------|:----------|:------|
|`compare(value, time_window)`|This function compares the value calculated in the current time period with the value calculated by time\_window.value is in the type of double or long. The unit of time\_window is seconds. The return values are in the array type.

The return values include the current value, the value before time\_window, and the ratio of the current value to the value before time\_window.

|`*|select compare( pv , 86400) from (select count(1) as pv from log)`|

## Example {#section_hcx_rhd_p2b .section}

-   Calculate the ratio of the current 1-hour PV to the same time period PV of yesterday.

    The starting time is 2010-07-25 14:00:00; the ending time is 2018-07-25 15:00:00.

    The query analysis statement is:

    ```
    * | select compare( pv , 86400) from (select count(1) as pv from log)
    ```

    86400 represents the current time period minus 86400 seconds.

    Returned result:

    ```
    [9.0,19.0,0.47368421052631579]
    ```

    The descriptions for these returned values are as follows:

    -   9.0 is the PV value from 2018-07-25 14:00:00 to 2018-07-25 15:00:00.
    -   19.0 is the PV value of 2018-07-24 14:00:00 to 2018-07-24 15:00:00.
    -   0.47368421052631579 is the ratio of the current time period to the previous time period.
    If you expand the array into three columns of numbers, the analysis statement is:

    ```
    * | select diff[1],diff[2],diff[3] from(select compare( pv , 86400) as diff from (select count(1) as pv from log))
    ```

-   Calculate the ratio of the PV for each minute in the current one hour to the PV for the same time period of yesterday, and display the result in a line chart.

    1.  Calculate the ratio of the PV for each minute in the current one hour to the PV for the same time period of yesterday. The starting time is 2018-07-25 14:00:00, and the ending time is 2018-07-25 15:00:00.

        The query analysis statement is:

        ```
        *| select t, compare( pv , 86400) as diff from (select count(1) as pv, date_format(from_unixtime(__time__), '%H:%i') as t from log group by t) group by t order by t
        ```

        Returned result:

        |t|diff|
        |:-|:---|
        |14:00|\[9520.0,7606.0,1.2516434393899554\]|
        |14:01|\[8596.0,8553.0,1.0050274757395066\]|
        |14:02|\[8722.0,8435.0,1.0340248962655603\]|
        |14:03|\[7499.0,5912.0,1.2684370771312586\]|

        t represents time in the form of `hour:minute`. The content of the diff column is an array containing the following:

        -   The PV value for the current time period.
        -   The PV value for the previous time period.
        -   The ratio of the PV value for the current time period to the PV value for the previous time period.
    2.  Use the following statement to expand the query results in the form of a line chart:

        ```
        *|select t, diff[1] as current, diff[2] as yestoday, diff[3] as percentage from(select t, compare( pv , 86400) as diff from (select count(1) as pv, date_format(from_unixtime(__time__), '%H:%i') as t from log group by t) group by t order by t)
        ```

        Configure the query result as a line chart, in which the two lines represent today's value and yesterday's value, respectively:

        ![](images/7639_en-US.png "Line chart")


