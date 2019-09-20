# Use Function Compute to cleanse log data {#concept_61721_zh .concept}

LogHub of Log Service is a streaming data center. Logs can be consumed in real time after being written to LogHub. The extract-transform-load \(ETL\) process of Log Service can process such streaming data in seconds.

## Overview {#section_hlf_gzk_5fb .section}

ETL is the process of extracting data from the source, transforming data, and then loading data to the destination. The traditional ETL process is an important part in building a data warehouse. You need to extract required data from the data source, cleanse the data, and then load data to the destination data warehouse based on the pre-defined data warehouse model. Under the ever-growing business requirements, different systems need to exchange large amounts of data. Data flows in different systems. This is helpful in fully exploring the value of log big data.

## Benefits of the Log Service and Function Compute ETL process {#section_rns_3zk_5fb .section}

-   Data is collected, stored, processed, and analyzed in a uniform manner.
-   Data processing is fully managed, triggered by time, and automatically retried.
-   Shards can be scaled out to meet the resource requirements of big data.
-   Data is processed in Function Compute, which provides elastic resources and supports the pay-as-you-go billing method.
-   The ETL process is transparent to users and provides the logging and alerting features.
-   Built-in function templates are continuously added to reduce the cost of function development under mainstream requirements.

## Scenarios {#section_oqj_4zk_5fb .section}

## Data cleansing and processing {#section_ypq_j15_6fz .section}

Log Service allows you to quickly collect, process, query, and analyze logs.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13202/156896157532403_en-US.png)

## Data shipping {#section_6iu_1lp_fra .section}

You can ship data to the destination and build data pipelines between big data products in the cloud.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13202/156896157532404_en-US.png)

## ETL model {#section_bsd_n1l_5fb .section}

The ETL model is a stream-based, real-time data stream processing model. The ETL trigger polls all shards in the source Logstore to detect the position where data is written and regularly generates trituple information to trigger relevant functions. The trituple information is used to identify the range of data to be processed in the current ETL task.

Shards can be scaled out to ensure the auto scaling of the ETL process. Tasks are triggered by a timer to ensure that data is continuously loaded.

During the implementation of the ETL process, in consideration of the flexibility of user-defined functions \(UDFs\), functions are called in Function Compute to process data. Function Compute provides pay-as-you-go, auto scaling, and custom code execution to meet the requirements of many cloud users. In addition, considering the end-to-end latency of user data, big data throughput, and SQL usability, the Log Service ETL runtime will involve stream computing engines, such as Alibaba Cloud StreamCompute, in the future to meet more user requirements.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13202/156896157632405_en-US.png)

## ETL logs {#section_rsd_n1l_5fb .section}

-   ETL process logs

    ETL process logs record the key points and errors of each step during the implementation of the ETL process, including the start time and end time of a step, completion status of initialization, and module error information. The purpose of recording process logs is to know the status of the ETL process at any time and the causes of errors. Logs generated during the execution of functions record the key points and exceptions of data processing.

-   ETL scheduling logs

    ETL scheduling logs record only the start time and end time of an ETL task, whether the task is successful, and the information returned when the task is successful. If an error occurs, ETL error logs are generated, and an alert email or SMS message is sent to the system administrator. Based on scheduling logs, reports can be created to display the overall status of the ETL process.


## Application examples {#section_4ua_1u5_znf .section}

Software built based on HTTP servers such as Nginx and Apache can record access logs for each user. Using the Log Service and Function Compute ETL process, you can analyze the regions where your services are used and the links used by users in these regions to access your services.

For data analysis engineers, the workload of the ETL process accounts for 60% to 70% of the overall workload of a project. Log Service can use built-in function templates to shorten the duration of the ETL process to at most 15 minutes.

## Step 1: Centralize log storage {#section_7s2_juy_2j5 .section}

You can use the Logtail client of Log Service to quickly collect log files from a machine, for example, Nginx.

Nginx access logs collected by the client are all stored in a Logstore of Log Service. As shown in the following figure, the value of the forward field indicates the IP address from which the user sends the request.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13202/156896157632406_en-US.png)

## Step 2: Process data in the cloud {#section_2lg_cvc_lrg .section}

