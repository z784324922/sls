# Search, analysis, and visualization {#concept_qjd_m3h_hfb .concept}

|Function|Item|Limit|Note|
|:-------|:---|:----|:---|
|Query|Number of keywords|The number of conditions specified for querying words besides Boolean logical operators. You can query up to 30 keywords each time.|For example, "a and b or c and d...".|
|The length of a single value.|The maximum length of a single value is 10 KB. The excess part of the value is not queried.|If the length of a single value is greater than 10 KB, the log might not be found through keywords, but the data is still complete.|
|Single project concurrency|The number of single project concurrency is up to 100.|-|
|Number of entries of returned query results|By default, a maximum of 100 entries of query results are returned each time.|You can read the full query results by turning pages.|
|Single Log content display|For logs exceeding 10,000 characters, Log service only processes the first 10,000 characters using the DOM word segmentation due to Web browser performance.|If a log contains more than 10,000 characters, the system will display a corresponding prompt.|
|SQL analysis|Maximum length of a single value|The maximum length of a single value is 2 KB. The excess part of the value is not queried.|Query results might not be accurate when the limit is exceeded, but the data is still complete.|
|Single project concurrency|The number of single project concurrency is up to 15.|-|
|Number of entries of results in each analysis| -   By default, 100 log entries are displayed after an SQL statement is executed.
-   The returned result of each analysis can be log entries of 100 MB or 100,000 log entries at most.

 | You can use the limit parameter to specify the number of returned log entries.

 You can turn over the page to view the returned results.

 |
|Field size|The size of a field used in a statement can be up to 2 KB.|The system truncates a field larger than 2 KB.|
|Double precision|A double type can be shown by using up to 52 digits. If you use more than 52 digits to show a double type, the precision will be affected.|For example, 0.1+0.2 is shown as 0.30000000000000004. We recommend that you use round\(0.1+0.2,2\) to take two significant values.|

