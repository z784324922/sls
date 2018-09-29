# Text logs - Configure time format {#concept_pdk_djc_wdb .concept}

As described in the core concepts of Log Service, each log in Log Service has a timestamp when this log happened. Logtail must extract the timestamp string of each log and parse it into a timestamp when collecting logs from your log files. Therefore, you must specify the timestamp format of the log for Logtail. 

In Linux, Logtail supports all time formats provided by the strftime function. Logtail can parse and use the timestamp strings that can be expressed in the log formats defined by the strftime function.

In reality, the timestamp strings of logs have multiple formats. To make configuration easier, Logtail supports the following common log time formats.

|Format|Meaning|Example |
|:-----|:------|:-------|
|%a|The abbreviation of a day in a week.|Fri|
|%A|The full name of a day in a week. |Friday|
|%b|The abbreviation of a month. |Jan|
|%B|The full name of the month.|January|
|%d|The day of the month in decimal format \[01,31\].|07, 31|
|%h|The abbreviation of a month. Same as `%b`.|Jan|
|%H|The hour in 24-hour format. |22|
|%I|The hour in 12-hour format. |11|
|%m|The month in decimal format. |08|
|%M|The minute in decimal format \[00, 59\].|59|
|%n|A line break. |A line break|
|%p|AM or PM locally. |AM/PM|
|%r|Time in 12-hour format, which is equivalent to `%I:%M:%S %p`. |11:59:59 AM|
|%R|Time expressed in hour and minute, which is equivalent to `%H:%M`.|23:59|
|% S|The second in decimal format \[00, 59\].|59|
|%t|Tab character.|Tab character|
|%y|The year without century in decimal format \[00, 99\].|04; 98|
|%Y|The year in decimal format. |2004; 1998|
|%z|The time zone or its abbreviation.|-07:00, +0800|
|%C|The century in decimal format \[00, 99\].|16|
|%e|The day of the month in decimal format \[1, 31\].  A single digit is preceded by a space.|7, 31|
|%j|The day of the year in decimal format \[00, 366\].|365|
|%u|The day of the week in decimal format \[1, 7\]. 1 represents Monday.|2|
|%U|The week number of the year \(Sunday as the first day of the week\)  \[00, 53\].|23|
|%V|The week number of the year \(Monday as the first day of the week\) \[01, 53\].  If the week at the beginning of January has no less than four days, this week is the first week of the year. Otherwise, the next week is considered as the first week of the  year.|24|
|%w|The day of the week in decimal format \[0, 6\]. 0 represents Sunday. |5|
|%W|The week number of the year \(Monday as the first day of the week\)  \[00,53\].|23|
|%c|Standard date and time representation.|To specify more information such as long date and short date, we recommend that you use the preceding supported formats for more precise expression.|
|%x|Standard date representation.|To specify more information such as long date and short date, we recommend that you use the preceding supported formats for more precise expression.|
|%X|Standard time representation.|To specify more information such as time, we recommend that you use the preceding supported formats for more precise expression.|
|%s|Unix timestamp.|1476187251|

