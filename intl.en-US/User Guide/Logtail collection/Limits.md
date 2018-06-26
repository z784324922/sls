# Limits {#concept_x1y_p3q_zdb .concept}

|Item|Capabilities and limits|
|:---|:----------------------|
|File encoding|Log files encoded in UTF-8 and GBK are supported.  Log files encoded in other formats result in undefined behaviors such as gibberish and data loss. We recommend that you use UTF-8 encoding for better processing performance.|
|Log file size|Unlimited.|
|Log file rotation|Both `.log*` and `.log` are supported.|
|Log collection behavior upon log parsing block|When block occurs in log parsing, Logtail keeps the open status of the log file FD. If log file rotation occurs multiple times during the block, Logtail attempts to keep the log parsing sequence of each rotation. If the number of unparsed log rotations is more than 20, Logtail does not process subsequent log files. Soft link support More information, see here.|
|Single log size|Monitored directories can be soft links.|
|Single log size|The size of a single log cannot exceed 512 KB. If multiple-line logs are divided by a regular expression, the maximum size of each log is still 512 KB.  If the log size exceeds 512 KB, the log is forced to be divided into multiple parts for collection. For example, a log is 1025 KB. The first 512 KB is processed for the first time, the subsequent 512 KB is processed for the second time, and the last 1 KB is processed for the third time.|
|Regular expression type|Use regular expressions that are compatible with Perl. |
|Multiple collection configurations for the same file|Not supported. We recommend that you collect log files to a Logstore and configure multiple subscriptions.  If this function is required, configure a soft link for the log file to bypass this limit.|
|File opening behavior|Logtail keeps a file to be collected in the open status. Logtail closes the file if the file does not have any modification within five minutes.|
|First log collection behavior|Logtail only collects incremental log files. If modifications are found in a file for the first time and the file size exceeds 1 MB, Logtail collects the logs from the last 1 MB. Otherwise, Logtail collects logs from the beginning. If a log file is not modified after the configuration is issued, Logtail does not collect this file.|
|Non-standard text log|For a row containing ‘\\0’ in the log. The log is truncated to the first ‘\\0’.|

|Item|Capabilities and limits|
|:---|:----------------------|
|Checkpoint timeout period|If the file has not been modified for more than 30 days, the Checkpoint is deleted.|
|Checkpoint storage policy|Regular save every 15 minutes, automatically saved when the program exits.|
|Checkpoint save path|The default save path is `/tmp/logtail_checkpoint` , you can modify the parameters according to [Configure startup parameters](intl.en-US/User Guide/Logtail collection/Install/Configure startup parameters.md).|

|Item|Capabilities and limits|
|:---|:----------------------|
|Configuration update|Your updated configuration takes effect with a delay of about 30 seconds.|
|Dynamic configuration loading|Supported. The configuration update does not affect other collections.|
|Number of configurations|Theoretically unlimited. We recommend that the number of collection configurations for a server is no more than 100.|
|Multi-tenant isolation|The isolation between collection configurations.|

|Item|Capabilities and limits|
|:---|:----------------------|
|Log processing throughput|The default limit to raw log traffic is 2 MB/s. Data is uploaded after being encoded and compressed, generally with a compression ratio of 5–10 times.  Logs may be lost if the log traffic exceeds the limit. To adjust the parameter, see   [Configure startup parameters](intl.en-US/User Guide/Logtail collection/Install/Configure startup parameters.md) Configure startup parameters.|
|Maximum performance|In case of single core, the maximum processing capability is 100 MB/s for logs in simple mode, 20 MB/s by default for logs in full mode \(depending on the complexity of the regular expression\), 40 MB/s for logs in delimiter mode, and 30 MB/s for logs in JSON mode. Enabling multiple log processing threads improves the performance by 1.5–3 times.|
|Number of monitored directories|Logtail actively limits the depth of monitored directories to conserve your resources.  If the upper limit is reached, Logtail stops monitoring more directories and log files. Logtail monitors at most 3,000 directories  \(including subdirectories\).|
|Default resource limit|By default, Logtail occupies up to 40% of CPU usage and 256 MB of memory usage. If logs are generated at a high speed, you can adjust the parameter by following the  [Configure startup parameters](intl.en-US/User Guide/Logtail collection/Install/Configure startup parameters.md)  Configure startup parameters.|
|Processing policy for resource limit exceeding|If the resources occupied by Logtail in 3 minutes exceed the upper limit, Logtail is forced to restart, which may cause loss or duplication of data.|

|Item|Capabilities and limits|
|:---|:----------------------|
|Network error handling|If the network connection is abnormal, Logtail actively retries and automatically adjusts the retry interval.|
|Handling of resource quota exceeding|If the data transmission rate exceeds the maximum quota of Logstore, Logtail blocks log collection and automatically retries.|
|Maximum retry period for timeout|If data transmission fails for more than 6 successive hours, Logtail discards the data.|
|Status self-check|Logtail automatically restarts in the case of an exception, for example, abnormal exit of a program or resource limit exceeding.|

|Item|Capabilities and limits|
|:---|:----------------------|
|Log collection delay|Except for block status, the delay in log collection by Logtail does not exceed one second after logs are flushed to a disk.|
|Log uploading policy|Logtail automatically aggregates logs in the same file before uploading them. Log uploading is triggered in the condition that more than 2,000 logs are generated, the log file exceeds 2 MB, or the log collection exceeds 3 seconds.|

