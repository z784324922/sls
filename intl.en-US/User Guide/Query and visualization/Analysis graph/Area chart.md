# Area chart {#concept_ndk_hnq_zdb .concept}

The area chart is based on the line chart and has the section between the line and the coordinate axis in the line chart filled with color. The filled section is the area and the color highlights the trend better.  The same as the line chart, the area chart emphasizes the number changes over time, and is used to highlight the trend of the total number.  Both the line chart and the area chart are mostly used to indicate the trend and relationship, instead of the specific values.

## Basic components {#section_uj4_gw1_5db .section}

-   X axis \(horizontal axis\)
-   Y axis \(vertical axis\)
-   Area block

## Configuration items {#section_iff_hw1_5db .section}

|Configuration item|Description|
|:-----------------|:----------|
|X axis|Generally, the X axis is an ordered data type \(time series\).|
|Y axis|You can configure one or more columns of data to correspond to the value interval of the Y axis.|
|Legend|The location where the legend is in the graph. You can configure the legend to the top, bottom, left, or right of the graph.|
|Padding|The distance between the coordinate axis and the graph boundary.|

## Procedure { .section}

1.  On the query page, enter the query statement in the search box, select the time interval, and then click **Search**.
2.  Select the area chart \(![](https://cdn.yuque.com/lark/2018/png/60648/1523258345818-2f28fbf4-c1cc-499b-9977-0a1d6e3b35bf.png) \).
3.  Configure the graph properties.

    **Note:** The number of data records for a single area block in the area chart must be greater than two in case that the data trend cannot be analyzed. We also recommend that you have no more than five area blocks in an area chart.


## Example {#section_phn_4w1_5db .section}

## Simple area chart {#section_pj3_pw1_5db .section}

The PV of IP `42.0.192.0` within the last day:

```
remote_addr: 42.0.192.0 | select date_format(date_trunc('hour', __time__), '%m-%d %H:%i') as time, count(1) as PV group by time order by time limit 1000
```

Select `time` as the X Axis and `PV` as the Y Axis.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13151/5735_en-US.png "Simple area chart")

## Stacked area chart {#section_ylc_ww1_5db .section}

```
* | select date_format(date_trunc('hour', __time__), '%m-%d %H:%i') as time, count(1) as PV, approx_distinct(remote_addr) as UV group by time order by time limit 1000
```

Select `time` as the X Axis. Select `PV` and `UV` as the Y Axis.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13151/5736_en-US.png "Stacked area chart")

