# Date and time functions {#LogService_user_guide_0110 .reference}

Log Service supports time functions, date functions, interval functions, and a time series padding function. You can use the functions described in this topic in analysis statements.

## Date and time data types {#section_kwg_n5k_5cb .section}

1.  Unix timestamp: the number of seconds that have elapsed since 00:00:00 \(UTC\) on January 1, 1970. The value is of the integer type. For example, `1512374067` indicates the time `Mon Dec 4 15:54:27 CST 2017`. The built-in time `__time__` in each log of Log Service is of this type.
2.  timestamp: the time in the format of string, for example, `2017-11-01 13:30:00`.

## Date functions {#section_ek4_q5k_5cb .section}

The following table describes the common date functions supported by Log Service.

|Function|Description|Example|
|:-------|:----------|:------|
|`current_date`|Returns the current date.|`latency>100| select current_date`|
|`current_time`|Returns the current time.|`latency>100| select current_time`|
|`current_timestamp`|Returns the current timestamp by combining the results of current\_date and current\_time.|`latency>100| select current_timestamp`|
|`current_timezone()`|Returns the current time zone.|`latency>100| select current_timezone()`|
|`from_iso8601_timestamp(string)`|Parses an ISO 8601-formatted time into a timestamp that contains the time zone.|`latency>100| select from_iso8601_timestamp(iso8601)`|
|`from_iso8601_date(string)`|Parses an ISO 8601-formatted time into a date.|`latency>100| select from_iso8601_date(iso8601)`|
|`from_unixtime(unixtime)`|Returns a Unix timestamp as a timestamp.|`latency>100| select from_unixtime(1494985275)`|
|`from_unixtime(unixtime,string)`|Returns a Unix timestamp as a timestamp that uses the specified string as the time zone.|`latency>100| select from_unixtime (1494985275,'Asia/Shanghai')`|
|`localtime`|Returns the current time.|`latency>100| select localtime`|
|`localtimestamp`|Returns the current timestamp.|`latency>100| select localtimestamp`|
|`now()`|Functions the same as `current_timestamp`.|N/A|
|`to_unixtime(timestamp)`|Returns a timestamp as a Unix timestamp.|`*| select to_unixtime('2017-05-17 09:45:00.848 Asia/Shanghai')`|

## Time functions {#section_jky_3mq_tdb .section}

**MySQL time formats**

Log Service supports the MySQL time formats, such as %a, %b, and %y.

|Function|Description|Example|
|:-------|:----------|:------|
| `date_format(timestamp, format)` |Formats a timestamp as a string by using the specified format.|`latency>100`| `select date_format (date_parse('2017-05-17 09:45:00','%Y-%m-%d %H:%i:%S'), '%Y-%m-%d')`|
| `date_parse(string, format)` |Parses a string into a timestamp by using the specified format.|`latency>100`|`select date_format (date_parse(time,'%Y-%m-%d %H:%i:%S'), '%Y-%m-%d')`|

|Format|Description|
|:-----|:----------|
|%a|The abbreviation of a day in a week, such as Sun and Sat.|
|%b|The abbreviation of a month, such as Jan and Dec.|
|%c|The month, of the numeric type. Valid values: \[1, 12\].|
|%D|The day in a month with a suffix, such as 0th, 1st, 2nd, and 3rd.|
|%d|The day in a month, in decimal format. Valid values: \[01, 31\].|
|%e|The day in a month, in decimal format. Valid values: \[1, 31\].|
|%H|The hour in 24-hour format.|
|%h|The hour in 12-hour format.|
|%I|The hour in 12-hour format.|
|%i|The minutes, of the numeric type. Valid values: \[00, 59\].|
|%j|The day in a year. Valid values: \[001, 366\].|
|%k|The hour. Valid values: \[0, 23\].|
|%l|The hour. Valid values: \[1, 12\].|
|%M|The month. Valid values: January, February, March, April, May, June, July, August, September, October, November, and December.|
|%m|The month, of the numeric type. Valid values: \[01, 12\].|
|%p|The abbreviation of a period in 12-hour format. Valid values: AM and PM.|
|%r|The time in 12-hour format: `hh:mm:ss AM/PM`.|
|%S|The seconds. Valid values: \[00, 59\].|
|%s|The seconds. Valid values: \[00, 59\].|
|%T|The time in 24-hour format: `hh:mm:ss`.|
|%U|The week in a year, where Sunday is the first day of each week. Valid values: \[00, 53\].|
|%u|The week in a year, where Monday is the first day of each week. Valid values: \[00, 53\].|
|%V|The week in a year, where Sunday is the first day of each week. Valid values: \[01, 53\]. %V is used with %X.|
|%v|The week in a year, where Monday is the first day of each week. Valid values: \[01, 53\]. %v is used with %x.|
|%W|The day in a week. Valid values: Sunday, Monday, Tuesday, Wednesday, Thursday, Friday, and Saturday.|
|%w|The day in a week, where Sunday is the first day of each week. Valid values: \[0, 6\].|
|%Y|The year in four-digit format.|
|%y|The year in two-digit format.|
|%%|The escape character %.|

