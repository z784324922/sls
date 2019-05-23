# Logtail collection process {#concept_bjz_3hp_1fb .concept}

The Logtail client performs the following six steps to collect logs from your server: monitor files, read files, process logs, filter files, aggregate logs, and send logs.

After you install the Logtail client on your server and configure a Logtail Config, Logtail starts collecting logs to Log Service. The log collection process involves the following steps:

1.  [Monitor files](#)
2.  [Read files](#)
3.  [Process logs](#)
4.  [Filter logs](#)
5.  [Aggregate logs](#)
6.  [Send logs](#)

**Note:** After a Logtail Config is configured for a machine group, unmodified logs on a server in the machine group will be regarded as historical files. However, Logtail does not collect historical files. If you want to collect historical logs, see [Import history logs](reseller.en-US/User Guide/Logtail collection/Text logs/Import history logs.md).

## Monitor files {#section_u2b_x5s_cfb .section}

After you install the Logtail client on your server and configure a Logtail Config based on data sources, the Logtail Config sends logs to Logtail in real time. Then, Logtail uses the Logtail Config to monitor files.

1.  Specifically, Logtail scans the log directories and files that conform to the specified file naming conventions layer by layer according to the configured log path and maximum monitoring directory depth.

    To ensure the efficiency and stability of log collection, Logtail registers event monitoring for the collection directory \(namely, the [Inotify](http://man7.org/linux/man-pages/man7/inotify.7.html) directory on Linux or the [ReadDirectoryChangesW](https://docs.microsoft.com/zh-cn/windows/desktop/api/winbase/nf-winbase-readdirectorychangesw) directory on Windows\) and performs periodic polling.

2.  If the monitoring results show that unmodified log files that conform to the file naming conventions exist in the specified directory, Logtail will not collect the files. If there are modified log files, a collection process will be triggered and Logtail will read the files.

## Read files {#section_u34_x5s_cfb .section}

Logtail starts to read the modified files.

1.  Logtail checks the size of a file when reading the file for the first time.
    -   If the file size is smaller than 1 MB, Logtail reads the file from the beginning.
    -   If the file size is larger than 1 MB, Logtail reads the last 1-MB content of the file.
2.  If Logtail has read the file before, Logtail reads the file from the last checkpoint.
3.  Logtail can read up to 512 KB at a time. Therefore, you need to limit the log size to 512 KB.

**Note:** If you have modified the time on your server, you need to manually restart Logtail. Otherwise, the log generation time will be incorrect and some logs may be mistakenly discarded.

## Process logs {#section_f41_y5s_cfb .section}

Logtail splits a log into lines, parses the log, and confirms the correctness of the time field settings.

1.  **Line splitting**:

    If a **line start regular expression** has been specified in the Logtail Config, Logtail will split the log into lines according to the line start settings. In this case, Logtail processes the lines as multiple logs. If no line start regular expression has been specified, Logtail regards a data block as a log and processes it.

2.  **Parsing**:

    Logtail uses the Logtail Config to parse the log content based on specified rules, such as regular expressions, delimiters, and JSON arrays.

    **Note:** An excessively complex regular expression may lead to an abnormally high CPU usage. Therefore, we recommend that you use an efficient regular expression.

3.  **Parsing failure handling**:

    Depending on whether the [discarding logs with parsing failure](reseller.en-US/User Guide/Logtail collection/Text logs/Collect text logs.md#table_eq2_ccc_wdb) function is enabled in the Logtail Config, you can handle logs with parsing failure as follows:

    -   If the function is enabled, Logtail discards the log and reports a corresponding error.
    -   If the function is disabled, you need to upload the original log with its key of **raw\_log** and Value of the log content.
4.  **Time field settings**:

    -   If the time field is not set, the log generation time is the current parsing time.
    -   If the time field is set and the log generation time is:
        -   Less than 12 hours from the current time, Logtail extracts the time from the parsed time field.
        -   More than 12 hours from the current time, Logtail discards the log and reports a corresponding error.

## Filter logs {#section_k14_y5s_cfb .section}

Logtail filters logs according to the [filter settings](reseller.en-US/User Guide/Logtail collection/Text logs/Collect text logs.md#) in the Logtail Config.

-   If the filter is not set, Logtail will not filter logs but directly aggregates logs.
-   If the filter is set, Logtail will traverse and verify all fields in each log.
    -   Logtail collects logs that conform to filter settings, that is, all fields in filter settings can be found in the log and all the fields conform to the setting requirements.
    -   Logtail does not collect logs that do not conform to filter settings.

## Aggregate logs {#section_pvz_y5s_cfb .section}

Logtail sends log data to Log Service. To reduce the number of network requests, Logtail caches the logs for some time. Then, Logtail aggregates and packages the logs to send them to Log Service.

During caching, Logtail will immediately package logs and send them if any of the following conditions is met:

-   Log aggregation lasts more than 3s.
-   There are more than 4.096 logs to be aggregated.
-   The target log size exceeds 512 KB.

## Send logs {#section_rf4_z5s_cfb .section}

Logtail sends the aggregated log to Log Service. You can set the startup parameters `max_bytes_per_sec` and `send_request_concurrency` by following the instructions provided in [Configure startup parameters](reseller.en-US/User Guide/Logtail collection/Install/Configure startup parameters.md#) to adjust the log sending rate and the maximum number of logs that can be concurrently sent. In this case, Logtail ensures that the preset values are not exceeded.

If the log sending fails, Logtail automatically retries or quits the task according to the corresponding error message.

|Error message|Description|Handling method|
|:------------|:----------|:--------------|
|Error code: 401|The Logtail client does not have the permission to collect data.|Logtail discards the log package.|
|Error code: 404|The project or Logstore specified in the Logtail Config does not exist.|Logtail discards the log package.|
|Error code: 403|The Shard quota exceeds the upper limit.|Wait for 3s and try again.|
|Error code: 500|An error occurs on the server.|Wait for 3s and try again.|
|Network expiration|A network connection error occurs.|Wait for 3s and try again.|

