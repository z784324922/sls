# Python logs {#reference_bj1_ss5_vdb .reference}

The logging module of Python provides a general logging system, which can be used by third-party modules or applications.  The logging module provides different log levels and logging methods such as files, HTTP GET/POST, SMTP, and Socket. You can customize a logging method as needed. The logging module is the same as Log4j except that they have different implementation details. The logging module provides the logger, handler, filter, and formatter features.

To ccollect Python logs, we recommend you to use logging handler directly:

-   [Automatically upload Python logs using log handler](https://aliyun-log-python-sdk.readthedocs.io/tutorials/tutorial_logging_handler.html)
-   [Log handler automatically parses logs in KV format](https://aliyun-log-python-sdk.readthedocs.io/tutorials/tutorial_logging_handler_kv.html)
-   [Log handler automatically parses a log in JSON format](https://aliyun-log-python-sdk.readthedocs.io/tutorials/tutorial_logging_handler_json.html)

## Python log format {#section_pc1_m5b_ry .section}

The log format specifies the output format of log recording in formatter.  The construction method of formatter needs two parameters: message format string and message date string. Both of the parameters are optional.

Python log format:

```
import logging  
import logging.handlers  
LOG_FILE = 'tst.log'  
handler = logging. handlers. rotatingfilehandler (LOG_FILE, maxbytes = 1024*1024, backupcount = 5) # Instantiate the handler   
fmt = '%(asctime)s - %(filename)s:%(lineno)s - %(name)s - %(message)s'  
formatter = logging.Formatter(fmt) # Instantiate the formatter  
handler.setFormatter(formatter)      # Add the formatter to the handler  
logger = logging.getLogger('tst')    # Obtain the logger named tst  
logger.addHandler(handler)           # Add the handler to the logger  
logger.setLevel(logging.DEBUG)  
logger.info('first info message')  
logger.debug('first debug message')
```

## Field description {#section_abx_zc3_dbb .section}

The formatter is configured in the `%(key)s` format, that is, replacing the dictionary keywords.  The following keywords are provided:

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

Output sample log:

```
2015-03-04 23:21:59,682 - log_test.py:16 - tst - first info message   
2015-03-04 23:21:59,682 - log_test.py:17 - tst - first debug message
```

## Configure Logtail to collect Python logs {#section_akh_2d3_dbb .section}

For the detailed procedure of collecting Python logs by using Logtail, see [5-minute quick start](../../../../reseller.en-US/Quick Start/5-minute quick start.md) and [Apache logs](reseller.en-US/User Guide/Data Collection/Common log formats/Apache logs.md). Select the corresponding configuration based on your network deployment and actual situation. 

The automatically generated regular expression is only based on the log sample and does not cover all the situations of logs. 

See the following common Python logs and the corresponding regular expressions:

-   Log format

    ```
    2016-02-19 11:03:13,410 - test.py:19 - tst - first debug message
    ```

    Regular expression type

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

    Regular expression type

    ```
    (\d+-\d+-\d+\s\S+)\s-\s([^:]+):(\d+)\s+-\s+(\d+)\s+(\w+)\s+(\S+)\s+(\w+)\s+(\S+)\s+(\S+)\s+(\d+)\s+(\w+)\s+(\d+)\s+(\w+)\s+-\s+(. *)
    ```


