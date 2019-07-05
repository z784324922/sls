# Log analysis {#concept_hws_53h_1gb .concept}

The Search & Analysis page of Log Service is embedded in the Full Log page of the new Anti-DDoS Pro console. You can switch between the Full Log and Log Reports pages. After you enable the log analysis feature of Anti-DDoS Pro, you can query and analyze collected logs in real time, view and edit dashboards, and set alert rules.

## Procedure {#section_zdf_5c2_j2b .section}

1.  Log on to the new Anti-DDoS Pro console. In the left-side navigation pane, choose **Statistics** \> **Full Log**.
2.  Select the website domain for which you want to view log reports, and ensure that the **Status** switch is turned on.
3.  Click **Full Log**.

    The Search & Analysis page of Log Service is embedded in the Full Log page. The system displays logs of the target website based on a default filtering condition, such as `matched_host:"0523.yuanya.aliyun.com"`.

    ![](images/33764_en-US.png "Full log")

4.  Enter a statement, select a time range, and then click **Search & Analysis**.

    **Note:** Logs of Anti-DDoS Pro are stored for 180 days. By default, you can only query logs of the past 180 days.

    ![](images/33765_en-US.png "Query logs")


On the Search & Analysis page, you can customize query and analysis.

-   **Search and process logs by using built-in syntax** 

    Log Service supports query syntax and analysis syntax for log query in complex scenarios. For more information, see [Search and process logs by using built-in syntax](#) in this topic.

-   **View the distribution of log entries by time** 

    The histogram under the search box displays the distribution of log entries that match both the statement and the time range. The horizontal axis indicates the time and the vertical axis indicates the number of log entries. The total number of queried log entries is displayed below the histogram.

    **Note:** You can drag the mouse pointer in the histogram to narrow down the time range. The `time picker` automatically updates the time range, and the query results are automatically updated.

    ![](images/33766_en-US.png "Distribution of log entries by time")

-   **View raw logs** 

    On the **Raw Logs** tab page, each log entry is detailed on an individual page, which displays the time when the log is generated and the log content. The log content contains fields and their values. You can sort log entries, download logs, and click **Column Settings** to select column items to be displayed.

    You can click a field value or part of the field value in log content, and then a corresponding condition is automatically specified in the search box. For example, if you click `GET` in the `request_method: GET` field, the following statement is automatically generated in the search box:

    ``` {#codeblock_xra_nvh_144}
    <The original search statement> and request_method: GET
    ```

    ![](images/33767_en-US.png "Raw logs")

-   **View analysis charts** 

    Log Service supports displaying analysis results in charts. You can select a chart type as needed on the Graph tab page. For more information, see [Charts](reseller.en-US/User Guide/Query and visualization/Analysis graph/Graph description.md#section_pn5_ymv_tdb).

    ![](images/33768_en-US.png "Analysis charts")

-   **Quick analysis** 

    The quick analysis feature provides you with an eay-to-use interactive experience. It enables you to analyze the distribution of a field in a specified time range. This feature can reduce the time used for indexing critical data. For more information, see [Quick analysis](reseller.en-US/User Guide/Index and query/Query/Quick analysis.md#section_lly_xvj_5cb).

    ![](images/33769_en-US.png "Quick analysis")


## Search and process logs by using built-in syntax {#section_ncn_mvt_j2b .section}

A statement consists of a query clause \(Search\) and an analysis clause \(Analytics\), which are separated with a vertical bar \(`|`\).

``` {#codeblock_rf1_87j_5p9}
$Search | $Analytics
```

|Clause|Description|
|:-----|:----------|
|Query|A query clause can contain keywords, strings, numbers, value ranges, or a combination of them. If the clause is not specified or only contains an asterisk \(`*`\), the search result includes all log entries.|
|Analysis|You can use the analysis clause to process query results.|

**Note:** Both query and analysis clauses are optional. If no query clause is specified, the query result includes all log entries in the specified time range, and all log entries are processed based on the analysis clause. If no analysis clause is specified, the query results are returned without being processed.

## Query syntax {#section_jhz_xvr_k2b .section}

The query feature of Log Service supports **full text search** and **search by field**. Statements in the search box can be displayed in multiple lines and highlighted.

-   **Full text search** 

    You can enter keywords to search for all log entries without specifying field names. To use multiple keywords, you can enclose each keyword within quotation marks \("\) and separate them with spaces or `and`.

    Examples:

    -   **Multi-keyword search** 

        You can use the following statements to search for log entries that contain `www.aliyun.com` and `error`. Example:

        ``` {#codeblock_e9h_5rj_c9c}
        www.aliyun.com error
        ```

        Alternative:

        ``` {#codeblock_pft_l4t_wak}
        www.aliyun.com and error
        ```

    -   **Conditional search** 

        You can use the following statement to search for log entries that contain `www.aliyun.com` and `error`, or log entries that contain www.aliyun.com and `404`. Example:

        ``` {#codeblock_l11_bfk_ysr}
        www.aliyun.com and (error or 404)
        ```

    -   **Prefix search** 

        You can use the following statement to search for log entries that contain both `www.aliyun.com` and keywords starting with `failed_`. Example:

        ``` {#codeblock_env_dbq_jfb}
        www.aliyun.com and failed_*
        ```

        **Note:** You can only add an asterisk \(`*`\) as a suffix and you cannot add an asterisk \(`*`\) as a prefix. For example, the statement cannot be `*_error`.

-   **Search by field** 

    Log Service supports accurate queries based on fields.

    You can use the comparison of numeric fields in the format of `field: value` or `field>=value` and separate filtering conditions with `and` and `or`. You can use this feature together with full-text search and separate filtering conditions with `and` and `or`.

    Website access logs and attack logs of Anti-DDoS Pro also support search by field. For more information about the description, type, and format of each log field, see [Log fields](reseller.en-US/User Guide/Cloud product collection/Logs of BGP-line Anti-DDoS Pro/Log fields.md).

    Examples:

    -   **Search by specifying multiple fields** 

        You can use the following statement to search for all log entries that record CC attacks on `www.aliyun.com`:

        ``` {#codeblock_xt0_rhs_wnc}
        matched_host: www.aliyun.com and cc_blocks: 1
        ```

        You can search for all log entries that record visits with 404 errors from a specific client to `www.aliyun.com`. In the following example, the client IP address is `10.2.3.4`.

        ``` {#codeblock_9dt_pi6_cb3}
        real_client_ip: 10.2.3.4 and matched_host: www.aliyun.com and status: 404
        ```

        **Note:** In the preceding examples, `matched_host`, `cc_blocks`, `real_client_ip`, and `status` are fields defined in access and attack logs of Anti-DDoS Pro. For more information about log fields, see [Log fields](reseller.en-US/User Guide/Cloud product collection/Logs of BGP-line Anti-DDoS Pro/Log fields.md).

    -   **Search by specifying numeric fields** 

        You can use the following statement to search for log entries where the response time exceeds 5 seconds:

        ``` {#codeblock_2qq_e4r_xwq}
        request_time_msec > 5000
        ```

        You can search for log entries by specifying a value range. In the following example, the response time is greater than 5 seconds, and is less than or equal to 10 seconds:

        ``` {#codeblock_izz_13f_sgw}
        request_time_msec in (5000 10000]
        ```

        You can also use the following statement:

        ``` {#codeblock_noy_6bj_j8g}
        request_time_msec > 5000 and request_time_msec <= 10000
        ```

    -   **Check whether a specific field exists** 

        You can use the following statements to check whether a specific field exists:

        -   Search for log entries that contain the `ua_browser` field: `ua_browser: *`
        -   Search for log entries that do not contain the `ua_browser` field: `not ua_browser: *`

For more information about query syntax, see [Index and query](reseller.en-US/User Guide/Index and query/Overview.md).

## Analysis syntax {#section_dhz_xvr_k2b .section}

You can use the SQL-92 syntax for log analysis and statistics. For more information about the syntax and functions supported by Log Service, see [Syntax description](reseller.en-US/User Guide/Index and query/Syntax description.md).

**Note:** 

-   In analysis clauses, the `from log` part is similar to the `from <table name>` part in standard SQL statements, and can be omitted.
-   The first 100 log entries are returned by default. You can modify the number of returned log entries by using the [LIMIT syntax](reseller.en-US/User Guide/Index and query/Analysis grammar/LIMIT syntax.md).

## Time-based log query and analysis {#section_eyl_kxt_j2b .section}

Each log entry of Anti-DDoS Pro has a `time` field in the `yyyy-MM-ddTHH:mm:ss+<time zone>` format. For example, in `2018-05-31T20:11:58+08:00`, the time zone is `UTC+8`. Each log entry has a built-in field: `__time__`, which indicates the time when the log entry is generated, so that time-based calculations can be performed. This field is in the [Unix timestamp](https://en.wikipedia.org/wiki/Unix_time) format, and the value of this field indicates the number of seconds that have elapsed since 00:00:00 Coordinated Universal Time \(UTC\), January 1, 1970. Therefore, after a timestamp is calculated, it must be formatted before it is displayed.

-   **Select and display the time** 

    The following example shows how to search for the latest 10 log entries that record CC attacks on `www.aliyun.com` over a specific period of time. The query result includes the time, real\_client\_ip, and http\_user\_agent fields, and the log entries are sorted based on the `time` field.

    ``` {#codeblock_bip_2ud_zps}
    matched_host: www.aliyun.com and cc_blocks: 1 
    |  select time, real_client_ip, http_user_agent
        order by time desc
        limit 10
    ```

    ![](images/6848_en-US.png "Select and display the time")

-   **Calculate the time** 

    To calculate the number of days since a CC attack, you can use the `__time__` field. Example:

    ``` {#codeblock_3zd_ovp_ghz}
    matched_host: www.aliyun.com  and cc_blocks: 1 
    |  select time, 
              round((to_unixtime(now()) - __time__)/86400, 1) as "days_passed",             real_client_ip, http_user_agent
          order by time desc
          limit 10
    ```

    **Note:** In the preceding example, `round((to_unixtime(now()) - __time__)/86400, 1)` is used for calculation. The `to_unixtime` function is used to convert the time obtained by `now()` into a Unix timestamp. The build-in `__time__` field subtracted from the calculated value is the number of seconds that have elapsed. The number of days since each CC attack equals the number of seconds divided by `86,400` and then rounded to the decimal by using the `round(data, 1)` function. 86,400 is the total number of seconds in a day.

    ![](images/6849_en-US.png "Query results")

-   **Group log entries by a built-in time** 

    To query CC attacks on a website every day in a specific time range, you can use the following SQL statements:

    ``` {#codeblock_4ed_xsm_2es}
    matched_host: www.aliyun.com  and cc_blocks: 1 
    | select date_trunc('day', __time__) as dt, 
             count(1) as PV 
          group by dt 
          order by dt
    ```

    **Note:** In the preceding example, the built-in `__time__` field is used in the `date_trunc('day', ...)` function to specify the time range of log entries as a day. Each log entry is assigned to a group based on the day when the log entry is generated. The total number of log entries in each group is counted by using count\(1\). These log entries are grouped and ordered by using the dt field. You can use other values for the first parameter of the `date_trunc` function to group log entries based on other time units, such as `second`, `minute`, `hour`, `week`, `month`, and `year`. For more information about time-related functions, see [Date and time functions](reseller.en-US/User Guide/Index and query/Analysis grammar/Date and time functions.md).

    ![](images/6850_en-US.png "Analysis results")

    The analysis results can be displayed in a line chart.

    ![](images/6851_en-US.png "Line chart")

-   **Group log entries by a self-defined time** 

    If you want to group log entries by a self-defined time, complex calculations are required. To query log entries of CC attacks on a website within every 5 minutes, you can use the following SQL statements:

    ``` {#codeblock_ecc_lcq_rti}
    matched_host: www.aliyun.com  and cc_blocks: 1 
    | select from_unixtime(__time__ - __time__% 300) as dt, 
             count(1) as PV 
          group by dt 
          order by dt 
          limit 1000
    ```

    **Note:** In the preceding example, the `__time__ - __time__% 300` expression contains the built-in time field, and the expression result is formatted by using the `from_unixtime` function. Each log entry is assigned to a group that indicates a time range of 5 minutes \(300 seconds\). The total number of log entries in each group is counted by using count\(1\). These log entries are grouped and ordered by using the dt field. The first 1,000 log entries are equivalent to the first 83 hours of log entries in the selected time range.

    ![](images/6852_en-US.png "Analysis results")

    The analysis results can be displayed in a line chart.

    ![](images/6853_en-US.png "Line chart")


For more information about time-related functions, see [Date and time functions](reseller.en-US/User Guide/Index and query/Analysis grammar/Date and time functions.md). For example, the `date_parse` and `date_format` functions can convert a time format to another format.

## Client IP address-based log query and analysis {#section_zr4_t2v_j2b .section}

In log entries of Anti-DDoS Pro, the `real_client_ip` field represents the real client IP address. However, if you cannot obtain the real IP address because a user use a proxy or the IP address in the header is incorrect, you can use the `remote_addr` field.

-   **Distribution of attackers by country** 

    You can use the following statements to analyze the source countries of CC attacks on a website:

    ``` {#codeblock_kd8_36d_ayq}
    matched_host: www.aliyun.com  and cc_blocks: 1 
    | SELECT ip_to_country(if(real_client_ip='-', remote_addr, real_client_ip)) as country, 
             count(1) as "number of attacks" 
             group by country
    ```

    **Note:** In the preceding example, the `if(condition, option1, option2)` function returns the real client IP address. If `real_client_ip` is `-`, the function returns the value of `remote_addr`. Otherwise, the function returns `real_client_ip`. The `ip_to_country` function is used to retrieve the country information corresponding to the client IP address.

    The analysis results can be displayed in a world map.

    ![](images/6855_en-US.png "World map")

-   **Distribution of visitors by province** 

    The following example shows how to use the `ip_to_province` function to obtain the distribution of visitors by province.

    ``` {#codeblock_ay1_6fl_5jw}
    matched_host: www.aliyun.com  and cc_blocks: 1 
    | SELECT ip_to_province(if(real_client_ip='-', remote_addr, real_client_ip)) as province, 
             count(1) as "number of attacks" 
             group by province
    ```

    **Note:** In the preceding example, the `ip_to_province` function is used to retrieve the origin \(province\) of the IP address. If the IP address is not in China, the system attempts to obtain the province or state where this IP address is located.

-   **Distribution of attackers in heatmap** 

    The following example shows how to use the `ip_to_geo` function to obtain the heatmap that indicates the distribution of attackers.

    ``` {#codeblock_klz_sg0_j8w}
    matched_host: www.aliyun.com  and cc_blocks: 1 
    | SELECT ip_to_geo(if(real_client_ip='-', remote_addr, real_client_ip)) as geo, 
             count(1) as "number of attacks" 
             group by geo
             limit 10000
    ```

    **Note:** In the preceding example, the `ip_to_geo` function is used to retrieve the latitude and longitude of the IP address. The limit is set at 10,000 to retrieve the first 10,000 records.

    ![](images/6858_en-US.png "Analysis results: distribution of attackers in heatmap")

    The analysis results can be displayed in a Amap.

    ![](images/6859_en-US.png "Amap")


For more information about IP-based functions, see [IP functions](reseller.en-US/User Guide/Index and query/Analysis grammar/IP functions.md). For example, you can use the `ip_to_provider` function to obtain the provider of IP addresses. You can use the `ip_to_domain` function to determine whether an IP address is public or private.

