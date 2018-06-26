# **Query diagnosed errors** {#task_njf_nkb_ry .task}

Errors may occur during log collection by Logtail, such as regular expression parsing failures, incorrect file paths, and traffic exceeding the shard service capability. Currently, the query function is provided for debugging log collection errors.

1.   Enter the error diagnosis page Enter the Logstore List page by clicking a project name. Click **Diagnose** at the right of the Logstore. The Log Collection Error dialog box appears.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13084/5337_en-US.png "Diagnostics")

2.   View log collection errors.  You can view the list of Logtail log collection errors of a specified Logstore in the Log Collection Error dialog box.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13084/5338_en-US.png "View collection errors")

3.   Query log collection errors of a specified machine  

    To query all log collection errors occurred to a specific machine, enter the IP address of the machine in the search box on the query page. Logtail reports errors every 5 minutes.

    After fixing these errors and resuming business, check if the errors persist based on the timeframe.  Historical error reports are still displayed before they expire. Ignore the historical error reports and only check whether new errors occurred after these historical errors are fixed.


|Error type|Description |Handling method|
|:---------|:-----------|:--------------|
|LOGFILE\_PERMINSSION\_ALARM|Logtail has no permission to read the specified file. |Check the Logtail startup account on the server. We recommend that Logtail be started by a root user.|
|SPLIT\_LOG\_FAIL\_ALARM|The regular expression cannot match with the beginning of a line and cannot separate logs into lines. |Check whether the regular expression is correct or not.`*`。|
|MULTI\_CONFIG\_MATCH\_ALARM |A file can only be collected by one Logtail configuration.|Check whether a file is collected by multiple Logtail configurations. If yes, delete the redundant configurations.|
|REGEX\_MATCH\_ALARM|In the regular expression parsing mode, the log content does not match with the regular expression.|Copy the log sample in the error content to retry the matching and generate new regular expression. |
|PARSE\_LOG\_FAIL\_ALARM|Logtail fails to parse logs because the log format does not conform to the definition in the parsing modes such as JSON and delimiter.|Click the error to view relevant details.|
|CATEGORY\_CONFIG\_ALARM|The Logtail collection configuration is invalid. |A common error is that the regular expression fails to extract the file path as a topic. For other errors, open a ticket. |
|LOGTAIL\_CRASH\_ALARM|Logtail has crashed because it exceeds the upper limit of machine resource usage.|For more information, see [Configure startup parameters](intl.en-US/User Guide/Logtail collection/Install/Configure startup parameters.md) to modify the upper limits of CPU usage and memory usage. Open a ticket if you have any questions. |
|REGISTER\_INOTIFY\_FAIL\_ALARM|Logtail fails to register log listening in Linux possibly because Logtail does not have permissions to access the folder or the folder has been deleted.|Check if Logtail has permissions to access the folder or the folder is deleted.|
|DISCARD\_DATA\_ALARM|This error is caused by the insufficient CPU resources configured for Logtail or the flow control of network sending.|For more information, see [Configure startup parameters](intl.en-US/User Guide/Logtail collection/Install/Configure startup parameters.md) to modify the upper limit of CPU usage or the limit on concurrent sending by using network. Open a ticket if you have any questions.|
|SEND\_DATA\_FAIL\_ALARM|\(1\) The main account does not create any AccessKey. \(2\) The Logtail client cannot connect to Log Service, or the network link quality is bad. \(3\) The writing quota of Log Service is insufficient.|1. Use the main account to log on to the AccessKey console to create an AccessKey. 2. Check the local configuration file /usr/local/ilogtail/ilogtail\_config.json. Run the curl    <endpoint\>\(3\) Add shards to the Logstore to support writing larger data volumes.|
|PARSE\_TIME\_FAIL\_ALARM |Logtail fails to parse the time field according to the time parsing expression. |Configure the time parsing expression correctly according to the log time. |
|REGISTER\_INOTIFY\_FAIL\_ALARM|Logtail fails to register inotify watcher for the log directory. |Check whether the directory exists and the directory permission settings.|
|SEND\_QUOTA\_EXCEED\_ALARM|The traffic of writing logs exceeds the limit. |[Split the shard for expansion](../../../../intl.en-US/Product Introduction/Basic concepts/Shard.md) in the console.|
|READ\_LOG\_DELAY\_ALARM |Log collection lags behind log generation. Generally, this error is caused by the insufficient CPU resources configured for Logtail or the network bandwidth throttling.|For more information, see Logtail[Configure startup parameters](intl.en-US/User Guide/Logtail collection/Install/Configure startup parameters.md)to modify the upper limit of CPU usage or the limit on concurrent sending by using network. Open a ticket if you have any questions.|
|DROP\_LOG\_ALARM|Log collection lags behind log generation, and the number of unprocessed log rotations is more than 20. Generally, this error is caused by the insufficient CPU resources configured for Logtail or the flow control of network sending.|For more information, see Logtail[Configure startup parameters](intl.en-US/User Guide/Logtail collection/Install/Configure startup parameters.md)to modify the upper limit of CPU usage or the limit on concurrent sending by using network. Open a ticket if you have any questions.|
|LOGDIR\_PERMINSSION\_ALARM|Logtail has no permission to read the log monitoring directory. |Check whether the log monitoring directory exists. If yes, check the directory permission settings.|
|ENCODING\_CONVERT\_ALARM|Logtail fails to convert the encoding. |Check whether the log encoding format configuration is consistent with the log encoding format. |
|OUTDATED\_LOG\_ALAR|Logs expire with a time lag of more than 12 hours.  Possible causes: Log parsing lags behind by more than 12 hours, the user-defined time field is incorrectly configured, or the time output of the logging program is abnormal. |Check whether READ\_LOG\_DELAY\_ALARM exists. If yes, handle the error according to the instructions of READ\_LOG\_DELAY\_ALARM. If not, check the time field. If the time field is correctly configured, check whether the time output of the logging program is normal. Open a ticket if you have any questions.|
|STAT\_LIMIT\_ALARM |  The number of files in the log collection configuration directory exceeds the limit.|Check whether or not the log collection configuration directory contains many files and subdirectories, and properly configure the monitored root directory and the maximum monitoring depth of the directory.|
|DROP\_DATA\_ALARM |Timeout for flushing logs to the local machine when the process exits. Logs without being flushed are discarded.|Generally, this error is caused by the severe collection obstruction. For more information, see [Configure startup parameters](intl.en-US/User Guide/Logtail collection/Install/Configure startup parameters.md) to modify the upper limit of CPU usage or the limit on concurrent sending by using network. Open a ticket if you have any questions.|
|INPUT\_COLLECT\_ALARM|An exception occurred when collecting the input sources. |Handle the error as instructed by the error message. |
|HTTP\_LOAD\_ADDRESS\_ALARM|The entered HTTP address is invalid. |Check the address validity.|
|HTTP\_COLLECT\_ALARM|An exception occurred when collecting HTTP.|Troubleshoot the error as instructed by the error message. Generally, this error is caused by timeout. |
|FILTER\_INIT\_ALARM|An exception occurred when initiating the filter.|Generally, this error is caused by the invalid regular expression of the filter. Handle the error as instructed.|
|INPUT\_CANAL\_ALARM|An exception occurred when running MySQL binlog.|Troubleshoot the error as instructed by the error message. The canal service may be restarted when the configuration is updated. Therefore, you can ignore the service restart error.|
|CANAL\_INVALID\_ALARM|An exception in the internal status of MySQL binlog.|The table schema modification during the running time leads to the inconsistent meta and then causes this error. Check if the table schema is modified when the error is reported. For other cases, open a ticket. |
|MYSQL\_INIT\_ALARM |An exception occurred when initiating MySQL.|Handle the error as instructed by the error message. |
|MYSQL\_CHECKPOING\_ALARM|An exception in the MySQL checkpoint format.|Check whether or not to modify the checkpoint configurations. For other cases, open a ticket.|
|MYSQL\_TIMEOUT\_ALARM |Timeout for querying MySQL.|Check whether the MySQL server and the network are abnormal.|
|MYSQL\_PARSE\_ALARM |Logtail fails to parse the MySQL query results. |Check whether the checkpoint format configured by MySQL matches with the format of the specified field.|

**Note:** To view all the complete log lines that are discarded because of parsing failure, log on to the machine to view the /usr/local/ilogtail/ilogtail.LOG file.

