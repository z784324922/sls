# Analyze IIS access logs {#task_crv_5z2_32b .task}

IIS is an extensible web server used to build and host websites. You can use access logs collected by IIS to obtain data such as page views, unique visitors,  client IP addresses, bad requests, and network flow, to monitor and analyze access to your website. 

-   You must have activated Log Service.
-   You must have created a Project and a Logstore. For detailed steps about creating a Project and a Logstore, see [Preparation](../intl.en-US/User Guide/Preparation/Preparation.md).

**Log format**

We recommend that you use the W3C Extended Log Format so that you can specify configurations according to your requirements. In the IIS Manager, click the **Select Fields** toggle. Then, select **sc-bytes** and **cs-bytes** in the Standard Fields list.

![](images/6664_en-US.png "Select fields")

The configuration is as follows:

```
logExtFileFlags="Date, Time, ClientIP, UserName, SiteName, ComputerName, ServerIP, Method, UriStem, UriQuery, HttpStatus, Win32Status, BytesSent, BytesRecv, TimeTaken, ServerPort, UserAgent, Cookie, Referer, ProtocolVersion, Host, HttpSubStatus"
```

-   **Field prefixes**

    |Prefix|Description|
    |:-----|:----------|
    |s-|Server actions|
    |c-|Client actions|
    |cs-|Client-to-server actions|
    |sc-|Server-to-client actions|

-   **Field description**

    |Field|Description|
    |:----|:----------|
    |date|The date that the activity occurs|
    |time|The time that the activity occurs|
    |s-sitename|The Internet service name and instance number of the site visited by the client|
    |s-computername|The name of the server on which the log entry is generated|
    |s-ip|The IP address of the server on which the log entry is generated|
    |cs-method|HTTP request methods such as GET and POST|
    |cs-uri-stem|The target of the action|
    |cs-uri-query|URI query The information following the question mark \(?\) in the HTTP request statement|
    |s-port|The port number of the server that is connected with the client|
    |cs-username|The name of the authenticated user that accessed the server. Authenticated users are referenced as `domain\user name`. Anonymous users are indicated by a hyphen \(-\).|
    |c-ip|The IP address of the client that makes the request|
    |cs-version|The protocol version such as HTTP 1.0 or HTTP 1.1|
    |user-agent|The browser that the client uses|
    |Cookie|The content of the cookie sent or received. A hyphen \(-\) is used when there is no cookie.|
    |referer|The site that the user last visited This site provides a link to the current site.|
    |cs-host|The host header name|
    |sc-status|The HTTP or FTP status code|
    |sc-substatus|The status code of HTTP sub-protocol|
    |sc-win32-status|The Windows status code.|
    |sc-bytes|The number of bytes the server sends|
    |cs-bytes|The number of bytes the server receives|
    |time-taken|The length of time that the action took, which is indicated in milliseconds|


1.  Start the Data Import Wizard steps. 
    1.  On the homepage of the Log Service console, click the specified Project Name to enter the Logstore List page. 
    2.  Click the icon in the **Data Import Wizard** column of the specified project. 
2.  In Step 1 Select Data Source, choose **IIS ASSESSLOG** under the **Third-Party Software** category. 
3.  Configure the data source. 
    1.  Enter the **Configuration Name** and **Log Path**. You can view the log path in the IIS Manager.

        ![](images/6665_en-US.png "View the log path.")

4.  Select **Log format**. Select the log format of your IIS access log.
    -   IIS: Microsoft IIS log file format
    -   NCSA: NCSA Common log file format
    -   W3C: W3C Extended log file format
5.  Fill in the field **IIS Logformat configuration**. 
    -   Microsoft IIS and NCSA Public formats have fixed configurations.
    -   To configure the **IIIS access log** to W3C format, follow these steps:
    1.  Open the IIS configuration file. 

        -   The default path for IIS5 configuration file: C:\\WINNT\\system32\\inetsrv\\MetaBase.bin
        -   The default path for IIS6 configuration file: C:\\WINDOWS\\system32\\inetsrv\\MetaBase.xml
        -   The default path for IIS7 configuration file: C:\\Windows\\System32\\inetsrv\\config\\applicationHost.config
        ![](images/6666_en-US.png "View the configuration file.")

    2.  As shown in figure 3, copy the text inside the quotation marks in the field `logFile logExtFileFlags`. 
    3.  In the field **IIS Logformat configuration** in the console, paste the specified text inside the quotation marks. 

        ![](images/6667_en-US.png "Configure the Data Source")

