# Collect text logs {#concept_ed1_fbc_wdb .concept}

The Logtail client helps Log Service users collect text logs from Elastic Compute Service \(ECS\) instances or local servers in the console.

## Prerequisites {#section_ehf_hbc_wdb .section}

-   You must install Logtail before collecting logs. For installation methods, see [Install Logtail in Linux](reseller.en-US/User Guide/Logtail collection/Install/Install Logtail in Linux.md) and [Install Logtail on Windows](reseller.en-US/User Guide/Logtail collection/Install/Install Logtail on Windows.md).

-   To collect logs from ECS instances or local servers, you must open ports 80 and 443.


## Limits {#section_eg1_lbc_wdb .section}

-   A file can only be collected using one configuration. To collect a file with multiple configurations, we recommend you use the soft link. For example, to collect files under `/home/log/nginx/log` with two configurations, you can use the original path for one configuration, and run the command `ln -s /home/log/nginx/log   /home/log/nginx/link_log` to create a soft link of this folder, and then use the soft link path for the other configuration.

-   For more information about operating systems supported by the Logtail client, see [Overview](reseller.en-US/User Guide/Logtail collection/Overview/Overview.md).

-   The ECS instances of the classic network or Virtual Private Cloud \(VPC\) and the Log Service project must belong to the same region. If your source data is transmitted by Internet \(similar to IDC\), you can select the region that the Log Service resides in based on the region description.


## Configuration process of log collection {#section_kyp_4bc_wdb .section}

The following are simple mode and full mode examples. The configuration process as follows:

![](images/2859_en-US.png "Log collection configuration process")

## Log collection modes {#section_pzt_z5l_ggb .section}

Logtail supports simple mode, delimiter mode, JSON mode, full mode, and other log collection methods.

-   Simple mode

    Currently, simple mode is the single-line mode. By default, one line of data is a log, and two logs are separated by a line break in the log file. The system does not extract log fields \(that is, the regular expression \(.\*\) by default\), and uses the current server system time as the generated log time. To configure more settings, you can change the configuration to full mode to adjust the settings. For more information on how to change the Logtail configuration, see [Create a Logtail configuration](reseller.en-US/User Guide/Logtail collection/Machine Group/Create a Logtail configuration.md).

    In the simple mode, specify the file directory and file name. Then, the Logtail collects logs by each line and uses the system time.

-   **Delimiter mode**

    Logtail can collect delimiter logs through the delimiter mode. For more information, see [Delimiter logs](reseller.en-US/User Guide/Other collection methods/Common log formats/Delimiter logs.md).

-   **JSON mode**

    You can select **JSON mode** to collect [JSON logs](reseller.en-US/User Guide/Other collection methods/Common log formats/JSON logs.md).

-   **Full mode**

    To configure more personalized field extraction settings for log contents \(such as cross-line logs and field extraction\), select **Full Mode**.

    Log Service provides a log sample-based regular expression generation function in the data collection wizard. However, multiple manual tests to fit the log samples are required because of the different log samples. For more information about how to test the regular expressions, see [How do I test regular expressions?](../../../../../reseller.en-US/.md)


## Procedure {#section_xss_pbc_wdb .section}

1.  Click Project name to enter the Logstore List.
2.  Select Logstore, and click the **Wizard** at the right side of the Logstore.
3.  Select the data source.

    Select **Text** under **Other Sources** and then click Next to go to the Configure Data Source step.

4.  Specify the **Configuration Name**. 

    The configuration name can be 3–63 characters long, contain lowercase letters, numbers, hyphens \(-\), and underscores \(\_\), and must begin and end with a lowercase letter or number.

    **Note:** The configuration name cannot be modified after the configuration is created.

