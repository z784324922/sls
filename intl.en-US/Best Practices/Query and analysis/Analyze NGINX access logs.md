# Analyze NGINX access logs {#concept_64278_zh .concept}

Currently, Log Service supports saving query statements through **Saved Search**. Log Service also supports setting the trigger cycle \(interval\) for queries, setting judgment conditions for execution results, and reporting alerts. You can set an alerting action to specify the way to inform you when the execution result of a regular Saved Search operation meets the trigger conditions.

Currently, the following three notification methods are supported:

-   Notification center: Multiple contacts can be set in the Alibaba Cloud notification center. You can send notifications to contacts through emails and SMS messages.
-   WebHook: including DingTalk Chatbot and custom WebHook.
-   \(Coming soon\) Writing back to Log Service Logstores: You can subscribe to events through Realtime Compute and Function Compute, or generate views and reports for alerts.

For more information about how to configure the alerting feature, see [Configure an alarm](../../../../intl.en-US/Alarm/Configure an alarm/Configure an alert.md#). In addition to the monitoring and alerting features of Log Service, you can also use CloudMonitor to monitor all metrics of Log Service. CloudMonitor can send you a notification when the alerting condition is triggered.

![Alert notifications](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13215/156894163132470_en-US.png)

## Scenarios {#section_izx_jnb_wfb .section}

This section takes NGINX logs as an example to describe how to query and analyze collected logs regularly through Log Service and determine the following business issues based on the query result:

-   Whether an error exists.
-   Whether a performance problem exists.
-   Whether a sudden decrease or increase of the traffic exists.

## Preparation \(NGINX log access\) {#section_dpv_mnb_wfb .section}

1.  Collect log data.
    1.  On the **Overview** page, click Import Data in the upper-right corner. In the dialog box that appears, click **NGINX Access Log**.
    2.  Select a Logstore.

        If you enter the log collection configuration process by clicking the + icon next to **Data Import** under a Logstore on the Logstores tab, the system skips this step.

    3.  Create a machine group.

        Before creating a machine group, make sure that you have installed Logtail.

        -   Machines of Alibaba Group: By default, Logtail is installed for these machines. If Logtail is not installed on a machine, contact us as prompted.
        -   ECS instances: Select an ECS instance, and click **Install**. ECS instances running Windows do not support one-click installation of Logtail. In this case, you need to manually install Logtail. For more information, see [Install Logtail in Windows](../../../../intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Windows.md).
        -   User-created machines: Install Logtail as prompted. For more information about how to install Logtail, see [Install Logtail in Linux](../../../../intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Linux.md#) or [Install Logtail in Windows](../../../../intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Windows.md#) based on your operating system.
        After installing Logtail, click **Confirm Installation** to create a machine group. If you have created a machine group, click **Use Existing Machine Group**.

    4.  Configure the machine group.

        Select a machine group and move the machine from **Source Machine Group** to **Application Machine Group**.

    5.  Specify the following configuration items: Configuration Name, Log Path, NGINX Log Format, and NGINX Key. You can specify Advanced Options based on your needs.
    6.  Click **Next**.
2.  Complete query and analysis configurations.

    For more information, see [Enable and set indexes](../../../../intl.en-US/Index and query/Enable and set indexes.md#), [Interconnect with DataV big screen](../../../../intl.en-US/Index and query/Query and visualization/Other visualization methods/Interconnect with DataV big screen.md#), or [Collect and analyze NGINX access logs](../../../../intl.en-US/Quick Start/Collect and analyze Nginx access logs.md#).

3.  Set the views and alerts for key metrics.

Sample views:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13215/156894163332472_en-US.png)

## Procedure {#section_3nl_c06_njp .section}

## 1. Determine if any error exists {#section_9x3_36b_hms .section}

The common error codes are as follows: 404 \(the request cannot find the address\), 502, and 500 \(an error occurs with the server\). Generally, we only focus on 500 errors.

Determine if a 500 error exists. You can run the following query statement to count the number of errors \(c\) per unit time. Then, you can set the alert rule as c \> 0, indicating that an alert will be sent when the number of 500 errors exceeds 0 in the unit time.

``` {#codeblock_j6q_oi1_9ev}
status:500 | select count(1) as c
```

This method is relatively simple but too sensitive. For services facing relatively high business pressure, a few 500 errors are common. In response to this situation, you can set the trigger count to 2 in the trigger conditions so that alerts are only triggered when the conditions are met for 2 times in a row.

## 2. Determine if any performance problem exists {#section_yn6_037_vu4 .section}

Although no error occurs in server operation, the latency might be increased. You can set an alert for latency.

For example, you can calculate the latency of all the write requests \("Post"\) of an interface \("/adduser"\) through the following method: Set the alert rule as l \> 300000, indicating that an alert will be sent when the average latency exceeds 300 ms.

``` {#codeblock_aku_orh_ibe}
Method:Post and URL:"/adduser" | select avg(Latency) as l
```

Sending alerts based on the average latency is simple and direct. However, this method may average the latency of individual requests, making it difficult to detect problems. For example, you can compute a mathematical distribution for the latency in the time period, namely, dividing the latency into 20 intervals and calculating the number in each interval. As shown in the histogram, the latency of most requests is lower than 20 ms, but the highest latency reaches 2.5s.

``` {#codeblock_s7e_4cf_xyj}
Method:Post and URL:"/adduser" | select numeric_histogram(20, Latency)
```

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13215/156894163832473_en-US.png)

You can use the percentile in mathematical statistics \(the maximum latency is 99%\) as the trigger condition. In this way, you can exclude false alerts triggered by accidental high latency and reflect the overall situation of the latency. The following statement calculates the latency of the 99th percentile, approx\_percentile \(Latency, 0.99\). You can also change the second parameter to calculate the latency of other percentiles, for example, the request latency of the 50th percentile, approx\_percentile \(Latency, 0.5\).

``` {#codeblock_4d3_71a_w2f}
Method:Post and URL:"/adduser" | select approx_percentile(Latency, 0.99) as p99
```

In a monitoring scenario, you can chart the average latency, the 50th percentile latency, and the 90th percentile latency. The following figure shows the latency of every minute in a day \(1,440 minutes\).

``` {#codeblock_49y_0pi_yld}
* | select avg(Latency) as l, approx_percentile(Latency, 0.5) as p50, approx_percentile(Latency, 0.99) as p99, date_trunc('minute', time) as t group by t order by t desc limit 1440
```

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13215/156894163932474_en-US.png)