## Truncation function {#section_zpc_jv2_4fb .section}

Log Service supports a truncation function, which can return a time truncated by the second, minute, hour, day, month, and year. The truncation function is applicable to time-based statistics.

-   Function syntax:

    ```
    date_trunc(unit, x)
    ```

-   Parameters:

    The value of the x parameter can be a timestamp or Unix timestamp.

    The following table lists the values of the unit parameter and the output results when the x parameter is set to `2001-08-22 03:04:05.000`.

    |Value|Output result|
    |:----|:------------|
    |second|2001-08-22 03:04:05.000|
    |minute|2001-08-22 03:04:00.000|
    |hour|2001-08-22 03:00:00.000|
    |day|2001-08-22 00:00:00.000|
    |week|2001-08-20 00:00:00.000|
    |month|2001-08-01 00:00:00.000|
    |quarter|2001-07-01 00:00:00.000|
    |year|2001-01-01 00:00:00.000|

-   Example:

    The date\_trunc function is applicable only to statistics at a fixed time interval. For statistics based on flexible time dimensions, for example, every 5 minutes, you need to use a GROUP BY clause according to the mathematical modulus method.

    ```
    * | SELECT count(1) as pv,  __time__ - __time__% 300 as minute5 group by minute5 limit 100
    ```

    In the preceding statement, `%300` indicates that modulus and truncation are performed every 5 minutes.

    The following example shows how to use the date\_trunc function:

    ```
    *|select  date_trunc('minute' ,  __time__)  as t,
           truncate (avg(latency) ) ,
           current_date  
           group by   t
           order by t  desc 
           limit 60
    ```


## Interval functions {#section_frx_vvh_t2b .section}

Interval functions are used to perform interval-related calculation. For example, add or subtract an interval based on a date, or calculate the interval between two dates.

|Function|Description|Example|
|:-------|:----------|:------|
|``date_add`(unit, value, timestamp)`|Adds an interval `value` of the `unit` type to a `timestamp`. To subtract an interval, use a negative `value`.|`date_add('day', -7, '2018-08-09 00:00:00')`: indicates seven days before August 9.|
|`date_diff(unit, timestamp1, timestamp2)`|Returns the time difference between `timestamp1` and `timestamp2` expressed in terms of `unit`.|`date_diff('day', '2018-08-02 00:00:00', '2018-08-09 00:00:00') = 7`|

The following table lists the values of the unit parameter supported by the interval functions.

|Value|Description|
|:----|:----------|
|millisecond|The millisecond.|
|second|The second.|
|minute|The minute.|
|hour|The hour.|
|day|The day.|
|week|The week.|
|month|The month.|
|quarter|The quarter, namely, three months.|
|year|The year.|

## Time series padding function {#section_wsz_wt2_4fb .section}

The time series padding function time\_series is used to pad time series data with the missing time.

-   Function format:

    ```
    time_series(time\_column, window, format, padding\_data)
    ```

-   The following table describes the parameters.

    |Parameter|Description|
    |:--------|:----------|
    |time\_column|The sequence of time, such as the default time field `__time__` provided by Log Service. The value is of the long or timestamp type.|
    |window|The size of the time window, which is composed of a number and a unit. Unit: s \(seconds\), m \(minutes\), H \(hours\), or d \(days\). For example, 2h, 5m, or 3d.|
    |format|The MySQL time format, which is the final output format.|
    |padding\_data|The content to be added. Valid values:    -   0: adds 0.
    -   null: adds null.
    -   last: adds the last value.
    -   next: adds the next value.
    -   avg: adds the average value of the last and next values.
|

-   Example:

    The following statement is used to format data every 2 hours:

    ```
    * | select time_series(__time__, '2h', '%Y-%m-%d %H:%i:%s', '0')  as stamp, count(*) as num from log group by stamp order by stamp
    
    ```

    The following figure shows the output result.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13111/156205726737530_en-US.png)


