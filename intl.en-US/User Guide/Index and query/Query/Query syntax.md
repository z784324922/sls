# Query syntax {#concept_tnd_1jq_zdb .concept}

To help you query logs more effectively, Log Service provides a set of query syntax to express query conditions.

## Query methods {#section_t32_m4v_1bb .section}

After [enabling the index function and configuring indexes](reseller.en-US/User Guide/Index and query/Enable and set indexes.md), you can enter a query and analysis statement on the log query page to query logs.

The query statement is the first part of a query analysis statement, and is used to specify rules for filtering logs and logs that conform to the query condition. Both full text query and key/value query are supported.

-   Full text query

    In a full text query, the entire log is regarded as a special key/value pair, in which the log content is regarded as the value. You can specify keywords for a full text query. Specifically, you can specify the keywords which must be included in or excluded from the query condition. The log that meets the specified query condition will be returned as a query result.

    Log Service also supports phrase query and fuzzy query.

    -   Common full text query: You need to specify a keyword and rule. Logs that contain the keyword and conform to the rule will be returned as query results.

        For example, `a and b` indicates that the query results must contain both the keywords `a` and `b`.

    -   Phrase query: If the target phrase contains a space, you can enclose the phrase with double quotation marks \(""\). In this case, the phrase will be regarded as a complete keyword for log query.

        For example. `"http error"` indicates that the query results must contain `http error`.

    -   Fuzzy query: You can specify a partial word up to 64 characters in length, and add a fuzzy query keyword \(`*` or `?`\) at the middle or end of the word. By doing so, up to 100 words that meet the query condition among all logs will be queried, and the logs corresponding to the 100 words will be returned as query results.

        For example, `addr?`indicates that Log Service needs to query up to 100 words starting with `addr`, and return the corresponding logs.

-   Key/value query

    After configuring indexes for fields, you can query the name or content of a specific field. For fields of the double or longtype, you can also specify the value range for query. For example, the key/value query statement `Latency>5000 and Method:Get* and not Status:200` indicates that the query results must meet the following conditions:

    -   The value of `Latency` must be greater than 5000.
    -   The `Method` field must start with `Get`.
    -   The value of the `Status` field is not 200.
    You can perform various types of basic query and combined query according to the data types set for field indexes. For more information about key/value query examples, see [Index data type overview](reseller.en-US/User Guide/Index and query/Data type of index/Overview.md).


## Precautions {#section_w1r_zww_cfb .section}

-   When both full text query and key/value query are performed, if the delimiters set for the two query methods are different, the delimiter set for key/value query is used, and the query results of full text query become invalid.
-   You can query fields with a specified value range only after setting the data type of the fields to double or long. If the field data type is unspecified or the syntax for querying value ranges is incorrect, Log Service determines that the query condition is for full text query. This may return unexpected query results.
-   If the date type of a field is changed from text to numeric, the data collected before the change only support `=` query.

## Operators {#section_u2r_p4v_1bb .section}

The following operators can be used in query statements:

|Operator|Description|
|:-------|:----------|
|and|Binary operator. Format: query1 and query2. Indicates the intersection of the query results of query1 and query2. With no syntax keyword among multiple words, the relation is and by default.|
|or|Binary operator. Format: `query1 or query2`. Indicates the union of the query results of `query1` and `query2` .|
|not|Binary operator. Format: `query1 not query2`. Indicates a result that matches `query1` and does not match `query2`, which is equivalent to `query1–query2`. If only `not query1` exists, it indicates to select the results excluding `query1` from all the logs.|
|\( , \)|Parentheses \(\) are used to merge one or more sub-queries into one query condition to increase the priority of the query in the parentheses \(\).|
|:|Used to query the key-value pairs. `term1:term2` makes up a key-value pair. If the key or value contains reserved characters such as spaces and colons \(:\), use quotation marks \(“\) to enclose the entire key or value.|
|“|Converts a keyword to a common query character. Each term enclosed in quotation marks \(“\) can be queried and is not be considered as a syntax keyword. Or all the terms enclosed in quotation marks \(“\) are regarded as a whole in the key-value query.|
|\\|Escape character. Used to escape quotation marks. The escaped quotation marks indicate the symbols themselves, and they cannot be used as escape characters, such as `"\""`.|
|||The pipeline operator indicates more calculations based on the previous calculation, such as query1 | timeslice 1h | count.|
|timeslice|The time-slice operator indicates how long the data is calculated as a whole. Timeslice 1h, 1m, 1s indicates 1 hour, 1 minute, and 1 second respectively. For example, query1 | timeslice 1h | count represents the query query condition, and returns to the total number of hours divided by 1 hour.|
|count|The count operator indicates the number of log lines.|
|\*|Fuzzy query keyword. Used to replace zero or multiple characters. For example, the query results of `que*` start with `que`. **Note:** At most 100 query results can be returned.

 |
