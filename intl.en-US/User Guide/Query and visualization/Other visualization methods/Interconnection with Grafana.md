# Interconnection with Grafana {#concept_cfp_pnq_zdb .concept}

Alibaba Cloud Log Service is an all‑in‑one service for log‑type data. Users can leave trivial jobs like data collection, storage computing interconnection, and data index and query to Log Service to focus on the analysis. In September 2017, Log Service upgraded the LogSearch/Analytics, allowing real-time log analysis using query + SQL92 syntax.

Besides the built-in Dashboard, interconnections with DataV, Grafana, Tableua, and Quick are also available to visualize the results analysis. This article demonstrates how to analyze and visualize Nginx logs using Log Service by giving an example of Grafana.

## Process structure {#section_oxj_c1q_12b .section}

Process from log collection to analysis is structured as follows.

![](images/5856_en-US.png "Process structure")

## Procedure {#section_cwp_zzp_12b .section}

1.  Log data collection.  For detailed procedures, see [5-minute quick start](../../../../../reseller.en-US/Quick Start/5-minute quick start.md).
2.  Index settings and console query configuration. For detailed procedures, see [Overview](reseller.en-US/User Guide/Index and query/Overview.md) and [Analysis - Nginx access logs](../../../../../reseller.en-US/Quick Start/Analysis - Nginx access logs.md) or Website Log Analysis Case.
3.  Install the Grafana plug-in to convert real-time query SQL into views.

After completing Step 1 and 2, you can view the raw log on the query page.

This document demonstrates Step 3.

## Procedure {#section_zwh_yzp_12b .section}