5.  Specify the log directory and the file name.

    The directory structure must be a full path or a path that contains wildcards.

    **Note:** Only  `*` and `?`can be used as wildcards in the directory.

    The log file name must be a complete file name or a name that contains wildcards. For the rules of file names, see [Wildcard matching](http://man7.org/linux/man-pages/man7/glob.7.html).

    The search mode of log files is the multi-level directory matching mode, namely, under the specified folder \(including directories of all levels\), all the files that conform to the file name can be monitored. 

    -   `/apsara/nuwa/ …   /*.log` means the files whose suffix is `.log` and exist in the `/apsara/nuwa` directory \(including its recursive subdirectories\).
    -   `/var/logs/app_* … /*.log*` means the files whose file name contains `.log` and exist in all of the directories that conform to the `app_*` mode \(including their recursive subdirectories\) under the `/var/logs` directory.
    **Note:** A file can only be collected by one configuration.

    ![](images/2860_en-US.png "Specify the Directory and file name")

6.  Set collection mode. The following uses the full mode as an example.
    1.  Enter  **the Log Sample**.

        The purpose of providing a log sample is facilitating the Log Service console in automatically extracting the regex matching mode in logs. Be sure to use a log from the actual environment.

    2.  Disable    **Singleline**.

        By default, the single-line mode is used, that is, two logs are separated by a line break. To collect cross-line logs \(such as Java  program logs\), you must disable **Singleline** and then configure the **Regular Expression**.

    3.  Modify. the  **Regular Expression**.

        You can select to **automatically generate the regular expression** or **manually enter the regular expression**. After entering the log sample, click  **Auto Generate** to automatically generate the regular expression. If failed, you can switch to the manual mode to enter the regular expression for verification. 

    4.  Enable Extract Field.

        To analyze and process fields separately in the log content, use the Extract  **Field** function to convert the specified field to a key-value pair before sending  it to Log Service. Therefore, you must specify a method for parsing the log content, that is, a regular expression.

        The Log Service console allows you to specify a regular expression for parsing the log content in two ways.  The first option is to automatically generate a regular expression through simple interactions. You can select the field to be extracted in the log sample and then click Generate RegEx to automatically generate the regular expression in the Log Service console.

        In this way, you can generate the regular expression without writing it on your own. You can also manually enter a regular expression. Click **Manually Input Regular Expression** to switch to the manual input mode. After entering the regular expression, click **Validate** to validate whether or not the entered regular expression can parse and extract the log sample. For more information, see [How do I test regular expressions?](../../../../../reseller.en-US/.md)

        No matter the regular expression for parsing the log content is automatically generated or manually entered, you must name each extracted field, that is, set keys for the fields.

         

    5.  Set **Use System Time**.

        Default settings **Use System Time is set by default**.  If disabled, you must specify a certain field \(value\) as the time field during field extraction and  name this field `time`.  After selecting a `time` field, you  can click  **Auto Generate** in  **Time Format** to generate a  method to parse this time field.  For more information on log time format, see [Text logs - Configure time format](reseller.en-US/User Guide/Logtail collection/Text logs/Text logs - Configure time format.md).

    6.  Enable **Drop Failed to Parse Logs** as needed.

        This option specifies whether to upload the logs with parsing failure to Log Service.

        When this option is enabled, the logs with parsing failure will not be uploaded to Log Service. When the option is disabled, the raw log will be uploaded to Log Service when log parsing fails. The key of the raw log is `__raw_log__` and the value is the log content.

7.  \(Optional\) Set **Advanced Options** as needed and click **Next**.

    If you have no special requirements, retain the default settings.

    |Configuration item|Desceiption|
    |:-----------------|:----------|
    |Upload Original Log|Select whether or not to upload the original log.  If enabled, the new field is added by default to upload the original log.|
    |Topic Generation Mode|     -     **Null - Do not generate topic**: The default option, which indicates to set the topic as a null string and you can query logs without entering the topic. 
    -   **Machine Group Topic Attributes**: Used to clearly differentiate log data generated in different frontend servers.
    -   **File Path Regular**: With this option selected, you must enter the **Custom RegEx** to use the regular expression to extract contents from the path as the topic. Used to differentiate log data generated by users and instances. Used to differentiate log data generated by users and instances.
 |
    |Custom RegEx|After selecting **File Path Regular** as Topic Generation Mode, you must enter your custom regular expression.|
    |Log File Encoding|     -   utf8: Use UTF-8 encoding.
    -   gbk: Use GBK encoding.
 |
    |Maximum Monitor Directory Depth|Specify the maximum depth of the monitored directory when logs are collected from the log source, that is, at most how many levels of logs can be monitored. The range is 0–1000, and 0 indicates to only monitor the current directory level.|
    |Timeout |A log file has timed out if it does not have any update within a specified time.  You can configure the following settings for **Timeout**.    -   Never Time out: Specify to monitor all log files persistently and the log files never time out. 
    -   30 minute timeout: A log file has timed out and is not monitored if it does not have any update within 30 minutes. 
|
    |Filter Configuration|Only logs that **completely** conform to the filter conditions can be collected. For example: 

    -   **collect logs that conform to a condition** :  Key:level Regex:WARNING|ERROR indicates to only collect logs whose level is WARNING or ERROR.
    -      **[filter logs that do not conform to a condition](http://www.regular-expressions.info/lookaround.html)** :
        -   `Key:level Regex:^(?!. *(INFO|DEBUG))`,  indicates to not collect logs whose level is INFO or DEBUG.
        -   `Key:url Regex:. *^(?!.*(healthcheck)). *`, indicates to filter logs with healthcheck in the url. Such as logs in which key is url  and value is `/inner/healthcheck/jiankong.html` will not be collected.
 For similar examples, see[regex-exclude-word](https://stackoverflow.com/questions/2404010/match-everything-except-for-specified-strings) and [regex-exclude-pattern](https://stackoverflow.com/questions/2078915/a-regular-expression-to-exclude-a-word-string).

|

8.  Click **Next** after completing the configurations.

    If you have not created a machine group, you must create one first. For how to create a machine group, see Create a machine [Create a machine group with an IP address as its identifier](reseller.en-US/User Guide/Logtail collection/Machine Group/Create a machine group with an IP address as its identifier.md)group.

    **Note:** 

    -   It takes up to three minutes for the Logtail configuration to take effect, so be patient.
    -   To collect IIS access logs, see [Use Logstash to collect IIS logs](reseller.en-US/User Guide/Other collection methods/Common log formats/Use Logstash to collect IIS logs.md).
    -   After creating the Logtail configuration, you can view the Logtail configuration list, modify the Logtail configuration, or delete the Logtail configuration. For more information, see [Create a Logtail configuration](reseller.en-US/User Guide/Logtail collection/Machine Group/Create a Logtail configuration.md).
    ![](images/2865_en-US.png "Applying the configuration to the machine group")

    Log Service starts to collect logs after completing the configurations.


## Subsequent operations {#section_prs_2cc_wdb .section}

After completing the preceding configurations, you can configure the **Search, Analysis, and Visualization** and **Shipper & ETL** as instructed on the page.

Logs collected to Log Service in the simple mode are as follows. All the contents of each log are displayed under the key named **content**.

 ![](images/2866_en-US.png "Preview") 

Logs collected to Log Service in the full mode are as follows. The contents of each log are collected to Log Service according to the configured key-value.

![](images/2867_en-US.png "Preview")

## Logtail configuration items {#section_cph_gcc_wdb .section}

You must complete the configuration items when configuring Logtail. The descriptions and limits of the commonly used configuration items are as follows.

|Configuration item|Description |
|Log path|Make sure that the log monitoring directory and the log file name match with the files on the machine. The directory does not support fuzzy match and must be set to an absolute path, while the log file name supports fuzzy match. The path that contains wildcards can match with directories of multiple levels, that is, under the specified folder \(including directories of all levels\), all the files that conform to the file name can be monitored.|
|Log file name|The name of the file from which logs are collected, which is case-sensitive and can contain wildcards,  for example, `*.log` .  The file name wildcards in Linux include \* ,  “?”, and \[…\] .|
|Local Storage|Whether or not to enable the local cache to temporarily store logs that cannot be sent because of short-term network interruption.|
|First-line log header|Specifies the starting header of a multiline log by specifying a regular expression.  Lines cannot be used to separate individual logs when multiline log is collected \(such as the stack information in application logs\).  In this case, you must specify the start line of a multi-line log. When this line is discovered, this indicates the last log has ended and a new log has begun. Therefore, you must specify a matching rule for the starting header, that is, a regular expression here.|
|Log parsing expression|Defines how to extract a piece of log information and convert it to a log format supported by Log Service. The user must specify a regular expression to extract the required log field information and define the name of each field to be extracted.|
|Log time format|Defines how to parse the time format of the timestamp string in log data. For more information, see [Text logs - Configure time format](reseller.en-US/User Guide/Logtail collection/Text logs/Text logs - Configure time format.md).|

## Writing method of logs {#section_jqm_jcc_wdb .section}

In addition to using Logtail to collect logs, Log Service also provides APIs and SDKs to help you write logs.

-    APIs to write logs

    Log Service provides RESTful APIs to help you write logs.  You can use the [PostLogstoreLogs](../../../../../reseller.en-US/API Reference/Logstore related APIs/PostLogstoreLogs.md) API to write data. For more information on a complete API reference, see [Overview](../../../../../reseller.en-US/API Reference/Overview.md).

-   Use SDKs to write logs

    In addition to APIs, Log Service also provides SDKs in multiple languages \(Java,  .NET, PHP, and Python\) to help you write logs. For more information on a  complete SDK reference, see SDK reference [Overview ](../../../../../reseller.en-US/SDK Reference/Basic Descriptions /Overview.md).