|?|Fuzzy query keyword. Used to replace one character. For example, the query results of `qu? ry` start with `qu`, end with `ry`, and have a character in the middle.|
|`__topic__`|Topic data query. You can query the data of zero or multiple topics in the query. For example, `__topic__:mytopicname`.|
|`__tag__`|Query a tag value in a tag key. For example, `__tag__:tagkey:tagvalue`.|
|Source|Query the data of an IP. For example, `source:127.0.0.1`.|
|\>|Query the logs with a field value greater than a specific number. For example, `latency > 100`.|
|\>=|Query the logs with a field value greater than or equal to a specific number. For example, `latency >= 100`.|
|<|Query the logs with a field value less than a specific number. For example, `latency < 100`.|
|<=|Query the logs with a field value less than or equal to a specific number. For example, `latency <= 100`.|
|=|Query the logs with a field value equal to a specific number. For example, `latency = 100`.|
|in|Query the logs with a field staying within a specific range. Braces \(\[\]\) are used to indicate closed intervals and parentheses \(\(\)\) are used to indicate open intervals. Enclose two numbers in braces \(\[\]\) or parentheses \(\(\)\) and separate the numbers with several spaces. For example, `latency in [100 200]` or `latency in (100 200]]`.|

**Note:** 

-   Operators are case-insensitive.
-   Priorities of operators are sorted in descending order as follows: `:`\>`"`\>`( )`\>`and`\>`not`\>`or`.
-   Log Service reserves the right to use the following operators: `sort`, `asc`, `desc`, `group by`, `avg`, `sum`, `min`, `max`, and `limit`.To use these keywords, enclose them in quotation marks \(""\).

## Query examples {#section_k4z_jsc_ry .section}

|Query demand|Example|
|:-----------|:------|
|Logs that contain a and b at the same time|`a and b` or `a b`|
|Logs that contain a or b|`a or b`|
|Logs that contain a but do not contain b|`a not b`|
|All the logs that do not contain a|`not a`|
|Logs that contain a and b, but do not contain c|`a and b not c`|
|Logs that contain a or b and must contain c|`(a or b ) and c`|
|Logs that contain a or b, but do not contain c|`(a or b ) not c`|
|Logs that contain a and b and may contain c|`a and b or c`|
|Logs whose FILE field contains apsara|`FILE:apsara`|
|Logs whose FILE field contains apsara and shennong|`FILE:"apsara shennong"`, `FILE:apsara FILE: shennong`, or `FILE:apsara and FILE:shennong`|
|Logs that contain and|`and`|
|Logs with the FILE field containing apsara or shennong|`FILE:apsara or FILE:shennong`|
|Logs with the file info field containing apsara|`"file info":apsara`|
|Logs that contain quotation marks \(""\)|`\"`|
|All logs starting with shen|`shen*`|
|All logs starting with shen in the FILE field|`FILE:shen*`|
|All logs with the FILE field of shen\*|`FILE: "shen*"`|
|Logs starting with shen, ending with ong, and having a character in the middle|`shen?ong`|
|Logs starting with shen and aps|`shen* and aps*`|
|Logs starting with shen every 20 minutes|`shen*| timeslice 20m | count`|
|All data in the topic1 and topic2|`__topic__:topic1 or __topic__ : topic2`|
|All data of the tagvalue2 in the tagkey1|`__tag__ : tagkey1 : tagvalue2`|
|All data with a latency greater than or equal to 100 and less than 200|`latency >=100 and latency < 200` or `latency in [100 200)`|
|All requests with a latency greater than 100|`latency > 100`|
|Logs that do not contain spider and do not contain opx in http\_referer|`not spider not bot not http_referer:opx`|
|Logs with the empty cdnIP field|`not cdnIP:""`|
|Logs without cdnIP field|`not cdnIP:*`|
|Logs with the cdnIP field|`cdnIP:*`|

## Specified or cross-topic query {#section_jjw_zsc_ry .section}

Each LogStore can be divided into one or more subspaces by the topic. During therfhfrg query, specifying topics can limit the query range so as to increase the speed. Therefore, we recommend that you use topic to divide the LogStore if you have a secondary classification requirement for the LogStore.

With one or more topics specified, the query is only performed in the topics that meet the conditions. However, if no topic is specified, data of all the topics is queried by default.

For example, use topic to classify logs with the different domain names:

![](images/5523_en-US.png "Log topic")

Topic query syntax:

-   Data of all the topics can be queried. If no topic is specified in the query syntax and parameter, data of all the topics is queried.
-   Supports query by topic. The query syntax is `__topic__:topicName`. The old mode \(specify the topic in the URL parameter\) is still supported.
-   Multiple topics can be queried. For example, `__topic__:topic1 or __topic__:topic2` indicates the union query of data from Topic1 and Topic2 .

## Fuzzy search {#section_zst_xlv_m2b .section}

Log Service support fuzzy search. Specify a word within 64 characters, and add fuzzy search operators such as `*` and `?` in the middle or in the end of the word. 100 eligible words will be searched out, in the meantime, all the logs eligible and contain the 100 words will be returned.

**Limits**:

-   Prefix must be specified when query logs, that is, the word can not begin with `*` and `?` .
-   Precise the specified word, you will get a more accurate result.
-   Fuzzy search cannot be used to search for words that exceeds 64 characters. It is recommended that you specified a word under 64 characters.

