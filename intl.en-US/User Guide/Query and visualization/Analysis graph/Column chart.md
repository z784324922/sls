# Column chart {#concept_vrr_dnq_zdb .concept}

The column chart displays the numeric comparison among data types by using vertical or horizontal columns. The line chart describes the ordered data, while the column chart describes different types of data and counts the number in each data type.

You can also use multiple rectangular blocks to correspond to one type attribute in the grouping or stacked modes to analyze the differences of data types in different dimensions.

## Basic components {#section_pn5_1tv_tdb .section}

-   X axis \(horizontal axis\)
-   Y axis \(vertical axis\)
-   Rectangular block
-   Legend

The column chart provided by Log Service uses the vertical columns by default, that is, the width of the rectangular block is fixed, and the height of the rectangular block indicates the numeric value. Use the grouped column chart to display the data if multiple columns of data are mapped to the Y axis.

## Configuration items {#section_fdk_btv_tdb .section}

|Configuration items|Description |
|:------------------|:-----------|
|X axis|Generally, the X axis indicates the data types.|
|Y axis|You can configure one or more columns of data to correspond to the value interval of the Y axis.|
|Legend|The location where the legend is in the graph. You can configure the legend to the top, bottom, left, or right of the graph.|
|Padding|The distance between the coordinate axis and the graph boundary.|

## Procedure { .section}

1.  On the query page, enter the query statement in the search box, select the time interval, and then click **Search**.
2.  Click the Graph tab and select the column chart \(column\).![](https://cdn.yuque.com/lark/2018/png/60648/1523174359779-5f8165bb-f26f-40f1-8a13-a153ea3229eb.png) 
3.  Configure the graph properties.

    **Note:** Use the column chart if the number of data types is no more than 20. We recommend that you use `LIMIT` to control the number of data types in case that the horizontal width is so large that the analytical comparison is not intuitive.  We also recommend that you have no more than five columns of data to map to the Y axis.


## Example  {#section_a3d_ktv_tdb .section}

## Simple column chart {#section_sw1_ltv_tdb .section}

To query the number of visits for each `http_referer` in the current time interval, the statement is as follows:

```
* | select http_referer, count(1) as count group by http_referer
```

Select `http_referer` as the X Axis and `count` as the Y Axis.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13146/5713_en-US.png "Simple column chart")

## Grouped column chart {#section_t2q_ltv_tdb .section}

To query the number of visits and the average bytes for each `http_referer` in the current time interval, the statement is as follows:

```
* | select http_referer, count(1) as count, avg(body_bytes_sent) as avg group by http_referer
```

Select `http_referer` as the X Axis, and select `count` and `avg` as the Y Axis.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13146/5714_en-US.png "Grouped column chart")

