# Node.js logs {#reference_z2j_xt5_vdb .reference}

By default, Node.js logs are printed to the console, which makes the data collection and troubleshooting inconvenient. By using Log4js, logs can be printed to files and log format can be customized, which is convenient for data collection and coordination.

```
var log4js = require('log4js');
log4js.configure({
  appenders: [
    {   
      type: 'file', //file output
      filename: 'logs/access.log', 
      maxLogSize: 1024,
      backups:3,
      category: 'normal' 
    }   
  ]
});
var logger = log4js.getLogger('normal');
logger.setLevel('INFO');
logger.info("this is a info msg");
logger.error("this is a err msg");
```

## Log format {#section_zrl_wbp_tcb .section}

After the log data is stored in the text file format by using Log4js, the log is displayed in the following format in the file:

```
[2016-02-24 17:42:38.946] [INFO] normal - this is a info msg
[2016-02-24 17:42:38.951] [ERROR] normal - this is a err msg
```

Log4js has six output levels, including trace, debug, info, warn, error, and fatal in ascending order.

## Collect Node.js logs by using Logtail {#section_orl_wbp_tcb .section}

For how to configure Logtail to collect Log4j logs, see [Python logs](intl.en-US/User Guide/Data Collection/Common log formats/Python logs.md). Select the corresponding configuration based on your network deployment and actual situation. 

The automatically generated regular expression is only based on the log sample and does not cover all the situations of logs. Therefore, you must adjust the regular expression slightly after it is automatically generated. Therefore, you must adjust the regular expression slightly after it is automatically generated. See the following Node.js log samples for reference and write a correct and comprehensive regular expression for your log.

  **See the following common Node.js logs and the corresponding regular expressions**:

-   Log sample 1:
    -   Log sample:

        ```
        [2016-02-24 17:42:38.946] [INFO] normal - this is a info msg
        ```

    -   Regular expression type

        ```
        \[([^]]+)]\s\[([^\]]+)]\s(\w+)\s-(. *)
        ```

    -   Extracted fields:

        `time`, `level`, `loggerName` and `message`。


-   Log sample 2:
    -   Log sample:

        ```
        [2016-01-31 12:02:25.844] [INFO] access - 42.120.73.203 - - "GET /user/projects/ali_sls_log? ignoreError=true HTTP/1.1" 304 - "http://
        aliyun.com/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/48.0.2564.97 Safari/537.36"
        ```

    -   Regular expression type

        ```
        \[([^]]+)]\s\[(\w+)]\s(\w+)\s-\s(\S+)\s-\s-\s"([^"]+)"\s(\d+)[^"]+("[^"]+)"\s"([^"]+). *
        ```

    -   Extracted fields:

        `time`, `level`、, `loggerName`, `ip,``request`, `status`, `referer` and `user_agent`.


