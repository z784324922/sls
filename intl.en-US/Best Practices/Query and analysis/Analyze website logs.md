# Analyze website logs {#concept_fzr_jl1_wfb .concept}

Website access logs are vital to webmasters. The access information of a website includes the page views \(PVs\), unique visitors \(UVs\), regions of visitors who access the website, and top 10 pages with most visits. Application logs are essential to application developers. Analysis and optimization of the SQL ORDER BY and LIMIT statements can improve application quality. O&M engineers can monitor server logs and locate exceptions. By monitoring logs in real time, engineers can obtain the server response time changes in the last hour and check whether the traffic is normal when a request is sent from a client to a machine for load balancing. Engineers can also view the monitoring data and obtain key information through the dashboard.

To meet the requirements of the preceding scenarios, Log Service provides a one-stop solution for log data collection and analysis. The LogSearch and Analytics features allow you to analyze logs in real time by using the query statements and SQL-92 statements. Log Service supports multiple visualization methods, including the built-in dashboards and open-source visualization tools such as [DataV](../../../../reseller.en-US/Index and query/Overview.md#), Grafana, Tableau through Java Database Connectivity \(JDBC\), and QuickBI.

Log Service provides data analysis charts to display the real-time log query and analysis results. In addition, Log Service provides dashboards for displaying the log data analysis result in different scenarios.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13211/156894515632501_en-US.png)

[Click here for a trial](https://signin.aliyun.com/1654218965343050/login.htm?spm=a2c4e.11153940.blogcont186380.13.1a9a56c7m4cqrm&callback=https%3A%2F%2Fsls.console.aliyun.com%2Fnext%2Fproject%2Fdashboard-demo%2Flogsearch%2Faccess-log)

Username: sls\_reader1@\*\*\*\*\*\*\*\*\*\*\*\*

Password: pnX-32m-MHH-xbm

## Features {#section_sz5_d2c_wfb .section}

-   No need to define charts in advance: You can apply any computing method and any filter condition to logs generated in any time period. The corresponding chart appears within seconds.
-   Interactive analysis: Seamless switching is supported between charts and raw logs.
-   Scenario-specific solution: You can view the analysis dashboard through the data import wizard without complex configurations.

## Chart types {#section_krj_h2c_wfb .section}

The following figure shows the chart types that are supported by the visualization feature of Log Service.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13211/156894515632502_en-US.png)

## Process and architecture {#section_ubs_j2c_wfb .section}

1.  Collect data. Log Service supports multiple methods to collect logs, such as by using clients, webpages, protocols, and SDKs or APIs \(for mobile apps and games\). All collection methods are implemented based on RESTful APIs. You can also implement new collection methods by using APIs and SDKs.
2.  Set indexes and use query and analysis statements to query and analyze logs.
3.  Visually display data. Log Service provides RESTful APIs. You can use an appropriate method to visualize your log data. The following section describes the visualization and dashboard features of Log Service.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13211/156894515632503_en-US.png)

## Examples {#section_p4x_x2c_wfb .section}

This section describes the typical use cases of different chart types. Before query and analysis, enable and set indexes. For more information, see [Enable and set indexes](../../../../reseller.en-US/Index and query/Enable and set indexes.md#).

1.  **Table** 

    A table, as the most common visual representation of data, consists of one or more groups of cells. It is used to display numbers and other items for quick reference and analysis. The items in a table are organized as rows and columns. The first row of a table is called the header, indicating the content and meaning of each column in the table.

    In Log Service, the result returned by the query and analysis statements is displayed in tables by default.

    Run the following statement to view the distribution of source IP addresses in the current time range in descending order:

    ``` {#codeblock_uzr_umc_3q7}
    * | SELECT sourceIPs, count(*) as count GROUP BY sourceIPs ORDER BY count DESC
    ```

    The following figure shows the result table. You can click the sort icon in the header to sort a column in a certain order.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13211/156894515632504_en-US.png)

2.  **Line chart** 

    A line chart analyzes trends. It is typically used to indicate the changes of a group of data based on an ordered data type \(successive time intervals in most cases\) for analyzing the trend of data changes.

    Run the following statement to analyze the changes of PVs, UVs, and average response time in the last 15 minutes:

    ``` {#codeblock_n7n_3g0_bo6}
    * | select date_format(from_unixtime(__time__ - __time__% 60), '%H:%i:%S') as minutes, approx_distinct(remote_addr) as uv, count(1) as pv, avg(request_time) as avg group by minutes order by minutes asc limit 100000
    ```

    Select minutes for X Axis, PV and UV for Left Y Axis, avg for Right Y Axis, and UV for Column Marker. The following figure shows a sample line chart.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13211/156894515632505_en-US.png)

3.  **Column chart** 

    A column chart uses vertical or horizontal columns to compare the numeric data among different types. A line chart describes the ordered data, while a column chart describes different types of data and counts the number in each data type. For more information about the line chart, see [Line chart](../../../../reseller.en-US/Index and query/Query and visualization/Analysis graph/Line chart.md#).

    Run the following statement to analyze the number of visits for each `http_referer` in the last 15 minutes:

    ``` {#codeblock_3mn_qvr_y1g}
    * | select http_referer, count(1) as count group by http_referer
    ```

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13211/156894515632507_en-US.png)

