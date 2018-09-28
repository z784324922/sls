# Analysis - Apache access logs {#task_gdc_rpl_52b .task}

Log Service supports one-stop configuration of collecting Apache logs and setting indexes through the data import wizard. You can analyze website accesses in real time through the default dashboard and query analysis statements.

-   You have activated Log Service.
-   You have created a project and a Logstore.

A webmasters uses Apache as the server to build a website. To assess accesses to the website, the webmaster has to analyze Apache access logs to obtain PV, UV, IP address region distribution, client types, source pages, and other information.

Log Service supports one-stop configuration of collecting Apache logs and setting indexes through the data import wizard, and creates the access analysis dashboard for Apache logs by default.

We recommend that you use the following custom configuration for Apache logs to fit the analysis scenarios:

```
LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\" %D %f %k %p %q %R %T %I %O" customized
```

**Note:** Please check if the log content corresponding to some fields has spaces according to you log content. For example, `%t`, `%{User-Agent}i`, `%{Referer}i`, and more. If fields with spaces exist, use`\"` to wrap the fields in the configuration information to avoid affecting log resolution.

The meaning of each field is as follows:

|Field|Field name|Description|
|:----|:---------|:----------|
|％h|remote\_addr|The client IP address.|
|％l|remote\_ident|The client log name, from identd.|
|％u|remote\_user|The client username.|
|％t|time\_local|The server time.|
|％r|request|The request content, including the method name, address, and HTTP protocol.|
|％\>s|status|The returned HTTP status code.|
|％b|response\_size\_bytes|The returned size.|
|％\{Rererer\}i|http\\u0008\_referer|The referer.|
|％\{User-Agent\}i|http\_user\_agent|The client information.|
|％D|request\_time\_msec|The request time in milliseconds.|
|％f|filename|The request file name with the path.|
|％k|keep\_alive|The number of keep-alive requests.|
|％p|remote\_port|The server port number.|
|％q|request\_query|A query string. If no query string exists, this field is an empty string.|
|％R|response\_handler|The handler for the server response.|
|％T|request\_time\_sec|The request time in seconds.|
|％I|bytes\_received|The number of bytes received by the server, requiring the mod\_logio module to be enabled.|
|％O|bytes\_sent|The number of bytes sent by the server, requiring the mod\_logio module to be enabled.|

1.  Log on to the [Log Service console](https://partners-intl.console.aliyun.com/#/sls) and click the project name. 
2.  On the Logstores page, click the data import wizard icon for a Logstore. 
3.  Select data source as **APACHE Access Log**. 
4.  Configure data source. 
    1.  Enter **Configuration Name**. 
    2.  Enter **Log Path**. 
    3.  Select **Log format**. Select **Log format** according to the format stated in your Apache log configuration file. To facilitate query analysis of log data, we recommend that you use a customized Apache log format.
    4.  Enter **Apache Logformat Configuration**. If you select **Log format** as **common** or **combined**, the corresponding configuration is automatically entered here. If you select the Log format as **Customized**, enter your customized configuration here. We recommend that you enter the configuration recommended at the beginning of this document.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/15381285549380_en-US.png)

    5.  Confirm **APACHE Key Name**. Log Service automatically parses your Apache key name, which you can confirm on the page.

        **Note:** The %r field is extracted to three keys:`request_method`, `request_uri`, and `request_protocol`.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/15381285549381_en-US.png)

    6.  Configure advanced options and click Next. 

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

5.  Apply the configuration to the machine group. Select machine groups to which the specified configurations will apply. Click **Apply to Machine Group** at the bottom right corner of the page.

    If you have not created any machine group, click **Create Machine Group** to create one.

6.  Configure **Search, Analysis, and Visualization**. If you can make sure that the log machine group has a normal heartbreak, click the **Preview** button to obtain the collected data.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/15381285549382_en-US.png)

    To query and analyze collected data of Log Service in real time, please confirm your index attribute configuration on the current page. Click **Open** to view the Key/Value Index Attributes.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/15381285549383_en-US.png)

    A dashboard named LogstoreName-apache-dashboard is preconfigured. After configuration, you can view real-time dynamics such as source IP address distribution and request status ratios on the Dashboard page.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/15381285549384_en-US.png)

    -   **Display source IP region distribution \(ip\_distribution\)**: Displays region distribution of the source IP addresses. The statistical statement is as follows:

        ```
        * | select ip_to_province(remote_addr) as address,
                    count(1) as c
                    group by address limit 100
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/15381285559389_en-US.png)

    -   **Count the ratios of request statuses \(http\_status\_percentage\)**: Counts the ratio of each HTTP status code on the last day. The statistical statement is as follows:

        ```
        status>0 | select status,
                        count(1) as pv 
                        group by status
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/15381285559388_en-US.png)

    -   **Count the ratios of request methods \(http\_method\_percentage\)**: Counts the ratio of each request method used on the last day. The statistical statement is as follows:

        ```
        * | select request_method,
                    count(1) as pv 
                    group by request_method
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/15381285559387_en-US.png)

    -   **PV/UV statistics \(pv\_uv\)**: Counts the numbers of PVs and UVs on the last day. The statistical statement is as follows:

        ```
        * | select date_format(date_trunc('hour', __time__), '%m-%d %H:%i')  as time,
                    count(1) as pv,
                    approx_distinct(remote_addr) as uv
                    group by time
                    order by time
                    limit 1000
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/15381285559385_en-US.png)

    -   **Count inbound and outbound traffic \(net\_in\_net\_out\)**: Counts inbound and outbound traffic. The statistical statement is as follows:

        ```
        * | select date_format(date_trunc('hour', __time__), '%m-%d %H:%i') as time, 
                    sum(bytes_sent) as net_out, 
                    sum(bytes_received) as net_in 
                    group by time 
                    order by time 
                    limit 10000mit 10
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/15381285559391_en-US.png)

    -   **Count the ratios of request UA \(http\_user\_agent\_percentage\)**: Counts the ratio of each browser used on the last day. The statistical statement is as follows:

        ```
        * | select  case when http_user_agent like '%Chrome%' then 'Chrome' 
                    when http_user_agent like '%Firefox%' then 'Firefox' 
                    when http_user_agent like '%Safari%' then 'Safari'
                    else 'unKnown' end as http_user_agent,count(1) as pv
                    group by  http_user_agent
                    order by pv desc
                    limit 10
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/15381285559390_en-US.png)

    -   **Count the top 10 referers \(top\_10\_referer\)**: Counts the top 10 referers on the last day. The statistical statement is as follows:

        ```
        * | select  http_referer, 
                    count(1) as pv 
                    group by http_referer 
                    order by pv desc limit 10
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/153812855510098_en-US.png)

    -   **Count the top 10 access pages \(top\_page\)**: Counts the top 10 pages with the most PVs on the last day. The statistical statement is as follows:

        ```
        * | select split_part(request_uri,'?',1) as path, 
                    count(1) as pv  
                    group by path
                    order by pv desc limit 10
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/15381285559386_en-US.png)

    -   **Count the top 10 uri addresses with longest response latency \(top\_10\_latency\_request\_uri\)**: Counts the top 10 uri addresses with the longest response latency for requests on the last day. The statistical statement is as follows:

        ```
        * | select request_uri as top_latency_request_uri,
                    request_time_sec 
                    order by request_time_sec desc limit 10 10
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17637/153812855510099_en-US.png)


