# Apache log {#concept_zcn_wq5_vdb .concept}

The Apache log format and directory are generally in the /etc/apache2/httpd.conf configuration file.

## Log format {#section_k1d_5sb_ry .section}

By default, the Apache log configuration file defines two print formats: combined format and common format. You can also create your own customized log print format as needed.

-   Combined format:

    ```
    LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" combined
    ```

-   Common format:

    ```
    LogFormat "%h %l %u %t \"%r\" %>s %b" 
    ```

-   Customized format:

    ```
    LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\" %D %f %k %p %q %R %T %I %O" customized
    ```


You need to specify the print format, log file path, and log name of the current log in the Apache log configuration file. For example, the following log configuration file indicates that combined print format is used, and the log path and name is displayed as /var/log/apache2/access\_log.

```
CustomLog "/var/log/apache2/access_log" combined
```

## Field description {#section_mm1_gl4_y1b .section}

|Format|Key name|Description|
|%a|client\_addr|Client IP address.|
|%A|local\_addr|Local private IP address.|
|%b|response\_size\_bytes|Size of response in bytes. When the size of response is null, this field is a hyphen \(-\).|
|%B|response\_bytes|Size of response in bytes. When the size of response is null, this field is a hyphen \(-\).|
|%D|request\_time\_msec|Request time, in microseconds.|
|%h|remote\_addr|Remote hostname.|
|%H|request\_protocol\_supple|Request protocol.|
|%l|remote\_ident|Client log name from identd.|
|%m|request\_method\_supple|Request method.|
|%p|remote\_port|Server port.|
|%P|child\_process|Child process ID.|
|%q|request\_query|Query string. If no query string exists, this field is an empty string.|
|"%r"|request|Request content, including the request method name, address, and HTTP protocol.|
|%s|status|HTTP status code.|
|%\>s|status|Final HTTP status code.|
|%f|filename|Filename.|
|%k|keep\_alive|Number of keepalive requests.|
|%R|response\_handler|Handler on the server.|
|%t|time\_local|Server time.|
|%T|request\_time\_sec|Request time, in seconds.|
|%u|remote\_user|Client username.|
|%U|request\_uri\_supple|Requested URL path. No query is included in the path.|
|%v|server\_name|Server name.|
|%V|server\_name\_canonical|Server name conforming to the UseCanonicalName setting.|
|%I|bytes\_received|Number of bytes received by the server. You must enable the mod\_logio module.|
|%O|bytes\_sent|Number of bytes sent by the server. You must enable the mod\_logio module.|
|"%\{User-Agent\}i"|http\_user\_agent|Client information.|
|"%\{Rererer\}i"|http\_referer|Source page.|

## Sample log {#section_ng3_hl4_y1b .section}

```
192.168.1.2 - - [02/Feb/2016:17:44:13 +0800] "GET /favicon.ico HTTP/1.1" 404 209 "http://localhost/x1.html" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/48.0.2564.97 Safari/537.36" 
```

## Configure a Logtail client to collect Apache logs {#section_al1_gl4_y1b .section}

1.  On the Logstores page, click the **Data Import Wizard** icon.
2.  Select a data type.

    Select **APACHE Access Log**.****

3.  Configure data source.

    1.  Enter the **Configuration Name** and **Log Path**.
    2.  Select a **Log format**.
    3.  Enter **APACHE Logformat Configuration** if you select the customized log format.

        Enter the log format configuration fields of the standard APACHE configuration file. Generally, the configuration file starts with LogFormat.

        **Note:** If you select **common** or **combined** from the **Log format** drop-down list, the configuration fields of the corresponding log format are automatically added here. You need to confirm whether the added configuration fields are consistent with the format defined in the local Apache configuration file.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/15426052219380_en-US.png)

    4.  Confirm **APACHE Key Name**.

        Log Service automatically reads your Apache keys. Confirm the Apache key names on the current page.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/15426052219381_en-US.png)

    5.  \(Optional\) Configure **Advanced options**.

        |Config Maps|Details|
        |:----------|:------|
        |Local Cache|Select whether to enable **Local Cache**.  If this function is enabled, logs can be cached in the local directory of the machine when Log Service is unavailable and continue to be sent to Log Service after the service recovery. By default, at most 1 GB logs can be cached.|
        |Upload Original Log|Select whether or not to upload the original log.  If enabled, the new field is added by default to upload the original log.|
        |Topic Generation Mode|         -     **Null - Do not generate topic**: The default option, which indicates to set the topic as a null string and you can query logs without entering the topic. 
        -   **Machine Group Topic Attributes**: Used to clearly differentiate log data generated in different frontend servers.
        -   **File Path Regular**: With this option selected, you must enter the **Custom RegEx** to use the regular expression to extract contents from the path as the topic. Used to differentiate log data generated by users and instances. Used to differentiate log data generated by users and instances.
 |
        |Custom RegEx|After selecting **File Path Regular** as Topic Generation Mode, you must enter your custom regular expression.|
        |Log File Encoding|         -   utf8: Use UTF-8 encoding.
        -   gbk: Use GBK encoding.
 |
        |Maximum Monitor Directory Depth|Specify the maximum depth of the monitored directory when logs are collected from the log source, that is, at most how many levels of logs can be monitored. The range is 0–1000, and 0 indicates to only monitor the current directory level.|
        |Timeout |A log file has timed out if it does not have any update within a specified time.  You can configure the following settings for **Timeout**.        -   Never Time out: Specify to monitor all log files persistently and the log files never time out. 
        -   30 minute timeout: A log file has timed out and is not monitored if it does not have any update within 30 minutes. 
|
        |Filter Configuration|Only logs that **completely** conform to the filter conditions can be collected. For example: 

        -   **collect logs that conform to a condition** :  Key:level Regex:WARNING|ERROR indicates to only collect logs whose level is WARNING or ERROR.
        -      **[filter logs that do not conform to a condition](http://www.regular-expressions.info/lookaround.html)** :
            -   `Key:level Regex:^(?!. *(INFO|DEBUG))`,  indicates to not collect logs whose level is INFO or DEBUG.
            -   `Key:url Regex:. *^(?!.*(healthcheck)). *`, indicates to filter logs with healthcheck in the url. Such as logs in which key is url  and value is `/inner/healthcheck/jiankong.html` will not be collected.
 For similar examples, see[regex-exclude-word](https://stackoverflow.com/questions/2404010/match-everything-except-for-specified-strings) and [regex-exclude-pattern](https://stackoverflow.com/questions/2078915/a-regular-expression-to-exclude-a-word-string).

|

4.  Click **Next**.
5.  Select a machine group and then click **Apply to Machine Group**.

    If you have not created any machine group, click **+Create Machine Group** to create one.

    After you apply the Logtail configuration to the machine group, Log Service collects Apache logs according to the configuration. You can configure indexes and log shippers by following the steps of the Data Import Wizard.