1.  Log on to the Function Compute console and create a service.

    In the advanced configuration section, we recommend that you configure a Logstore to store logs generated during data processing for the ETL function to be created, so that you can use the logs to locate exceptions during data processing. Grant the AliyunLogFullAccess permission of Log Service to the ETL function. Then, the function can read data from the source Logstore during execution and write the data to the destination Logstore after data processing.

    ![](images/32407_en-US.png)

2.  Use a built-in template to create the ETL function.

    ![](images/32408_en-US.png)

    The following figure shows the default function configuration.

    ![](images/32409_en-US.png)

3.  Create a Log Service trigger for the function.

    The following figure shows the configuration of the Log Service trigger.

    ![](images/32410_en-US.png)

    Set the data source to the Logstore that stores Nginx access logs as described in step 1, that is, `etl-test/logstore:nginx_access_log` in this example.

    Log Service polls data in the source Logstore. When data is continuously generated, an ETL task is created every 60 seconds to call the ETL function and process data. The trigger and execution results of the function are recorded in the etl-trigger-log Logstore that stores trigger logs.

    The implementation and features of functions vary with the function configuration. For more information about the configuration items of the ip-lookup template, see its README file.

4.  Save the configuration and wait 1 minute for the ETL task to start.

    Pay attention to the generated ETL process logs and scheduling logs. Based on the preceding configuration, the two types of logs are stored in the etl-function-log and etl-trigger-log Logstores, respectively.

    You can also use query statements to create reports based on ETL logs.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13202/156896157632411_en-US.png)

    -   The chart in the upper-left corner shows the number of times that the ETL function is triggered per minute. This chart is created by using the following query statement:

        ``` {#codeblock_p96_4za_jza}
        project_name : etl-test and job_name : ceff019ca3d077f85acaad35bb6b9bba65da6717 | select from_unixtime(__time__ - time % 60) as t, count(1) as invoke_count group by from_unixtime(__time__ - time % 60) order by t asc limit 1000
        							
        ```

    -   The chart in the upper-right corner shows the percentages of ETL tasks that are successful and fail. This chart is created by using the following query statement:

        ``` {#codeblock_t7s_60n_7u2}
        project_name : etl-test and job_name : ceff019ca3d077f85acaad35bb6b9bba65da6717 | select task_status, count(1) group by task_status
        							
        ```

    -   The chart in the lower-left corner shows the number of bytes of logs read from the source Logstore every 5 minutes. This chart is created by using the following query statement:

        ``` {#codeblock_tg6_53j_uz1}
        project_name : etl-test and job_name : ceff019ca3d077f85acaad35bb6b9bba65da6717 and task_status : Success | select from_unixtime(__time__ - time % 300) as t, sum(ingest_bytes) as ingest_bytes group by from_unixtime(__time__ - time % 300) order by t asc limit 1000
        							
        ```

    -   The chart in the lower-right corner shows the number of lines of logs read from the source Logstore every 5 minutes. This chart is created by using the following query statement:

        ``` {#codeblock_9bi_b0g_6e8}
        project_name : etl-test and job_name : ceff019ca3d077f85acaad35bb6b9bba65da6717 and task_status : Success | select from_unixtime(__time__ - time % 300) as t, sum(ingest_lines) as ingest_lines group by from_unixtime(__time__ - time % 300) order by t asc limit 1000
        							
        ```


## Step 3: Model data after data processing {#section_nxy_wjv_cs7 .section}

Nginx access logs on the machine are collected by the Logtail client to the source Logstore in real time, processed by the ETL function in a quasi-real-time manner, and then written to the destination Logstore. The following figure shows the data that is processed by the ETL function and contains the IP address information.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13202/156896157632412_en-US.png)

Compared with the data before processing, the data after processing adds four fields: country, province, city, and isp. Based on the four fields, you can know that the request whose IP address is 117.136.90.160 comes from Taiyuan, Shanxi, China, and the carrier is China Mobile.

Then, you can use the log analysis feature of Log Service to query the cities from which requests of specific IP addresses come and the distribution of Internet service providers \(ISPs\) in a period. Use the following two query statements to create reports:

-   `* | select city, count(1) as c group by city order by c desc limit 15`
-   `* | select isp, count(1) as c group by isp order by c desc limit 15`

