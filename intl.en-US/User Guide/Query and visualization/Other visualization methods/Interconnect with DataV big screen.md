# Interconnect with DataV big screen {#concept_mh2_rnq_zdb .concept}

People will think of the outstanding Tmall real-time big screen when talking about the Double 11 shopping campaign. The real-time big screen is impressive for its most typical stream computing architecture:

-   Data collection: Collect data from each source in real time.
-   Data collection: Collect data from each source in real time.
-   Real-time computing: Subscribe to real-time data and compute data in windows by using the computing rules. This is the most important part in the process.
-   Result storage: Store the computing results in SQL and NoSQL databases.
-   Visualization: Call the results by using APIs for demonstration.

In Alibaba Group, many mature products can be used to complete such work. The following figure shows the products generally used.

![](images/5861_en-US.png "Related products")

Besides the preceding solution, you can also use the LogSearch/Analytics APIs of Log Service to  directly interconnect with DataV to display data on a big screen.

![](images/5862_en-US.png "Log Service + DataV")

In September 2017, Log Service enhanced the real-time log analysis function \(LogSearch/Analytics\), which allows you to analyze logs in real time by using query and SQL92 syntax.  Besides the built-in dashboard, Log Service supports the interconnection methods such as Grafana and Tableau \(JDBC\) to achieve result analysis visualization.

## Features {#section_ls5_t2q_12b .section}

Based on the data volume, timeliness, and business needs, computing is generally divided into two modes:

-   Real-time computing \(stream computing\): Fixed computing + variable data.
-   Offline computing \(data warehouse + offline computing\): Variable computing + fixed data.

Log Service provides two interconnection methods to collect data in real time. In addition, in log analysis scenarios that has timeliness needs, LogHub data can be indexed in real time. Then, you can use LogSearch/Analytics to directly query and analyze data.  This method has the following advantages:

-   Fast: You can obtain the results immediately after query is passed in by using APIs, without waiting or pre-computing the results.
-   Real-time: In 99.9% cases, the generated logs are displayed on the big screen within 1s.
-   Dynamic: Whether statistic method modification or supplementary data, the display results are refreshed in real time, without waiting for recomputation.

However, no computing system is omnipotent. This method has the following limits:

-   Data volume: Up to 10 billion GB data can be computed at a time. You must set the time limit if the data volume is exceeded.
-   Computing flexibility: Currently, only the SQL92 syntax is supported for computing. Custom UDF is unsupported.

![](images/5863_en-US.png "Log service advantage")

## Configuration process {#section_awp_w2q_12b .section}

Operation Demonstration:

To interconnect Log Service data with DataV big screen, follow these steps:

