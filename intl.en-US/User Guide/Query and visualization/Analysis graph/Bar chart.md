# Bar chart {#concept_dt3_2nq_zdb .concept}

The bar chart is another form of column chart, that is, the horizontal column chart.  Generally, the bar chart is used to analyze the top scenario and the configuration method is similar to that of the column chart.

## Basic Components {#section_i2h_ztv_tdb .section}

-   X axis \(vertical axis\)
-   Y axis \(horizontal axis\)
-   Rectangular block
-   Legend

The height of the rectangular block in the bar chart is fixed and the width of the rectangular block indicates the numeric value. Use the grouped bar chart to display the data if multiple columns of data are mapped to the Y axis.

## Configuration item {#section_qrv_ztv_tdb .section}

|Description|Description|
|:----------|:----------|
|X axis| Generally, the X axis indicates the data types. |
|Y axis|You can configure one or more columns of data to correspond to the value interval of the Y axis.|
|Legend|The location where the legend is in the graph. You can configure the legend to the top, bottom, left, or right of the graph.|
|Padding|The distance between the coordinate axis and the graph boundary.|

## Procedure { .section}

1.  On the query page, enter the query statement in the search box, select the time interval, and then click **Search**.
2.  Click the Graph tab and select the bar chart ![](https://cdn.yuque.com/lark/2018/png/60648/1523178865970-9aea5bda-87b2-4aa8-b9da-1cbefb252e65.png).
3.  Configure the graph properties.

    **Note:** 

    -   Use the bar chart if the number of data types is no more than 20. We recommend that you use `LIMIT` to control the number of data types in case that the vertical height is so large that the analytical comparison is not intuitive, and use the`ORDER  BY` syntax when analyzing the top scenario.  We also recommend that you have no more than five columns of data to map to the Y axis.
    -   Supports grouped bar chart, but data in all groups of the bar chart must indicate the increase or decrease at the same time.

## Simple bar chart {#section_zq5_l5v_tdb .section}

To analyze the top 10 request\_uri with the largest number of visits, the statement is as follows:

```
* | select request_uri, count(1) as count group by request_uri order by count desc limit 10
```

![](images/5717_en-US.png "Simple bar chart")