6.  Confirm the key names. IIS log service will automatically extract the key names.

    ![](images/6668_en-US.png "IIS key names")

7.  Advanced Options \(Optional\) 

    |Config Maps|Details|
    |:----------|:------|
    |Local Cache|Select whether to enable **Local Cache**.  If this function is enabled, logs can be cached in the local directory of the machine when Log Service is unavailable and continue to be sent to Log Service after the service recovery. By default, at most 1 GB logs can be cached.|
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

    Confirm configurations and click **Next**.

8.  Apply the configuration to the machine group. Select the machine groups to which the specified configurations will apply. Click **Apply to Machine Group** at the bottom right corner of the page. 

    If you have not created any machine group, click **Create Machine Group** to create one.

9.  Configure **Search, Analysis, and Visualization** \(Optional\). 

    When the  heartbeat status of the machine group is normal, you can click Preview to view log data.

    ![](images/6669_en-US.png "Preview logs")

    Confirm your Index Properties in the current page to view and analyze the collected log data.  Click **Open** to view the Key/Value Index Attributes.

    You can configure key name mapping. The key names are generated based on previewed data, and correspond to the default key names.  

    ![](images/6670_en-US.png "Key/Value Index Attributes")

    The system provides the default dashboard LogstoreName-iis-dashboard for you. After the preceding configurations are completed, you can view real-time data \(including client IP distribution and the proportion of each HTTP status\) on the dashboard.

    ![](images/6671_en-US.png "Dashboard")

    -   Use the following statement to obtain **client IP distribution**:

        ```
        | select ip_to_geo("c-ip") as country, count(1) as c group by ip_to_geo("c-ip") limit 100
        ```

        ![](images/6672_en-US.png "Client IP distribution")

    -   Use the following statement to check recent **page views and unique visitors**:

        ```
        *| select approx_distinct("c-ip") as uv ,count(1) as pv , date_format(date_trunc('hour', __time__), '%m-%d %H:%i') as time group by date_format(date_trunc('hour', __time__), '%m-%d %H:%i') order by time limit 1000
        ```

        ![](images/6673_en-US.png "Page views and unique visitors")

    -   Use the following statement to obtain the **proportion of each HTTP status**:

        ```
        *| select count(1) as pv ,"sc-status" group by "sc-status"
        ```

        ![](images/6674_en-US.png "Proportion of each HTTP status")

    -   Use the following statement to view the **network flow**:

        ```
        *| select sum("sc-bytes") as net_out, sum("cs-bytes") as net_in ,date_format(date_trunc('hour', time), '%m-%d %H:%i') as time group by date_format(date_trunc('hour', time), '%m-%d %H:%i') order by time limit 10000
        ```

        ![](images/6675_en-US.png "Traffic inflows and outflows")

    -   Use the following statement to get the proportion of each Hrequest methodTTP request method:

        ```
        *| select count(1) as pv ,"cs-method" group by "cs-method"
        ```

        ![](images/6676_en-US.png "Proportion of each HTTP request method")

    -   Use the following statement to obtain the **proportion of each browser type**:

        ```
        *| select count(1) as pv, case when "user-agent" like '%Chrome%' then 'Chrome' when "user-agent" like '%Firefox%' then 'Firefox' when "user-agent" like '%Safari%' then 'Safari' else 'unKnown' end as "user-agent" group by case when "user-agent" like '%Chrome%' then 'Chrome' when "user-agent" like '%Firefox%' then 'Firefox' when "user-agent" like '%Safari%' then 'Safari' else 'unKnown' end order by pv desc limit 10
        ```

        ![](images/6677_en-US.png "Proportion of each browser type")

    -   Use the following statement to view the **top 10 most visited site addresses**:

        ```
        *| select count(1) as pv, split_part("cs-uri-stem",'?',1) as path group by split_part("cs-uri-stem",'?',1) order by pv desc limit 10
        ```

        ![](images/6678_en-US.png "Top 10 most visited site addresses")


