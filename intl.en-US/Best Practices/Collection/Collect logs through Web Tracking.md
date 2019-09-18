# Collect logs through Web Tracking {#concept_67246_zh .concept}

When sending an important email, you will set the read receipt tag in the email to make sure that the recipient has read the email. You will receive a reminder when the recipient reads the email.

The read receipt mode is widely used in many scenarios, such as:

-   Checking whether the recipient has read the sent leaflet.
-   Checking how many users have clicked a promoted webpage.
-   Analyzing user access conditions on a mobile application marketing activity page.

Traditional website- and webmaster-based solutions cannot be used for custom collection and statistics, as they face the following difficulties:

-   Hard to meet the individual needs: User behavior data is not generated at the mobile client. The user behavior data includes some parameters out of operations-based custom needs, such as the source, channel, environment, and behavior parameters.
-   High development difficulty and cost: To meet the need of data collection and analysis, you must first purchase the cloud host, public network IP address, development data receiving server, and message-oriented middleware. In addition, you must adopt mutual backup to guarantee the high availability of the services. Then, develop the server and perform tests.
-   Hard to use: After data is transmitted to the server, engineers must cleanse the results and import them to the database to generate data for operations.
-   Inelastic: The usage of users cannot be predicated. Therefore, a large resource pool must be reserved.

From the preceding aspects, when a content-oriented operations need is raised, a big challenge is how to quickly meet the collection and analysis needs of such user behavior data.

