# Configure the time format {#concept_pdk_djc_wdb .concept}

Each log in Log Service has a timestamp that records the log generation time. When collecting log data from your log files, Logtail must extract the timestamp string of each log and parse it into a timestamp. Therefore, you need to specify a timestamp format for parsing.

Logtail for Linux supports all time formats provided by the strftime function. Logtail only parses and uses the timestamp strings that can be expressed in the log formats defined by the strftime function.

**Note:** 

-   The log timestamp is accurate to seconds. Therefore, you only need to configure the time format to seconds, without the need for other information such as milliseconds or microseconds.

-   In addition, you only need to configure the time field, instead of other information.


## Common log time formats supported by Logtail {#section_wsg_dby_dgb .section}

The timestamp strings of logs have diverse formats. To facilitate configuration, Logtail supports multiple log time formats, as described in the following table.

|Format|Description|Example|
|:-----|:----------|:------|
|%a|The abbreviation of a day in a week.|Fri|
|%A|The full name of a day in a week.|Friday|
|%b|The abbreviation of a month.|Jan|
|%B|The full name of a month.|January|
|%d|The day in a month, in decimal format. Valid values: \[01, 31\].|07 or 31|
|%h|The abbreviation of a month, which is the same as `%b`.|Jan|
|%H|The hour in 24-hour format.|22|
|%I|The hour in 12-hour format.|11|
|%m|The month in decimal format.|08|
|%M|The minutes in decimal format. Valid values: \[00, 59\].|59|
|%n|The line break.|A line break|
|%p|The abbreviation of a period in 12-hour format.|AM or PM|
|%r|The time in 12-hour format, which is equivalent to `%I:%M:%S %p`.|11:59:59 AM|
|%R|The time expressed in hour and minutes, which is equivalent to `%H:%M`.|23:59|
|%S|The seconds in decimal format. Valid values: \[00, 59\].|59|
|%t|The tab character.|The tab character|
|%y|The year \(excluding the century\) in decimal format. Valid values: \[00, 99\].|04 or 98|
|%Y|The year in decimal format.|2004 or 1998|
|%C|The century in decimal format. Valid values: \[00, 99\].|16|
|%e|The day in a month, in decimal format. Valid values: \[1, 31\]. Prefix a space to a single-digit number.|7 or 31|
|%j|The day in a year, in decimal format. Valid values: \[00, 366\].|365|
|%u|The day in a week, in decimal format. Valid values: \[1, 7\]. A value of 1 indicates Monday.|2|
|%U|The week in a year, where Sunday is the first day of each week. Valid values: \[00, 53\].|23|
|%V|The week in a year, where Monday is the first day of each week. If the week at the beginning of January contains four or more days, this week is the first week of the year. If this week contains less than four days, the next week is considered as the first week of the year. Valid values: \[01, 53\].|24|
|%w|The day in a week, in decimal format. Valid values: \[0, 6\]. A value of 0 indicates Sunday.|5|
|%W|The week in a year, where Monday is the first day of each week. Valid values: \[00, 53\].|23|
|%c|The standard date and time.|To specify more information such as the long date or short date, you can use the preceding supported formats for more precise expression.|
|%x|The standard date.|To specify more information such as the long date or short date, you can use the preceding supported formats for more precise expression.|
|%X|The standard time.|To specify more information such as the long date or short date, you can use the preceding supported formats for more precise expression.|
|%s|The Unix timestamp.|1476187251|

## Example {#section_c5j_2by_dgb .section}

The following table lists the common log time formats, examples, and corresponding time expressions.

|Log time format|Example|Time expression|
|:--------------|:------|:--------------|
|Custom|2017-12-11 15:05:07|%Y-%m-%d %H:%M:%S|
|Custom|\[2017-12-11 15:05:07.012\]|\[%Y-%m-%d %H:%M:%S\]|
|RFC822|02 Jan 06 15:04 MST|%d %b %y %H:%M|
|RFC822Z|02 Jan 06 15:04 -0700|%d %b %y %H:%M|
|RFC850|Monday, 02-Jan-06 15:04:05 MST|%A, %d-%b-%y %H:%M:%S|
|RFC1123|Mon, 02 Jan 2006 15:04:05 MST|%A, %d-%b-%y %H:%M:%S|
|RFC3339|2006-01-02T15:04:05Z07:00|%Y-%m-%dT%H:%M:%S|
|RFC3339Nano|2006-01-02T15:04:05.999999999Z07:00|%Y-%m-%dT%H:%M:%S|

