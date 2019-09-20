# Cleanse data through ETL {#concept_43805_zh .concept}

Data is not flawless, so it requires processing. There is always a gap between raw data and final results. You can run Extract-Transform-Load \(ETL\) jobs to cleanse, transform, and sort data.

## Use case {#section_xw2_3yk_5fb .section}

"I Want Take-away" is an e-commerce website with a platform involving users, restaurants, and couriers. Users can place their take-away orders on the website, app, WeChat, or Alipay. Once receiving an order from a user, a merchant starts preparing the ordered dishes. At the same time, the system automatically notifies the nearest couriers. Then, one of the couriers accepts the order and delivers the dishes to the user.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13201/156896542632397_en-US.png)

The operations team has two tasks:

-   Obtain the locations of couriers so that the couriers can be dispatched to specific areas as required.
-   Obtain the information about the usage of coupons and cashes so that coupons can be given away to specific areas for interactive promotion.

## GPS data processing {#section_rbf_u28_tft .section}

The GPS data \(X, Y\) is reported through the courier app every minute. The data format is as follows:

``` {#codeblock_vpl_bwt_vd2}
2016-06-26 19:00:15 ID:10015 DeviceID:EXX12345678 Network:4G GPS-X:10.30.339 GPS-Y:17.38.224.5 Status:Delivering
			
```

Each record contains the reporting time, courier ID, network, device ID, and coordinate position including GPS-X and GPS-Y. The longitude and latitude information provided by GPS is accurate. However, the operations team only needs to obtain information about the courier status in each area. Therefore, such accurate data is not required. In this case, the coordinate position can be transformed to a readable field such as City, District, or ZipCode.

``` {#codeblock_xxs_l57_clk}
(GPS-X,GPS-Y) --> (GPS-X, GPS-Y, City, District, ZipCode)
			
```

This is a typical ETL requirement. Use LogHub to create two Logstores, namely, the PositionLog Logstore for raw data and the EnhancedLog Logstore for transformed data. Run an ETL program, such as Spark Streaming and Storm, or start Consumer Library in the container to read data from the PositionLog Logstore in real time, transform the coordinate position, and write the transformed data to the EnhancedLog Logstore. You can connect the EnhancedLog Logstore to Realtime Compute to visualize the log data. You can also query log data in the Logstore by creating indexes.

The whole process is as follows:

1.  Use event tracking on the courier app to report the current GPS location to the PositionLog Logstore every minute.
    -   We recommend that you use the LogHub SDK for iOS or Android, or Mobile Analytics \(MAN\) to access the mobile device logs.
2.  Use a program to read data from the PositionLog Logstore in real time. Then, transform the log data and write the transformed data to the EnhancedLog Logstore in real time.
    -   We recommend that you use Spark Streaming, Storm, Consumer Library \(which adopts a programming mode that supports automatic load balancing\), or SDK for subscription.
3.  Process the log data in the EnhancedLog Logstore, for example, computing the data and visualizing the result. We recommend that you use:
    -   LogHub for integrating Realtime Compute.
    -   LogShipper for data shipping to OSS, E-MapReduce, Table Store, and MaxCompute.
    -   LogSearch for querying orders.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13201/156896542632400_en-US.png)

## Order masking and analysis {#section_zpx_onl_u2d .section}

The payment service receives payment requests, including the payment account, method, amount, and coupons.

-   The payment requests include sensitive information. Therefore, order masking is required.
-   The coupon and cash data involved in the payment information needs to be separated.

The whole process is as follows:

1.  Create four Logstores, namely, PayRecords, Cleaned, Cash, and Coupon. The app writes the order data to the PayRecords Logstore through Log4J Appender.

    We recommend that you use Log4J Appender to avoid sensitive data from being saved to hard disks.

2.  The masking program consumes the data in the PayRecords Logstore in real time. After the account information is masked, the data in the PayRecords Logstore is written to the Cleaned Logstore.

The division program consumes the data in the Cleaned Logstore in real time, and stores the coupon and cash data respectively in the Cash Logstore and Coupon Logstore through the business logic.

We recommend that you use Spark Streaming, Storm, Consumer Library, or SDK for real-time data masking and division.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13201/156896542632401_en-US.png)

## Other features {#section_yvg_f4m_5ch .section}

-   You can control each Resource Access Management \(RAM\) user's permissions on each Logstore in LogHub in the RAM console.
-   You can obtain the current read and write data through CloudMonitor and view the consumption progress in the console.

