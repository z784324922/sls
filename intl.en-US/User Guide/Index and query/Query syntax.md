# Query syntax {#concept_tnd_1jq_zdb .concept}

To help you query logs more effectively, Log Service provides a set of query syntax to express query conditions.  You can specify query conditions by using the [GetLogs](../../../../reseller.en-US/API Reference/Logstore related APIs/GetLogs.md) and [GetHistograms](../../../../reseller.en-US/API Reference/Logstore related APIs/GetHistograms.md) interfaces in Log Service API  or on the query page of the Log Service console.  This document introduces the syntax of query conditions in details.

## Index types {#section_t32_m4v_1bb .section}

Log Service supports creating an index for the LogStore in the following methods:

-   Full text index: Query the entire line of logs as a whole without differentiating key and value.
-   Key/value index: Query logs after specifying a key. For example, FILE:app and Type:action. All the strings with the specified key are queried.

## Syntax keywords {#section_u2r_p4v_1bb .section}

LogSearch query conditions support the following keywords.

|Name|Meaning|
|:---|:------|
|and|Binary operator. Format: query1 and  query2. Indicates the intersection of the query results of query1 and query2.  With no syntax keyword among  multiple words, the relation is and by default.|
|or|Binary operator. Format:`query1 or query2`. Indicates the union of the query results of `query1` and `query2` .|
|not|Binary operator. Format: `query1 not query2`. Indicates a result that matches `query1`  and does not match `query2`, which is equivalent to `query1–query2`.  If only `not query1` exists, it indicates to select the results excluding `query1` from all the logs.|
|\( , \)|Parentheses \(\) are used to merge one or more sub-queries into one query to increase the priority of the query in the parentheses \(\).|
|:|Used to query the key-value pairs. `term1:term2` makes up a key-value pair.  If the key or value  contains reserved characters such as spaces and colons \(:\), use quotation marks \(“\) to enclose the entire key or value.|
|“|Converts a keyword to a common query character.   Each term enclosed in quotation marks \(“\) can be queried and is not be considered as a syntax keyword. Or all the terms enclosed in quotation marks \(“\) are regarded  as a whole in the key-value query.|
|\\|Escape character.  Used to escape quotation marks. The escaped quotation marks indicate the symbols themselves, and they cannot be used as escape characters, such as `"\""`.|
|||The pipeline operator indicates more calculations based on the previous calculation, such as query1 | timeslice 1h | count.|
|timeslice|The time-slice operator indicates how long the data is calculated as a whole. Timeslice 1h, 1m, 1s indicates 1 hour, 1  minute, and 1 second respectively. For example, query1 | timeslice 1h | count represents the query query condition, and returns to the total number of hours divided by 1 hour.|
|count|The count operator indicates the number of log lines.|
|\*|Fuzzy query keyword. Used to replace zero or multiple characters. For example, the query results of `que*` start with `que`.**Note:** At most 100 query results can be returned.

|
|?|Fuzzy query keyword. Used to replace one character. For example, the query results of `qu? ry` start with `qu`, end with `ry`, and have a character in the middle.|
|`__topic__`|Topic data query. With the new syntax, you can query the data of zero or multiple topics  in the query. For example, `__topic__:mytopicname`.|
|`__tag__`|Query a tag value in a tag key. For example, `__tag__:tagkey:tagvalue`.|
|Source|Query the data of an IP. For example, `source:127.0.0.1`.|
|\>|Query the logs with a field value greater than a specific number. For example, `latency > 100`.|
|\>=|Query the logs with a field value greater than or equal to a specific number. For example, `latency >= 100`.|
|<|Query the logs with a field value less than a specific number. For example, `latency < 100`.|
|<=|Query the logs with a field value less than or equal to a specific number. For example, `latency <= 100`.|
|=|Query the logs with a field value equal to a specific number. For example, `latency = 100`.|
|in|Query the logs with a field staying within a specific range. Braces \(\[\]\) are used to indicate closed intervals and parentheses \(\(\)\) are used to indicate open intervals. Enclose two numbers in braces \(\[\]\) or parentheses \(\(\)\) and separate the numbers with several spaces.  For example, `latency in  [100 200]` or `latency in (100 200]]`.|

**Note:** 

