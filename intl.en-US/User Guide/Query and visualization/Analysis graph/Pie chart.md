# Pie chart {#concept_yyr_fnq_zdb .concept}

The pie chart is used to indicate the ratios of different data types and compare different data types by using the radian.  A pie is divided into multiple sections according to the ratios of different data types. The entire pie indicates the total amount of data, and each section \(arc\) indicates the ratio of a data type to the total amount of data. The sum of all the section \(arc\) ratios is 100%.

## Basic components {#section_vsh_55v_tdb .section}

-   Sector
-   Text percentage
-   Legend

## Configuration items {#section_uvx_55v_tdb .section}

|Configuration item|Description|
|:-----------------|:----------|
|Type|The data types.|
|Value column |The value corresponding to different types of data.|
|Legend |The location where the legend is in the graph. You can configure the legend to the top, bottom, left, or right of the graph.|
|Padding |The distance between the coordinate axis and the graph boundary.|
|Pie chart type|Provides the pie chart \(the default one\), the cycle graph, and the Nightingale rose diagram.|

## Type {#section_oqz_w5v_tdb .section}

Log Service provides three types of pie charts: the default pie chart, the cycle graph, and the Nightingale rose diagram.

**Cycle graph**

Essentially, the cycle graph is a pie chart without the central part. Compared with the pie chart, the cycle graph has the following advantages:

-   Supports displaying the total amount based on the original components, which provides you with more information.
-   Comparing two pie charts is not intuitive. Two cycle graphs can be compared by using the ring length.

**Nightingale rose diagram**

Essentially, the Nightingale rose diagram is not a cycle graph, but a column chart in the polar coordinate system. The data types are divided by arcs and the radius of the arc indicates the data size. Compared with the pie chart, the Nightingale rose diagram has the following advantages:

-   Use the pie chart if the number of data types is no more than 10, and use the Nightingale rose diagram if the number of data types is 11–30.
-   The area is the square of radius. Therefore, the Nightingale rose diagram enlarges the differences among different types of data, and is especially applicable to comparing similar values.
-   A circle has a period. Therefore, the Nightingale rose diagram can also be used to indicate the time concept in a period, such as the week or the month.

## Procedure { .section}

1.  On the query page, enter the query statement in the search box, select the time interval, and then click **Search**.
2.  Click the Graph tab and select the pie chart![](https://cdn.yuque.com/lark/2018/png/60648/1523181004950-79fc5b0d-22e7-4d35-a61a-bc3cd0b76235.png).
3.  Instructions

**Note:** 

-   Use the pie chart and cycle graph if the number of data types is no more than 10. We recommend that you use LIMIT to control the number of data types in case that the number of sections with different colors is so large that the analysis is not intuitive.
-   We recommend that you use the Nightingale rose diagram or column chart if the number of data types is more than 10.

## Example {#section_yw4_dvv_tdb .section}

## Pie chart {#section_c12_fvv_tdb .section}

Analyze the ratio of the access `status` :

```
* | select status, count(1) as c group by status order by c limit 10
```

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13148/5719_en-US.png "Pie chart")

## Cycle graph {#section_ffc_kvv_tdb .section}

Analyze the ratio of the access `request_method`:

```
* | select request_method, count(1) as c group by request_method order by c limit 10
```

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13148/5721_en-US.png "Cycle graph")

## Nightingale rose diagram {#section_q3d_lvv_tdb .section}

Analyze the ratio of the access `request_uri`:

```
* | select request_uri, count(1) as c group by request_uri order by c
```

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13148/5722_en-US.png "Nightingale rose diagram ")