[Log Service](https://www.alibabacloud.com/zh/product/log-service?spm) provides Web Tracking, JS, or Tracking Pixel SDK for the preceding lightweight tracking collection scenarios. With this feature, tracking and data reporting can be completed within 1 minute. In addition, Log Service provides 500 MB [FreeTier quota](https://www.aliyun.com/price/product?spm=5176.55536.857803.practice.12e37c02LUJddO#/sls/detail) per month for each account, allowing you to process business without any cost.

## Features {#section_xln_f4b_wfb .section}

The collection and analysis solution is based on Alibaba Cloud Log Service, a one-stop service for log data. Log Service allows you to quickly complete the collection, consumption, shipping, query, and analysis of large amounts of log data without the need for development. This improves both the O&M efficiency and the operational efficiency. Log Service has the following features:

-   LogHub for real-time collection and consumption. It is interconnected with Blink, Flink, Spark Streaming, Storm, and Kepler.
-   LogShipper for data shipping. It is interconnected with MaxCompute, E-MapReduce, Object Storage Service \(OSS\), and Function Compute.
-   LogSearch and Analytics for query and real-time analysis. It is interconnected with DataV, Grafana, Zipkin, and Tableau.

![Features](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13216/156877358932482_en-US.png)

## Advantages on the collection side {#section_b51_sen_5le .section}

Log Service provides more than 30 data collection approaches and complete solutions for servers, mobile clients, embedded devices, and various development languages. For example:

-   Logtail: the log collection agent for x86 servers.
-   Android or iOS SDK: oriented to mobile clients.
-   C Producer Library: oriented to devices with limited CPU or memory, and smart devices.

![Advantages on the collection](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13216/156877359332483_en-US.png)

The lightweight collection solution Web Tracking in this topic only uses an HTTP GET request to transmit data to a Log Service Logstore. It is applicable to scenarios where no verification is required, such as static websites, advertising, document promotion, and mobile data collection. The following figure shows the characteristics of Web Tracking, when compared with other log collection solutions.

![Web Tracking](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13216/156877359532484_en-US.png)

## Web Tracking access process {#section_zqg_inu_dr1 .section}

The term Web Tracking \(also named Tracking Pixel\) is from the image tag in the HTML syntax. A 0-pixel image can be embedded on a page, which is invisible to users by default. When you access the page and the image is loaded, a GET request is initiated, transmitting parameters to the server.

To use Web Tracking, follow these steps:

1.  Enable the Web Tracking feature for the Logstore.

    By default, the Logstore does not allow anonymous writing. Before using the Logstore, you must enable the Web Tracking feature for the Logstore.

2.  Write data to the Logstore by tracking.

    You can write data in any of the following ways:

    -   Use an HTTP Get request to write data.

        ``` {#codeblock_69f_dd5_3cy}
        curl --request GET 'http://${project}.${sls-host}/logstores/${logstore}/track? APIVersion=0.6.0&key1=val1&key2=val2'
        ```

    -   Embed an HTML image tag. Data is automatically written to the Logstore when a user visits the page.

        ``` {#codeblock_7lw_uax_aqd}
        <img src='http://${project}.${sls-host}/logstores/${logstore}/track.gif? APIVersion=0.6.0&key1=val1&key2=val2'/>
        or
        <img src='http://${project}.${sls-host}/logstores/${logstore}/track_ua.gif? APIVersion=0.6.0&key1=val1&key2=val2’/> 
        track_ua.gif
        ```

        In addition to uploading custom parameters, the server also uses UserAgent and referer in the HTTP header as fields in logs.

    -   Use the SDK for JavaScript to write data.

        ``` {#codeblock_dq6_rpg_64h}
        <script type="text/javascript" src="loghub-tracking.js" async></script>
        
        var logger = new window.Tracker('${sls-host}','${project}','${logstore}');
        logger.push('customer', 'zhangsan');
        logger.push('product', 'iphone 6s');
        logger.push('price', 5500);
        logger.logger();
        							
        ```


For more information, see [Web Tracking ](../../../../reseller.en-US/Data Collection/Other collection methods/Web Tracking.md).

## Case about multi-channel content promotion {#section_ctd_q1k_csx .section}

## Scenarios {#section_6l3_5p5_5q7 .section}

Operational staff cannot wait to send new contents \(such as new functions, activities, games, and articles\) to users because this is the first and most important step to gain users.

Taking game release as an example. A great expense is put in game promotion, for example, 10,000 advertisements are invested. About 2,000 people load the advertisements, which accounts for 20% of the total number of advertisements. About 800 people click the advertisement, and fewer people finally download the game, register accounts, and try the game.

![Scenarios](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13216/156877359532485_en-US.png)

Therefore, obtaining the content promotion effectiveness accurately and in real time is important for the business. To reach the overall promotion goal, operational staff usually use various channels for promotion, for example:

-   In-site messages, official website blogs, and homepage banners.
-   SMS messages, emails, and leaflets.
-   New media, such as Sina Weibo, DingTalk group, WeChat public account, Zhihu forum, and TouTiao.

![Scenarios2](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13216/156877359632486_en-US.png)

## Procedure {#section_72e_zgy_cs9 .section}

## Step 1 Enable the Web Tracking feature {#section_wpn_ghn_p66 .section}

Create a Logstore \(for example, myclick\) in Log Service and enable the Web Tracking feature.

## Step 2 Generate a Web Tracking tag {#section_ag8_10o_yz7 .section}

1.  Add an identity for the article \(article=1001\) to be promoted in each promotion channel, and generate a Web Tracking tag as follows \(taking the img tag as an example\):

    -   In-site message channel \(mailDec\)

        ``` {#codeblock_67o_1gx_73v}
        <img src="http://example.cn-hangzhou.log.aliyuncs.com/logstores/myclick/track_ua.gif?APIVersion=0.6.0&from=mailDec&article=1001" alt="" title="">
        ```

    -   Official website channel \(aliyunDoc\)

        ``` {#codeblock_q85_b6o_67a}
        <img src="http://example.cn-hangzhou.log.aliyuncs.com/logstores/myclick/track_ua.gif?APIVersion=0.6.0&from=aliyundoc&article=1001" alt="" title="">
        ```

    -   Email channel \(email\)

        ``` {#codeblock_o1z_4zp_71l}
        <img src="http://example.cn-hangzhou.log.aliyuncs.com/logstores/myclick/track_ua.gif?APIVersion=0.6.0&from=email&article=1001" alt="" title="">
        ```

    You can add other channels after the from parameter or add more parameters to be collected in the URL.

2.  Put the img tag in the promotion content and release it.

## Step 3 Analyze logs {#section_4cg_lej_bxr .section}

After tracking collection, you can use the [LogSearch and Analytics](../../../../reseller.en-US/Index and query/Overview.md) features of Log Service to query and analyze large amounts of log data in real time. In addition to the [built-in dashboard](../../../../reseller.en-US/Index and query/Query and visualization/Dashboard/Create and delete a dashboard.md), Log Service supports the interconnection methods such as [DataV](../../../../reseller.en-US/Index and query/Query and visualization/Other visualization methods/Interconnect with DataV big screen.md), [Grafana](../../../../reseller.en-US/Index and query/Query and visualization/Other visualization methods/Interconnection with Grafana.md), and Tableau to achieve result analysis visualization.

The following figure displays the collected log data. You can enter a keyword in the search box to query logs.

![collected log data](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13216/156877359632487_en-US.png)

You can also enter an SQL statement after the query to perform real-time analysis and visualization in seconds.

![analysis and visualization](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13216/156877359732488_en-US.png)

1.  Design the query statements.

    The real-time analysis statements for user clicking or reading logs are as follows. For more information about the fields and analysis scenarios, see [Analysis syntax](../../../../reseller.en-US/Index and query/Syntax description.md).

    -   Current total traffic and page views

        ``` {#codeblock_1bl_k1h_xrn}
        * | select count(1) as c
        ```

    -   Curve of the page views per hour

        ``` {#codeblock_oeq_i0z_8gg}
        * | select count(1) as c, date_trunc('hour',from_unixtime(__time__)) as time group by time order by time desc limit 100000
        ```

    -   Ratios of page views in each channel

        ``` {#codeblock_byc_9n1_ph8}
        * | select count(1) as c, f group by f desc
        ```

    -   Devices where the page views are from

        ``` {#codeblock_0e3_rjr_0ae}
        * | select count_if(ua like '%Mac%')  as mac, count_if(ua like '%Windows%')  as win, count_if(ua like '%iPhone%')  as ios, count_if(ua like '%Android%')  as android
        ```

    -   Regions where the page views are from

        ``` {#codeblock_iea_k2p_4tt}
        * | select ip_to_province(__source__) as province, count(1) as c group by province order by c desc limit 100
        ```

2.  Configure the real-time data to a dashboard that is refreshed in real time.

    ![dashboard](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13216/156877359832489_en-US.png)


**Note:** An invisible img tag records your access to this topic. You can try to find it in the source code of the page.