-   Syntax keywords are case-insensitive.
-   Priorities of syntax keywords are sorted in descending order as follows: `: > " > ( ) > and not > or`.
-   Log Service reserves the right to use the following keywords: `sort asc desc group by avg sum min max limit`. To use these keywords, enclose them in quotation marks \(“\).
-   If both the full text index and key/value index are configured and have different word segmentation characters, data cannot be queried using the full text query method.
-   Set the column type as double or long before performing a numeric query. If the column type is not set or the syntax used for the numeric range query is incorrect, Log Service translates the query condition as a full text index, which may lead to an unexpected result.
-   If you change the column type from text to numeric, only the = query is supported for the data before this change.

## Query examples {#section_k4z_jsc_ry .section}

1.  Logs that contain a and b at the same time: `a and b` or `a b`.
2.  Logs that contain a or b: `a or b`.
3.  Logs that contain a but do not contain b: `a not b`.
4.  All the logs that do not contain a: `not a`.
5.  Query the logs that contain a and b, but do not contain c: `a and b not c`.
6.  Logs that contain a or b and must contain c: `(a or b ) and c`.
7.  Logs that contain a or b, but do not contain c: `(a or b ) not c`.
8.  Logs that contain a and b and may contain c: `a and b or c`.
9.  Logs whose FILE field contains apsara: `FILE:apsara`. 
10. Logs whose FILE field contains apsara and shennong: `FILE:"apsara shennong"`,  `FILE:apsara FILE: shennong` or `FILE:apsara and FILE:shennong.`
11. Logs containing and: `and`.
12. Logs with the FILE field containing apsara or shennong: `FILE:apsara or FILE:shennong`.
13. Logs with the file info field containing apsara: `"file info":apsara`.
14. Logs that contain quotation marks \(“\): `\"`.
15. Query all the logs starting with shen: `shen*`.
16. Query all the logs starting with shen in the FILE field: `FILE:shen*`.
17. Query all the logs starting with shen, ending with ong, and having a character in the middle: `shen? ong.`
18. Query the logs starting with shen and aps: `shen* and aps*`.
19. Query the logs starting with shen every 20 minutes: `shen*| timeslice 20m | count` .
20. Query all the data in the topic1 and topic2: `__topic__:topic1 or __topic__ : topic2`.
21. Query all the data of the tagvalue2 in the tagkey1: `__tag__ : tagkey1 : tagvalue2`.
22. Query all the data with a latency greater than or equal to 100 and less than 200: `latency >=100 and latency < 200` or `latency in [100 200)`.
23. Query all the requests with a latency greater than 100: `latency > 100`.
24. Query the logs that do not contain spider and do not contain opx in http\_referer: `not spider not bot not http_referer:opx`.
25. Query logs with the empty cdnIP field:  `cdnIP:""`.
26. Query logs without cdnIP field:`not cdnIP:*`.
27. Query logs with the cdnIP field: `cdnIP:*`.

## Specified or cross-topic query  {#section_jjw_zsc_ry .section}

Each LogStore can be divided into one or more subspaces by the topic. During therfhfrg query, specifying topics can limit the query range so as to increase the speed.  Therefore, we recommend that you use topic to divide the LogStore  if you have a secondary classification requirement for the LogStore.

With one or more topics specified, the query is only performed in the topics that meet the conditions.  However, if no topic is specified, data of all the topics is queried by default.

For example, use topic to classify logs with the different domain names:  

![](images/5523_en-US.png "Log topic")

Topic query syntax:

-   Data of all the topics can be queried. If no topic is specified in the query syntax and parameter, data of all the topics is queried.
-   Supports query by topic. The query syntax is `__topic__:topicName`.  The old mode \(specify the topic in the URL parameter\)  is still supported.
-   Multiple topics can be queried. For example, `__topic__:topic1 or __topic__:topic2` indicates the union query of data from Topic1 and Topic2 .

## Fuzzy search {#section_zst_xlv_m2b .section}

Log Service support fuzzy search. Specify a word within 64 characters, and add fuzzy search keywords such as `*` and `?` in the middle or in the end of the word. 100 eligible words will be searched out, in the meantime, all the logs eligible and contain the 100 words will be returned.

**Limits**：

-   Prefix must be specified when query logs, that is, the word can not begin with `*` and `?`  .
-   Precise the specified word, you will get a more accurate result.
-   Fuzzy search cannot be used to search for words that exceeds 64 characters. It is recommended that you specified a word under 64 characters.