1.  Collect data. See [5-minute quick start](../../../../../reseller.en-US/Quick Start/5-minute quick start.md#) to access the data source to Log Service.
2.  Set the index See [Index settings and visualization](reseller.en-US/User Guide/Index and query/Overview.md#) or Use case for [website log analysis](../../../../../reseller.en-US/Quick Start/Analysis - Nginx access logs.md#) in Best Practices.
3.  Interconnect with the DataV plug-in to convert the real-time results queried by using the SQL statement to a view.

After completing steps 1 and 2, you can view the raw logs on the search page. This document mainly describes how to perform step 3.

## Procedure {#section_sxv_y2q_12b .section}

****

## Step 1 Create a DataV data source {#section_ipp_z2q_12b .section}

Click **Data Sources** in the left-side navigation pane. Click **Add Source**. The New Data Source dialog box appears.  Enter the basic information of the data source. The following table describes the definition of each configuration item.

![](images/5865_en-US.png "New data")

|Configuration item |Description |
|:------------------|:-----------|
|Type |Select **Log Service**.|
|Name|Configure a name for the data source.|
|AK ID|The AccessKey ID of the main account, or the AccessKey ID of the sub-account that has the permission to read Log Service.|
|AK Secret|The AccessKey Secret of the main account, or the AccessKey Secret of the sub-account that has the permission to read Log Service.|
|Endpoint|The address of the region where the Log Service project resides. In the preceding figure, the address of region Hangzhou is entered.|

## Step 2 Create a line chart {#section_ynp_z2q_12b .section}

1.  Create a line chart.

    In the data configuration of the line chart, set the data source type to **Log Service**, select the data source **log\_service\_api** created in the previous step, and enter the parameters in the **Query** text box.

    ![](images/5866_en-US.png "Data source")

    An example of the query parameters is as follows and the following table describes the parameters.

    ```
    {
     "projectName": "dashboard-demo",
     "logStoreName": "access-log",
     "topic": "",
     "from": ":from",
     "to": ":to",
     "query": "*| select approx_distinct(remote_addr) as uv ,count(1) as pv , date_format(from_unixtime(date_trunc('hour',__time__) ) ,'%Y/%m/%d %H:%i:%s') as time group by time order by time limit 1000" ,
     "line": 100,
     "offset": 0
    }
    ```

    |Configuration item|Description|
    |:-----------------|:----------|
    |projectName|The name of your project.|
    |logstoreName|The name of your Logstore.|
    |topic|Your log topic. If you have not set the topic, leave the parameter value empty.|
    |from、to|from and to specify the start time and end time of the log respectively.**Note:** In the preceding example, the parameter values are respectively set to : `:from` and `:to`. During the test, you can enter the time in UNIX  format, for example, 1509897600. After the release, convert the time to `:from` and `:to`, and set the specific time ranges of the values in the URL parameter.  For example, the previewed URL is `http://datav.aliyun.com/screen/86312`. After `http://datav.aliyun.com/screen/86312?from=1510796077&to=1510798877` is opened, the values are computed based on the specified time.

|
    |query| Your query condition. In the preceding example, the query condition is the pv quantity per minute.  For more information about the query syntax, see [Syntax description](reseller.en-US/User Guide/Index and query/Syntax description.md).**Note:** The time in the query must be in the format like 2017/07/11  12:00:00. Therefore, use date\_format\(from\_unixtime\(date\_trunc\('hour',\_\_time\_\_\) \) ,'%Y/%m/%d %H:%i:%s'\) to align the time on the hour, and then convert it to the target format.

    ```
date_format(from_unixtime(date_trunc('hour',__time__) ) ,'%Y/%m/%d
                    %H:%i:%s')
    ```

|
    |line|Enter the default value 100.|
    |offset|Enter the default value 0.|

    After the configurations, click **View Data Response**.

    ![](images/5867_en-US.png "View Data Response")

2.  Create a filter.

    The Data Response Result dialog box appears after you click **View Data Response**. Select the Use Filter check box and click Select Filter \> New Filter to create a filter.

    Enter the filter content in the following format:

    ```
    return Object.keys(data).map((key) => {
    let d= data[key];
    d["pv"] = parseInt(d["pv"]);
    return d;
    }
    )
    ```

    In the filter, convert the result used by y-axis to the int type. In the preceding example, the y-axis indicates the pv. Therefore, the pv column must be converted.

    The results contain both the t and pv columns. You can set the x-axis to t and the y-axis to pv.


## Step 3 Configure a pie chart {#section_sx2_sfq_12b .section}

1.  Create a **carousel pie chart**.

    ![](images/5869_en-US.png "Query text box")

    Enter the following contents in the Query text box:

    ```
    {
     "projectName": "dashboard-demo",
     "logStoreName": "access-log",
     "topic": "",
     "from": 1509897600,
     "to": 1509984000,
     "query": "*| select count(1) as pv ,method group by method" ,
     "line": 100,
     "offset": 0
    }
    ```

    During the query, the ratios of different methods can be computed.

2.  Add a filter and enter the following contents in the filter:

    ```
    return Object.keys(data).map((key) => {
    let d= data[key];
    d["pv"] = parseInt(d["pv"]);
    return d;
    }
    )
    ```

    Enter method in the type text box and pv in the value text box for the pie chart.


## Step 4 Preview and release {#section_yyj_vfq_12b .section}

Click **Preview and Publish** to create a big screen.  Developers and business personnel can view their business access conditions in real time in the Double 11 shopping campaign.

Trial: [Demo](http://datav.aliyun.com/share/61e3690511158708bd68c27342c51c77?from=1510796077&to=1510798877). You can set the values of the parameters from and to in the URL to any time.

![](images/5870_en-US.png "Real-Time Screen")

## Use case: Continuously adjust the real-time big screen under the statistic criteria {#section_vd4_yfq_12b .section}

For example, a temporary requirement is raised during the Computing Conference, which is to count the online \(website\) traffic across China.  Total log data collection is configured and LogSearch/Analytics is enabled in Log Service. Therefore, you only need to enter your query condition.

1.  For example, to count the UV, obtain the unique count of the forward field under Nginx in all access logs from October 11 to the present.

    ```
    * | select approx_distinct(forward) as uv
    ```

2.  After the system runs online for one day, the requirement is changed. Currently, only data under the domain yunqi needs to be counted.  You can add a filter condition \(host\) for real-time query.

    ```
    host:yunqi.aliyun.com | select approx_distinct(forward) as uv
    ```

3.  It is detected that the Nginx access logs contain multiple IP addresses. By default, only the first IP address is required. Therefore, process the query condition in the query.

    ```
    host:yunqi.aliyun.com | select approx_distinct(split_part(forward,',',1)) as uv
    ```

4.  According to the requirement in the third day, the advertisement access in uc must be removed from access computing.  In this case, you can add a filter condition not ... to obtain the latest result immediately.

    ```
    host:yunqi.aliyun.com not url:uc-iflow | select approx_distinct(split_part(forward,',',1)) as uv
    ```

     


