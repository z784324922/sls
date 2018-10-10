# Date and time functions {#LogService_user_guide_0110 .reference}

Log Service supports time functions, date functions, and interval functions. You can use the date, time, and interval functions introduced in this document in the analysis syntax.

## Date and time type {#section_kwg_n5k_5cb .section}

1.  unixtime: Indicates the number of seconds since January 1, 1970 in the type of int. For example, `1512374067` indicates the time `Mon Dec 4 15:54:27 CST 2017`. The built-in time `__time__` in each log of Log Service is of this type.
2.  timestamp type: Indicates the time in the format of string. For example, `2017-11-01 13:30:00`.

## Date functions {#section_ek4_q5k_5cb .section}

The common date functions supported by Log Service are as follows.

|Function|Meaning|Example|
|:-------|:------|:------|
|`current_date`|Returns the current date.|`latency>100| select current_date`|
|`current_time`|Returns the current time.|`latency>100| select current_time`|
|`current_timestamp`|Returns the result combined by current\_date and current\_time.|`latency>100| select current_timestamp`|
|`current_timezone()`|Returns the time zone.|`latency>100| select current_timezone()`|
|`from_iso8601_timestamp(string)`|Converts an iso8601 time to a date with time zone.|`latency>100| select from_iso8601_timestamp(iso8601)`|
|`from_iso8601_date(string)`|Converts an iso8601 time to a date.|`latency>100| select from_iso8601_date(iso8601)`|
|`from_unixtime(unixtime)`|Converts a UNIX time to a timestamp.|`latency>100| select from_unixtime(1494985275)`|
|`from_unixtime(unixtime,string)`|Converts a UNIX time to a timestamp by using the string as the time zone.|`latency>100| select from_unixtime (1494985275,'Asia/Shanghai')`|
|`localtime`|Returns the current time.|`latency>100| select localtime`|
|`localtimestamp`|Returns the current timestamp.|`latency>100| select localtimestamp`|
|`now()`|Equivalent to `current_timestamp` .|-|
|`to_unixtime(timestamp)`|Timestamp is converted into unixtime.|`*| select to_unixtime('2017-05-17 09:45:00.848 Asia/Shanghai')`|

## Time Function {#section_jky_3mq_tdb .section}

**MySQL time format**

Log Service supports the MySQL time format such as %a, %b, and %y.

|Function|Meaning|Example|
|:-------|:------|:------|
| `date_format(timestamp, format)` |Converts the timestamp into a format representation.|`latency>100`| `select date_format (date_parse('2017-05-17 09:45:00','%Y-%m-%d %H:%i:%S'), '%Y-%m-%d') group by method`|
| `date_parse(string, format)` |Parses a string into a timestamp by using the format.|`latency>100`|`select date_parse('2017-05-17 09:45:00','%Y-%m-%d %H:%i:%S') group by method`|

|Format|Description|
|:-----|:----------|
|%a|The abbreviation of a day in a week, such as Sun and Sat.|
|%b|The abbreviation of a month, such as Jan and Dec.|
|%c|Month, in the numeric type: 1 to 12.|
|%D|The day of each month with a suffix, such as 0th, 1st, 2nd, and 3nd.|
|%d|The day of each month, which is in decimal format and in the range of 01 to 31.|
|%e|The day of each month, which is in decimal format and in the range of 1 to 31.|
|%H|The hour in 24-hour format.|
|%h|The hour in 12-hour format.|
|%I|The hour in 12-hour format.|
|%i|Minutes, which is in the type of number and in the range of 00 to 59.|
|%j|The day of each year, which is in the range of 001 to 366.|
|%k|Hour, which is in the range of 0 to 23.|
|%l|Hour, which is in the range of 1 to 12.|
|%M|The English expression of a month, which is in the range of January to December.|
|%m|Month, which is in the numeric format and in the range 01 to 12.|
|%p|AM or PM.|
|%r|Time in 12-hour format: `hh:mm:ss AM/PM`.|
|% S|Seconds, in the range of 00 to 59.|
|%s|Seconds, in the range of 00 to 59.|
|%T|Time, in the 24-hour format: `hh:mm:ss`.|
|%U|The week number of each year. Sunday is the first day of each week. The value range is from 00 to 53.|
|%u|The week number of each year. Monday is the first day of each week. The value range is 00 to 53.|
|%V|The week number of each year. Sunday is the first day of each week. The value range is 01 to 53. Use this format in conjunction with %X.|
|%v|The week number of each year. Monday is the first day of each week. The value range is 01 to 53. Use this format in conjunction with %x.|
|%W|The name of each day of a week, in the range of Sunday to Saturday.|
|%w|The day of the week, in the range of 0 to 6. Sunday is the day 0.|
|%Y|The year in the 4-digit format.|
|%y|The year in the 2-digit format.|
|%%|%Escape character|

**Time period alignment functions**

Log Service supports time period alignment functions, which can be aligned according to seconds, minutes, hours, days, months, and years. Time period alignment functions are usually used when statistics are made according to time.

**Function syntax**:

`date_trunc(unit, x)`

**Parameters**:

The optional values for Unit are as follows \(x is `2001-08-22 03:04:05.000`\)：

|Unit|Converted result|
|:---|:---------------|
|second|2001-08-22 03:04:05.000|
|minute|2001-08-22 03:04:00.000|
|hour|2001-08-22 03:00:00.000|
|day|2001-08-22 00:00:00.000|
|week|2001-08-20 00:00:00.000|
|month|2001-08-01 00:00:00.000|
|quarter|2001-07-01 00:00:00.000|
|year|2001-01-01 00:00:00.000|

x can be of the timestamp type or UNIX  time type.

date\_trunc can only make statistics every fixed time period. If you need to make statistics according to flexible time dimension, for example, make the statistics every five minutes, perform GROUP BY according to the mathematical modulus method.

```
* | SELECT count(1) as pv,  __time__ - __time__% 300 as minute5groupby minute5 limit 100
```

The `%300` indicates to make the modulus and alignment every five minutes.

## Date function example {#section_txq_jmq_tdb .section}

The following is a comprehensive example using the time format:

```
*|select  date_trunc('minute' ,  __time__)  as t,
       truncate (avg(latency) ) ,
       current_date  
       group by t
       order by t desc 
       limit 60
```

## Interval functions {#section_frx_vvh_t2b .section}

Interval functions are used to perform interval related calculation. For example, add or delete an interval in the date, or calculate the time between two dates.

|Function|Description|Example|
|:-------|:----------|:------|
|``date_add`(unit, value, timestamp)`|Add`value``unit` to `timestamp`. To perform minus calculation, use a negative `value`.|`date_add('day', -7, '2018-08-09 00:00:00')` indicates seven days before August 9.|
|`date_diff(unit, timestamp1, timestamp2)`|The number of `unit` between `timestamp1` and`timestamp2`.|`date_diff('day', '2018-08-02 00:00:00', '2018-08-09 00:00:00') = 7`|

The function supports the following interval units:

|Unit|Description|
|:---|:----------|
|millisecond|Milliseconds|
|second|Seconds|
|minute|Minutes|
|hour|Hours|
|day|Days|
|week|Weeks|
|month|Months|
|quarter|A quarter, namely, three months.|
|year|Years|

