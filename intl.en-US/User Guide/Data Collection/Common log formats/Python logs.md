# Python logs {#reference_bj1_ss5_vdb .reference}

The logging module of Python provides a general logging system, which can be used by third-party modules or applications. The logging module provides different log levels and logging methods such as files, HTTP GET/POST, SMTP, and Socket. You can customize a logging method as needed. The logging module is the same as Log4j except that they have different implementation details. The logging module provides the logger, handler, filter, and formatter features.

To collect Python logs, we recommend you to use logging handler directly:

-   [Automatically upload Python logs by using log handler](https://aliyun-log-python-sdk.readthedocs.io/tutorials/tutorial_logging_handler.html)
-   [Log handler automatically parses logs in KV format](https://aliyun-log-python-sdk.readthedocs.io/tutorials/tutorial_logging_handler_kv.html)
-   [Log handler automatically parses logs in JSON format](https://aliyun-log-python-sdk.readthedocs.io/tutorials/tutorial_logging_handler_json.html)

## Python log format {#section_pc1_m5b_ry .section}

The log format specifies the output format of log records in formatter. The construction method of formatter needs two parameters: message format string and message date string. Both of the parameters are optional.

Python log format:

```
import logging  
import logging.handlers  
LOG_FILE = 'tst.log'  
handler = logging.handlers.RotatingFileHandler(LOG_FILE, maxBytes = 1024*1024, backupCount = 5) # Instantiate the handler   
fmt = '%(asctime)s - %(filename)s:%(lineno)s - %(name)s - %(message)s'  
formatter = logging.Formatter(fmt)   # Instantiate the formatter  
handler.setFormatter(formatter)      # Add the formatter to the handler  
logger = logging.getLogger('tst')    # Obtain the logger named tst  
logger.addHandler(handler)           # Add the handler to the logger  
logger.setLevel(logging.DEBUG)  
logger.info('first info message')  
logger.debug('first debug message')
```

## Field description {#section_abx_zc3_dbb .section}

The formatter is configured in the `%(key)s` format, that is, replacing the dictionary keywords. The following keywords are provided:

|Format|Meaning|
|:-----|:------|
|%\(name\)s| The logger name of the generated log. |
|%\(levelno\)s|The log level in numeric format, including DEBUG, INFO, WARNING, ERROR, and CRITICAL.|
|%\(levelname\)s|The log level in text format, including DEBUG, INFO, WARNING, ERROR, and CRITICAL.|
|%\(pathname\)s|The full path of the source file where the statement that outputs the log resides \(if available\).|
|%\(filename\)s|The file name.|
|%\(module\)s|The name of the module where the statement that outputs the log resides.|
|%\(funcName\)s|The name of the function that calls the log output.|
|%\(lineno\)d|The code line where the function statement that calls the log output resides \(if available\).|
|%\(created\)f|The time \(in the UNIX standard time format\) when the log is created, which indicates the number of seconds since 1970-1-1 00:00:00 UTC.|
|%\(relativeCreated\)d|The interval \(in milliseconds\) between the log created time and the time that the logging module is loaded.|
|%\(asctime\)s|The log creation time, which is in the format of “2003-07-08 16:49:45,896” by default \(the number after the comma \(,\) is the number of milliseconds\).|
|%\(msecs\)d|The log creation time in the milliseconds.|
|%\(thread\)d|The thread ID \(if available\).|
|%\(threadName\)s|The thread name \(if available\).|
|%\(process\)d|The process ID \(if available\).|
|%\(message\)s|The log message.|

## Log sample {#section_zcv_1d3_dbb .section}

Log sample

```
2015-03-04 23:21:59,682 - log_test.py:16 - tst - first info message   
2015-03-04 23:21:59,682 - log_test.py:17 - tst - first debug message
```

Common Python logs and the corresponding regular expressions:

-   Log format

    ```
    2016-02-19 11:03:13,410 - test.py:19 - tst - first debug message
    ```

    Regular expression:

    ```
    (\d+-\d+-\d+\s\S+)\s+-\s+([^:]+):(\d+)\s+-\s+(\w+)\s+-\s+(. *)
    ```

-   Log format

    ```
    %(asctime)s - %(filename)s:%(lineno)s - %(levelno)s %(levelname)s %(pathname)s %(module)s %(funcName)s %(created)f %(thread)d %(threadName)s %(process)d %(name)s - %(message)s
    ```

    Log sample

    ```
    2016-02-19 11:06:52,514 - test.py:19 - 10 DEBUG test.py test <module> 1455851212.514271 139865996687072 MainThread 20193 tst - first debug message
    ```

    Regular expression:

    ```
    (\d+-\d+-\d+\s\S+)\s-\s([^:]+):(\d+)\s+-\s+(\d+)\s+(\w+)\s+(\S+)\s+(\w+)\s+(\S+)\s+(\S+)\s+(\d+)\s+(\w+)\s+(\d+)\s+(\w+)\s+-\s+(. *)
    ```


## Configure Logtail to collect Python logs {#section_akh_2d3_dbb .section}

For the detailed procedures of collecting Python logs by using Logtail, see [5-minute quick start](../../../../intl.en-US/Quick Start/5-minute quick start.md). Select the corresponding configuration based on your network deployment and actual situation.

1.  Create a project and a Logstore. For detailed procedures, see [Preparation](intl.en-US/User Guide/Preparation/Preparation.md).
2.  On the Logstores page, click the **Data Import Wizard** icon.
3.  Select a data source.

    Select the **Text File**.****

4.  Configure the data source.

    1.  Enter the **Configuration Name** and **Log Path**, and then select the **Full Regex Mode** from the mode drop-down list.
    2.  Turn on the **Singleline** switch.
    3.  Enter **Log Sample**.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13042/15404438449834_en-US.png)

    4.  Turn on the **Extract Field** switch.
    5.  Configure **Regular Expression**.

        1.  Generates a regular expression by selecting strings of the log sample.

            If the automatically generated regular expression does not match your log sample, you can generate a regular expression by selecting strings of the log sample. Log Service supports selecting strings to automatically parse the log sample, that is, to automatically generates a regular expression for each selected field. In **Log Sample**, select log fields and click **Generate RegEx**. A regular expression of each selected field is displayed in the **Regular Expression** column. You can generate a full regular expression for the log sample through multiple selections.

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13042/15404438449838_en-US.png)

        2.  Modify the regular expression.

            Considering the format of the actual log data might have minor changes, click **Manually Input** to adjust the automatically generated regular expression according to the actual situations to conform to all log formats that might occur in the collection process.

        3.  Validate the regular expression.

            Click **Validate** after modifying the regular expression If the regular expression is correct, extracted results are displayed. Modify the regular expression if any errors exist.

    6.  Confirm **Extraction Results**.

        View the parsing results of the log fields and enter corresponding keys for the log extraction results.

        Assign a descriptive field name for each log field extraction result. For example, assign time for the time field. If you do not use the system time, you must specify a field where value is time, and name its key as time.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13042/15404438449845_en-US.png)

    7.  Turn on the **System Time** switch.

        If you use the system time, the time of each log is the time when the Logtail client parses the log.

    8.  \(Optional\) Configure **Advanced options**.
    9.  Click **Next**.
    After completing Logtail configuration, apply the configuration to the machine group to collect Python logs.


