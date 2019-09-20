# Analyze vehicle track logs {#concept_59004_zh .concept}

Taxi companies record the details of each trip, including the time when a passenger gets in and out, latitude and longitude, distance of the trip, payment method, payment amount, and tax amount. Detailed data greatly facilitates the operation of taxi companies. For example, with the data, the companies can shorten the running intervals in peak hours or dispatch more vehicles to the areas where more people need taxis. With the help of the data, passengers can get a timely response and drivers can have higher incomes. This improves the efficiency of the whole society.

Taxi companies store the trip logs to Alibaba Cloud Log Service, and pick out useful information with the help of reliable storage and rapid statistical calculations. This topic describes how taxi companies mine useful information from the data stored in Alibaba Cloud Log Service.

Sample data:

``` {#codeblock_nba_8kk_2uh}
RatecodeID:  1VendorID:  2__source__:  11.164.232.105    __topic__:  dropoff_latitude:  40.743995666503906    dropoff_longitude:  -73.983505249023437extra:  0    fare_amount:  9    improvement_surcharge:  0.3    mta_tax:  0.5    passenger_count:  2    payment_type:  1    pickup_latitude:  40.761466979980469    pickup_longitude:  -73.96246337890625    store_and_fwd_flag:  N    tip_amount:  1.96    tolls_amount:  0    total_amount:  11.76    tpep_dropoff_datetime:  2016-02-14 11:03:13    tpep_dropoff_time:  1455418993    tpep_pickup_datetime:  2016-02-14 10:53:57    tpep_pickup_time:  1455418437    trip_distance:  2.02
		
```

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13208/156894042632455_en-US.png)

## Common statistics {#section_ht1_67r_i6z .section}

Before query and analysis, enable and set indexes. For more information, see [Enable and set indexes](../../../../reseller.en-US/Index and query/Enable and set indexes.md#).

1.  Run the following statement to count the number of passengers boarding the taxis during the day and determine the peak hours:

    ``` {#codeblock_kwg_gkx_mrp}
     *| select count(1) as deals, sum(passenger_count) as passengers,    
     (tpep_pickup_time %(24*3600)/3600+8)%24 as time        
     group by (tpep_pickup_time %(24*3600)/3600+8)%24 order by time limit 24
    					
    ```

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13208/156894042632456_en-US.png)

    ``` {#codeblock_4f8_xir_bwq}
    As shown from the preceding figure, the peak hours are generally the morning hours when people go to work and the evening hours when people get off work. With this data, taxi companies can dispatch more vehicles accordingly.
    					
    ```

2.  Run the following statement to collect statistics about the average trip distance in different time periods:

    ``` {#codeblock_370_jbl_7vv}
    *| select  avg(trip_distance)  as trip_distance, 
    (tpep_pickup_time %(24*3600)/3600+8)%24 as time         
    group by (tpep_pickup_time %(24*3600)/3600+8)%24 order by time limit 24
    					
    ```

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13208/156894042632457_en-US.png)

    Passengers tend to take a longer trip during certain time periods of the day, so taxi companies need to dispatch more vehicles.

3.  Run the following statement to calculate the average trip duration \(in minutes\) and the time required for per unit of mileage \(in seconds\), and determine during which time period of the day taxis experience more traffic:

    ``` {#codeblock_k5v_d79_jjd}
    *| select  avg(tpep_dropoff_time-tpep_pickup_time)/60  as driving_minutes, 
    (tpep_pickup_time %(24*3600)/3600+8)%24 as time  
    group by (tpep_pickup_time %(24*3600)/3600+8)%24 order by time limit 24
    					
    ```

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13208/156894042632458_en-US.png)

    ``` {#codeblock_zfv_vo7_heb}
    *| select  sum(tpep_dropoff_time-tpep_pickup_time)/sum(trip_distance)  as driving_minutes, 
    (tpep_pickup_time %(24*3600)/3600+8)%24 as time        
    group by (tpep_pickup_time %(24*3600)/3600+8)%24 order by time limit 24
    					
    ```

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13208/156894042632459_en-US.png)

    More vehicles must be dispatched during peak hours.

4.  Run the following statement to calculate average taxi fares during different time periods and determine the hours with more incomes:

    ``` {#codeblock_9fu_zwh_6zs}
    *| select  avg(total_amount)  as dollars, 
    (tpep_pickup_time %(24*3600)/3600+8)%24 as time 
    group by (tpep_pickup_time %(24*3600)/3600+8)%24 order by time limit 24
    					
    ```

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13208/156894042732460_en-US.png)

    The average taxi fares per customer are higher around 04:00, so financially stressed drivers can consider providing services during this time period.

5.  Run the following statement to view the distribution of the payment amount:

    ``` {#codeblock_gnw_hvk_66u}
    *| select case when total_amount < 1 then 'bill_0_1'  
    when total_amount < 10 then 'bill_1_10' 
    when total_amount < 20 then 'bill_10_20' 
    when total_amount < 30 then 'bill_20_30' 
    when total_amount < 40 then 'bill_30_40' 
    when total_amount < 50 then 'bill_10_50' 
    when total_amount < 100 then 'bill_50_100' 
    when total_amount < 1000 then 'bill_100_1000' 
    else 'bill_1000_'  end 
    as bill_level , count(1) as count group by 
    case when total_amount < 1 then 'bill_0_1'
    when total_amount < 10 then 'bill_1_10' 
    when total_amount < 20 then 'bill_10_20' 
    when total_amount < 30 then 'bill_20_30' 
    when total_amount < 40 then 'bill_30_40' 
    when total_amount < 50 then 'bill_10_50' 
    when total_amount < 100 then 'bill_50_100' 
    when total_amount < 1000 then 'bill_100_1000' 
    else 'bill_1000_'  end 
    order by count desc 
    
    					
    ```

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13208/156894042732461_en-US.png)

    As shown in the preceding figure, the payment amount of most transactions ranges from USD 1 to USD 20.