4.  **Bar chart** 

    A bar chart, also known as a horizontal bar chart, is suitable for analyzing the ranking of different categories.

    Run the following statement to analyze the top 10 `request_uri` with the most visits in the last 15 minutes:

    ``` {#codeblock_qva_8iz_qm7}
    * | select  request_uri, count(1) as count group by request_uri order by count desc limit 10    
    ```

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13211/156894515632508_en-US.png)

5.  **Pie chart** 

    A pie chart is used to indicate the ratios of different data types and compare different data types based on the arc length. A pie chart is divided into multiple sectors based on the percentages of various data types. The entire chart indicates the sum of data. Each sector \(arc-shaped\) indicates the ratio of a data type to the sum. The sum of percentages in all sectors is equal to 100%.

    Run the following statement to analyze the visit distribution of different request URIs in the last 15 minutes:

    ``` {#codeblock_rva_7up_jkp}
    * | select requestURI as uri , count(1) as c group by uri limit 10
    ```

    Pie chart:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13211/156894515732509_en-US.png)

    Donut chart:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13211/156894515732510_en-US.png)

    Polar area chart:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13211/156894515732511_en-US.png)

6.  **Single value chart** 

    A single value chart, as the easiest and most intuitive display type of data, shows the data on a point clearly and intuitively, and is generally used to indicate the key information on a time point.

    Run the following statement to analyze the PVs in the last 15 minutes:

    ``` {#codeblock_rkr_75a_5ty}
    * | select count(1) as PV
    ```

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13211/156894515732512_en-US.png)

7.  **Area chart** 

    Based on a line chart, an area chart fills the section between a line and the axis with color. The filled section is an area block and the color highlights the trend.

    Run the following statement to collect statistics about the PVs of the IP address `10.0.XX.XX` on the last day:

    ``` {#codeblock_act_jll_jie}
    remote_addr: 10.0.XX.XX | select date_format(date_trunc('hour', __time__), '%m-%d %H:%i') as time, count(1) as PV group by time order by time limit 1000
    ```

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13211/156894515732513_en-US.png)

8.  **Map chart** 

    You can add color blocks and marks to a map to display geographic data. Log Service provides three kinds of maps: Map of China, World Map, and AMap. Among them, AMap offers the scatter chart and heat map.

    You can use the remote\_addr parameter in the statements to make three types of map charts. Run the following statements to display the top 10 regions with the most visits:

    -   Map of China

        ``` {#codeblock_szx_mth_24h}
        * | select ip_to_province(remote_addr) as address, count(1) as count group by address order by count desc limit 10
        								
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13211/156894515732514_en-US.png)

    -   World Map

        ``` {#codeblock_xau_nhg_9c4}
        * | select ip_to_country(remote_addr) as address, count(1) as count group by address order by count desc limit 10
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13211/156894515732515_en-US.png)

    -   AMap

        ``` {#codeblock_q1n_mgx_5ce}
        * | select ip_to_geo(remote_addr) as address, count(1) as count group by address order by count desc limit 10
        								
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13211/156894515732516_en-US.png)

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13211/156894515732517_en-US.png)

9.  **Flow chart** 

    The banded branches with different colors indicate different types of information. The band width indicates the corresponding numeric value. In addition, the centralized time attribute of the original data maps to the X-axis, which forms a three-dimensional relationship.

    Run the following statement to display the trends of the requests sent through different methods in the last 15 minutes:

    ``` {#codeblock_rtd_5av_v5h}
    * | select date_format(from_unixtime(__time__ - __time__% 60), '%H:%i:%S') as minute, count(1) as c, request_method group by minute, request_method order by minute asc limit 100000
    ```

    Select minute for X Axis, c for Y Axis, and request\_method for Aggregate Column.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13211/156894515732518_en-US.png)

10. **Sankey diagram** 

    A Sankey diagram is a specific type of flow chart. It is used to describe the flow from one set of values to another. Sankey diagrams are applicable to scenarios such as network flow data. Generally, a Sankey diagram contains three sets of values: source, target, and value. The source and target describes the edge relationship between nodes, and value describes the relationship between source and target.

    Run the following statement to analyze the flow data in a load balancing scenario:

    ``` {#codeblock_9g4_3pq_7u0}
    * | select sourceValue, targetValue, streamValue group by sourceValue, targetValue, streamValue order by streamValue
    ```

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13211/156894515732519_en-US.png)

11. **Word cloud** 

    A word cloud visualizes text data. It is a cloud-like and colored image composed of words. It can be used to display a large amount of text data. The importance of each word is shown with its font size or color. This allows you to quickly perceive the weight of some keywords.

    Run the following statement to analyze the visits to different request URIs in the last 15 minutes:

    ``` {#codeblock_e6o_9a4_9sj}
    * | select requestURI as uri , count(1) as c group by uri limit 100
    ```

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13211/156894515732520_en-US.png)


## Add charts to a dashboard {#section_xdg_23c_wfb .section}

All charts obtained through the query and analysis statement can be saved in a dashboard. Then, you can adjust the layout of these charts to get a comprehensive dashboard.

You can click **Add to New Dashboard** to create a dashboard. After creating a dashboard, you can open the dashboard based on tags to view data in real time.

