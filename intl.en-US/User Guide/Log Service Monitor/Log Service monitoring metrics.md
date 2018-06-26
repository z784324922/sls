# Log Service monitoring metrics {#concept_prd_p4q_zdb .concept}

For details about metric data, see [Monitor Log Service](intl.en-US/User Guide/Log Service Monitor/Monitor Log Service.md).

1.  Read/Write traffic
    -   Meaning: Data traffic that is written to and read from each Logstore in real time.  It makes statistics on the traffic that is written to and read from the specified Logstore through iLogtail, SDKs, and APIs in real time. The traffic volume is the volume of transferred data \(or compressed data\). The measurement period is one minute.
    -   Unit: Bytes/min
2.  Raw data size
    -   Meaning: Volume of the raw data \(before compression\) written to each Logstore.
    -   Unit: Byte/min
3.  Total QPS
    -   Meaning: Number of QPSs of all operations. The measurement period is one minute.
    -   Unit: Count/min
4.  Operation count
    -   Meaning: Number of QPSs of various operations types. The measurement period is one minute.
    -   Unit: Count/min
    -   The following types of operations are measured:
        -   Write:
            -   PostLogStoreLogs: API later than 0.5
            -   PutData: API earlier than 0.4
        -   Keyword query:
            -   GetLogStoreHistogram: Query of keyword distribution, which is an API later than 0.5.
            -   GetLogStoreLogs: Query of keyword-matched logs, which is an API later than 0.5.
            -   GetDataMeta: Same as GetLogStoreHistogram, which is an API earlier than 0.4.
            -   GetData: Same as GetLogStoreLogs, which is an API earlier than 0.4.
        -   Batch data acquisition:
            -   GetCursorOrData: obtains cursors and data in batches.
            -   ListShards: obtains all shards in a Logstore.
        -   List:
            -   ListCategory: same as ListLogStoreLogs, which is an API earlier than 0.4
            -   ListTopics: traverses all topics in a Logstore.
5.  Service status
    -   Meaning: This view collects statistics on the QPSs that correspond to the HTTP status codes returned for all types of operations. You can locate the operation exception based on the return error code and adjust programs in a timely manner.
    -   Status codes:
        -   200: is the normal return code, indicating that the operation is successful.
        -   400: indicates an error of one of the following parameters: Host, Content-length, APIVersion, RequestTimeExpired, query time range, Reverse, AcceptEncoding, AcceptContentType, Shard, Cursor, PostBody, Parameter, and ContentType.
        -   401: indicates that authentication fails because the AccessKey ID does not exist, the signature does not match, or the signature account has no permission. Check whether the project permission list on SLSweb contains the AccessKey.
        -   403:   indicates a quota overrun. For example, the maximum number of Logstores, shards, or read/write operations per minute is exceeded. Locate the specific error based on the returned message.
        -   404: indicates that the requested resource does not exist. Resources include projects, Logstores, topics, and users.
        -   405: indicates that the operation method is incorrect. Check the URL of the request.
        -   500: indicates a Log Service error. Please try again.
        -   502: indicates a Log Service error. Please try again.
6.  Traffic successfully parsed by the agent
    -   Meaning: size of the logs \(raw data\) successfully collected by Logtail
    -   Unite: byte
7.  Number of lines successfully parsed by the agent \(Logtail\)
    -   Meaning: number of logs \(counted by lines\) successfully collected by Logtail
    -   Unit: line
8.  Number of lines the agent fails to parse
    -   Meaning: number of lines Logtail fails to collect due to an error. An error occurs if this view has data.
    -   Unit: line
9.  Agent error count
    -   Meaning: number of IP addresses that encounter an error when Logtail collects logs
    -   Unit: count
10. Number of machines with an agent error
    -   Meaning: number of alarms that indicate a collection error when Logtail collects logs
    -   Unit: count
11. IP address error count \(measured every 5 minutes\)
    -   Meaning: number of IP addresses under various collection error categories, including:
        -   LOGFILE\_PERMINSSION\_ALARM: The agent has no permission to access the log file.
        -   SENDER\_BUFFER\_FULL\_ALARM: Data is discarded because the data collection speed exceeds the network transfer speed.
        -   INOTIFY\_DIR\_NUM\_LIMIT\_ALARM \(INOTIFY\_DIR\_QUOTA\_ALARM\): The number of monitored directories exceeds 3,000. Please set the monitored root directory to a lower-level directory.
        -   DISCARD\_DATA\_ALARM: Data is lost because the data time is 15 minutes earlier than the system time. Ensure that the time of the data written to log files is less than 15 minutes before the system time.
        -   MULTI\_CONFIG\_MATCH\_ALARM: When multiple configurations are applied to collect the same file, Logtail selects a configuration randomly for collection and no data is collected by other configurations.
        -   REGISTER\_INOTIFY\_FAIL\_ALARM: Inotify event registration fails. For details, view the Logtail log.
        -   LOGDIR\_PERMINSSION\_ALARM: The agent has no permission to access the monitored directory.
        -   REGEX\_MATCH\_ALARM: regular expression match error. Please adjust the regular expression.
        -   ENCODING\_CONVERT\_ALARM: An error occurs when the log encoding format is converted. For details, view the Logtail log.
        -   PARSE\_LOG\_FAIL\_ALARM: log parsing error, which may be due to an incorrect regular expression at the beginning of the line or incorrect log splitting by line because the size of a single log exceeds 512 KB. For details, view the Logtail log. Adjust the regular expression if it is incorrect.
        -   DISCARD\_DATA\_ALARM: Data is discarded because Logtail fails to write the data to the local cached file when the data cannot be sent to the Log Service. The possible cause is that the speed at which log files are generated exceeds the speed at which data is written to the cached file.
        -   SEND\_DATA\_FAIL\_ALARM: Logtail fails to send parsed logs to the Log Service. For details, view the error code and message related to data sending failures in the Logtail log. Common errors include Log Service quota overruns and network exceptions at the agent side.
        -   PARSE\_TIME\_FAIL\_ALARM: An error occurs when the time field of the log is parsed. The time field parsed by Logtail using the regular expression cannot be parsed based on the time format configuration. Please modify the configuration.
        -   OUTDATED\_LOG\_ALARM: Logtail discards historical data. Ensure that the difference between the time of currently written data and the system time is less than 5 minutes.
    -   Locate the specific IP address based on the error. Log on to the machine and view the /usr/logtail/ilogtail.LOG file to identify the cause.