## 3. Determine if the traffic has a sudden decrease or increase {#section_ct9_pra_nin .section}

The natural traffic on the server is usually in line with probability distribution, which means that a process of slow increase or decrease exists. The sudden decrease or increase of the traffic indicates great changes in a short time period. This phenomenon is usually abnormal and needs special attention.

As shown in the following monitoring chart, the traffic decreases by over 30% within 2 minutes and resumes rapidly within 2 minutes.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13215/156894164132475_en-US.png)

The following reference frames are provided for a sudden decrease or increase:

-   Last window: compares data in the current time period with that in the previous time period.
-   Window of the same time period of the previous day: compares data in the current time period with that in the same time period of the previous day.
-   Window of the same time period of the previous week: compares data in the current time period with that in the same time period of the previous week.

This section takes the first reference frame as an example to calculate the change ratio of inbound traffic. You can also calculate other metrics of the traffic such as queries per second \(QPS\).

## 3.1 Define a calculation window {#section_dc6_gu3_rnm .section}

Define a window of 1 minute to calculate the inbound traffic size of this minute. The following figure shows the statistic result within a 5-minute interval.

``` {#codeblock_nl3_o6b_xw0}
* | select sum(inflow)/(max(__time__)-min(__time__)) as inflow , __time__-__time__%60  as window_time from log group by window_time order by window_time limit 15
```

As shown in the result, the average inbound traffic size specified by `sum(inflow)/(max(__time__)-min(__time__))` in every window is even.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13215/156894166332476_en-US.png)

## 3.2 Calculate the difference in the window \(max\_ratio\) {#section_2il_22o_twk .section}

Subqueries are involved. Run a query statement to calculate the change ratio between the maximum value or the minimum value and the average value from the preceding result. In this example, the change ratio between the maximum value and the average value is calculated, for example, 1.02. You can set the alert rule as max\_ratio \> 1.5, indicating that an alert will be sent when the ratio of change exceeds 50%.

``` {#codeblock_t3z_brp_fuy}
 * | select max(inflow)/avg(inflow) as max_ratio from (select sum(inflow)/(max(__time__)-min(__time__)) as inflow , __time__-__time__%60  as window_time from log group by window_time order by window_time limit 15) 
```

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13215/156894166332477_en-US.png)

## 3.3 Calculate the difference in the window \(latest\_ratio\) {#section_8jw_tzr_kac .section}

In some scenarios, more attention is paid to the fluctuation of the latest value \(whether the value is recovered\). You can use the max\_by function to get the inbound traffic size of the latest window \(specified by windows\_time\). Then, you can calculate the change ratio between the latest value and the average value, for example, 0.97.

``` {#codeblock_t9q_b5r_hr2}
 * | select max_by(inflow, window_time)/1.0/avg(inflow) as lastest_ratio from (select sum(inflow)/(max(__time__)-min(__time__)) as inflow , __time__-__time__%60  as window_time from log group by window_time order by window_time limit 15) 
```

**Note:** The calculation result of the max\_by function is of the character type, which must be converted to the numeric type. To calculate the relative ratio of changes, you can replace it with 1.0-max\_by\(inflow, window\_time\)/1.0/avg\(inflow\)\) as lastest\_ratio.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13215/156894166332478_en-US.png)

## 3.4 Calculate the difference in the window which indicates the fluctuation ratio, namely, the change ratio between the current value and the previous value {#section_yvw_701_u3a .section}

Another method for calculating the fluctuation ratio is the first derivative in mathematics, namely, the change ratio between the value of the current window and the value of the previous window.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13215/156894166432479_en-US.png)

Use the window function \(lag\) for calculation. Extract the current inbound traffic and the inbound traffic of the previous window to calculate the difference by using lag\(inflow, 1, inflow\)over\(\) and divide the calculated difference value by the current value to get the change ratio.

``` {#codeblock_ato_uin_b6d}
 * | select (inflow- lag(inflow, 1, inflow)over() )*1.0/inflow as diff, from_unixtime(window_time) from (select sum(inflow)/(max(__time__)-min(__time__)) as inflow , __time__-__time__%60  as window_time from log group by window_time order by window_time limit 15) 
```

In this example, a relatively major decrease occurs in traffic at 11:39, with a change ratio of over 40%.

To define an absolute change ratio, you can use the abs function \(absolute value\) to unify the calculation result.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13215/156894166432480_en-US.png)

## Summary {#section_wpd_re4_oz0 .section}

The query and analysis features of Log Service follow the SQL-92 standard and support various mathematical statistics and computing methods. Anyone who can use SQL can perform fast analysis. Have a try!