1.  [Install Grafana](#section_nwf_txp_12b)
2.  [Install the Log Service plug-in](#section_sm4_wxp_12b)
3.  [Configure the log data source](#section_q3b_zxp_12b)
4.  [Add Dashboard](#section_r3v_3yp_12b)
    1.  [Configure the template variables](#section_wjd_lyp_12b)
    2.  [Configure PV and UV](#section_ccp_vyp_12b)
    3.  [Configure inbound and outbound bandwidth](#section_yqs_2zp_12b)
    4.  [Percentage of HTTP methods](#section_zsj_hzp_12b)
    5.  [Percentage of HTTP status codes](#section_azy_jzp_12b)
    6.  [Page of top sources](#section_hzq_lzp_12b)
    7.  [Pages of the maximum latency](#section_txv_mzp_12b)
    8.  [Top pages](#section_ur2_4zp_12b)
    9.  [Top pages of non-200 requests](#section_u3c_pzp_12b)
    10. [Average frontend and backend latency](#section_fzc_qzp_12b)
    11. [Client statistics](#section_sn1_rzp_12b)
    12. [Save and release the dashboard](#section_tlk_tzp_12b)
5.  [View the results](#section_nyj_5zp_12b)

## 1 Install Grafana {#section_nwf_txp_12b .section}

For detailed installation steps, see [Grafana official document](http://docs.grafana.org/installation/).

To Ubuntu, for example, the installation command is:

```
wget https://s3-us-west-2.amazonaws.com/grafana-releases/release/grafana_4.5.2_amd64.deb
sudo apt-get install -y adduser libfontconfig
sudo dpkg -i grafana_4.5.2_amd64.deb
```

To use the pie chart, you must install the pie chart plug-in.  For detailed procedures, see [Grafana official document](https://grafana.com/plugins/grafana-piechart-panel/installation).

The installation command is as follows:

```
grafana-cli plugins install grafana-piechart-panel
```

## 2 install the Log service plug-in {#section_sm4_wxp_12b .section}

Install the Log Service plug-in Confirm the directory location of Grafana plug-in. The location of Ubuntu plug-in is `/var/lib/grafana/plugins/` . Restart the grafana-server after installing the plug-in.

Taking Ubuntu system as an example, run the following command to install the plug-in and restart the grafana-server.

```
cd /var/lib/grafana/plugins/
git clone https://github.com/aliyun/aliyun-log-grafana-datasource-plugin
service grafana-server restart
```

## 3 Configure the log data source {#section_q3b_zxp_12b .section}

For the deployment in a local machine, the default installation location is Port 3000. Open the Port 3000 in a browser.

1.  Click on Grafana’s logo in the upper left corner and select Data Sources in the dialog box.
2.  Click **Add data source**, and use Grafana and Alibaba Cloud Log Service for log visual analysis.
3.  Complete the configuration items for the new data source.

    Each part is configured as follows:

    |Configuration item|Configuration content|
    |:-----------------|:--------------------|
    |datasource|The name can be customized and the type is **LogService**.|
    |Http Setting|URL input example: `http://dashboard-demo.cn-hangzhou.log.aliyuncs.com` The `dashboard-demo` \(project name\) and `cn-hangzhou.log.aliyuncs.com` \(endpoint of the project region\) must be replaced with your project and region addresses when configuring your data source. You can select Direct or Proxy for Access.|
    |Http Auth|Use the default configuration.|
    |log service details|For detailed configuration of Log Service, enter Project, Logstore, and AccessKey that has the read permission respectively. AccessKey can belong to the primary account or the subaccount.|

    Configuration example

    ![](images/5857_en-US.png "Configuration example")


After the configuration is complete, click **Add** to add DataSource. Next, add Dashboard.

##  4 Add Dashboard {#section_r3v_3yp_12b .section}

Click to open the menu in the upper left corner, select **Dashboards** and click **New**.  Add a new Dashboard to the menu in the upper left corner.

## 4. 1 Configure the template variables {#section_wjd_lyp_12b .section}

You can configure the template variables in Grafana to show different views in the same view by choosing different variable values.  This document describes the configuration of each time interval and the access of different domain names.

1.  Click the Settings icon at the top of the page, and then click **Templating**.
2.  On the current page, the configured template variables are displayed. Click **New** to create a new template. First, configure a time interval.

     The name of the variable is the one you used in the configuration, which is called $myinterval here. You must write `$myinterval`   in the query condition. Please refer to the following table for configuration.

    |Configuration items|Configuration content|
    |:------------------|:--------------------|
    |Name|The variable name, which you can name myinterval.|
    |Type|Please select `Interval`|
    |Lable.|Please enter`time interval`|
    |Internal Options|Please enter `1m,10m,30m,1h,6h,12h,1d,7d,14d,30d` in Value entry.|

3.  Configure a domain name template.

    Generally, multiple domain names can be mounted on one vps. You must view the accesses of different domain names. For the template value, enter `*,www.host.com,www.host0.com,www.host1.com`  to view all the domain names, or view the accesses of `www.host.com` , `www.host0.com` or `www.host1.com` respectively.

    The domain name template configuration as below.

    |Configuration items|Configuration content|
    |:------------------|:--------------------|
    |Name|Variable name, you can name it hostname.|
    |Type|Please select `Custom`|
    |Lable|Please enter a `domain name`|
    |Custom Options|Please enter `*,www.host.com,www.host0.com,www.host1.com` forValue entry.|

    After the configuration is complete, the template variables configured appears on the top of the Dashboard page. You can select any value from the drop-down box. For example, the following values can be selected for time interval.


## 4.2 Configure PV and UV {#section_ccp_vyp_12b .section}

1.  Click ADD ROW on the left to create a new Row. If a Row already exists, you can select Add Panel in the left dialog box.
2.  Grafana supports multiple types of views.  For PV and UV data, create a **Graph**view.
3.  Click **Pannel Title** and click **Edit** in the dialog box.
4.  In Metrics configuration, select `logservice` for datasource and enter Query, Y axis, and X axis:

     ![](images/5858_en-US.png "Configure PV and UV") 

5.  In the dataSource drop-down box, select the configured: `logservice`.

    |Configuration items|Configuration content|
    |:------------------|:--------------------|
    |Query |    ```
$hostname| select approx_distinct(remote_addr) as uv ,count(1) as pv , __time__ -
                  __time__ % $$myinterval as time group by time order by time limit
                1,000
    ```

The $hostname in the preceding Query is replaced with the domain name selected by the user for actual display. The $$myinterval is replaced with a time interval. Note that the number of $ before myinterval is two and for hostname is one.|
    |X-Column|time|
    |Y-Column|uv,pv|

    UV and PV values are displayed with two Y axes due to their large difference. Click the colored line on the left of UV below the icon to decide whether the UV is displayed on the left or the right Y axis:

    ![](images/5859_en-US.png "Y-axis display")

    The default view for the title is Panel  Title.  To modify, open the General tab and enter a new title in the **Title** configuration item, such as `PV&UV`.


## 4.3 Configure inbound and outbound bandwidth {#section_yqs_2zp_12b .section}

You can add inbound and outbound bandwidth traffic in the same way with [4.2 Configure PV and UV](#section_ccp_vyp_12b).

The main configuration items are as follows:

|Configuration items|Configuration content|
|:------------------|:--------------------|
|Query| ```
$hostname | select sum(body_byte_sent) as net_out, sum(request_length) as net_in
              ,__time__ - __time__ % $$myinterval as time group by __time__ - __time__ %
              $$myinterval limit 10000
```

 |
|X-Column|Time|
|Y-Column|net\_in,net\_out|

## 4.4 Percentage of HTTP methods {#section_zsj_hzp_12b .section}

You can configure the percentage of HTTP methods in the same way with [4.2 Configure PV and UV](#section_ccp_vyp_12b).

Create a new Row, select  **Pie Chart** , and enter Query, X axis, and Y axis in the configuration.

The main configuration items are as follows:

|Configuration items|Configuration content|
|:------------------|:--------------------|
|Query| ```
$hostname | select count(1) as pv ,method group by method
```

 |
|X-Column|pie|
|Y-Column|method,pv|

## 4.5 Percentage of HTTP status codes {#section_azy_jzp_12b .section}

You can configure the percentage of HTTP status codes in the same way with [4.2 Configure PV and UV](#section_ccp_vyp_12b).

Create a new Row, and select **Pie Chart** for the view.

The main configuration items are as follows:

|Configuration items|Configuration content|
|:------------------|:--------------------|
|Query| ```
$hostname | select count(1) as pv ,status group by status
```

 |
|X-Column|pie|
|Y-Column|status,pv|

## 4.6 Page of top sources {#section_hzq_lzp_12b .section}

You can configure the Page of top sources in the same way with [4.2 Configure PV and UV](#section_ccp_vyp_12b).

Create a new Row, and select **Pie Chart** for the view:

The main configuration items are as follows:

|Configuration items|Configuration content|
|:------------------|:--------------------|
|Query| ```
$hostname | select count(1) as pv , referer group by referer order by pv
            desc
```

 |
|X-Column|pie|
|Y-Column|referer,pv|

## 4.7 Pages of the maximum latency {#section_txv_mzp_12b .section}

You can configure the pages of the maximum latency in the same way with [4.2 Configure PV and UV](#section_ccp_vyp_12b).

To show URL and its latency in a table, you must specify the view as **Table** at the time of creation.

The main configuration items are as follows:

|Configuration items|Configuration content|
|:------------------|:--------------------|
|Query| ```
$hostname | select url as top_latency_url ,request_time order by request_time desc
              limit 10
```

 |
|X-Column|X-Column is left blank|
|Y-Column|top\_latency\_url,request\_time|

## 4.8 Top pages {#section_ur2_4zp_12b .section}

You can add top pages in the same way with [4.2 Configure PV and UV](#section_ccp_vyp_12b).

Create a new Table view. Data Source select logservice, query content, X-axis, and y-axis please refer to the following table.

|Configuration items|Configuration content|
|:------------------|:--------------------|
|Query| ```
$hostname | select count(1) as pv, split_part(url,'?',1) as path group by
              split_part(url,'?',1) order by pv desc limit 20
```

 |
|X-Column|X-Column is left blank|
|Y-Column|path,pv|

## 4.9 Top pages of non-200 requests {#section_u3c_pzp_12b .section}

You can add top pages of non-200 requests in the same way with [4.2 Configure PV and UV](#section_ccp_vyp_12b).

Create a new Table view. Data Source select logservice, query content, X-axis, and y-axis please refer to the following table.

|Configuration items|Configuration content|
|:------------------|:--------------------|
|Query| ```
$hostname not status:200| select count(1) as pv , url group by url order by pv
              desc
```

 |
|X-Column|X-Column is left blank|
|Y-Column|url,pv|

## 4.10 Average frontend and backend latency {#section_fzc_qzp_12b .section}

You can add average frontend in the same way with [4.2 Configure PV and UV](#section_ccp_vyp_12b).

Create a new Graph view: Data Source select logservice, query content, X-axis, and y-axis please refer to the following table.

|Configuration items|Configuration content|
|:------------------|:--------------------|
|Query| ```
$hostname | select avg(request_time) as response_time, avg(upstream_response_time) as
              upstream_response_time ,__time__ - __time__ % $$myinterval as time group by __time__ -
              __time__ % $$myinterval limit 10000
```

 |
|X-Column|time |
|Y-Column|upstream\_response\_time,response\_time|

## 4.11 Client statistics {#section_sn1_rzp_12b .section}

You can add client statistics in the same way with [4.2 Configure PV and UV](#section_ccp_vyp_12b).

Create a new Pie Chart. Data Source select logservice, query content, X-axis, and y-axis please refer to the following table.

|Configuration items|Configuration content|
|:------------------|:--------------------|
|Query| ```
$hostname | select count(1) as pv, case when regexp_like(http_user_agent , 'okhttp') then 'okhttp' when regexp_like(http_user_agent , 'iPhone') then 'iPhone' when regexp_like(http_user_agent , 'Android') then 'Android' else 'unKnown' end as http_user_agent group by http_user_agent order by pv desc limit 10
```

 |
|X-Column|pie|
|Y-Column|http\_user\_agent,pv|

## 4.12 Save and release the dashboard {#section_tlk_tzp_12b .section}

Click the Save button at the top of the page to release the Dashboard.

## 5 View the results {#section_nyj_5zp_12b .section}

Open the home page of Dashboard to view the results. Demo address: [Demo](http://47.96.36.117:3000/dashboard/db/nginxfang-wen-tong-ji?orgId=1).

At the top of the page, you can choose the time range or time granularity of statistics, or you can choose a different domain name.

Now, the configuration of Dashboard for Nginx access statistics is completed, enabling you to mine valuable information from your views.

![](images/5860_en-US.png "Check the result.")

