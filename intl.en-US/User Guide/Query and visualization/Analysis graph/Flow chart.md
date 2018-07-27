# Flow chart {#concept_tmy_hnq_zdb .concept}

The flow chart, also known as ThemeRiver, is a stacked area chart around the central axis.  The banded branches with different colors indicate different types of information. The band width indicates the corresponding numeric value. Besides, the centralized time attribute of the original data maps to the X axis, which forms a three-dimensional relationship.

You can switch a flow chart to a line chart or column chart. Note that the column chart is displayed in the stacked form by default, and the start point of each data type is at the top of the last column.

## Basic components {#section_bjc_qkb_5db .section}

-   X axis \(horizontal axis\)
-   Y axis \(vertical axis\)
-   Band

## Configuration item {#section_ekc_rkb_5db .section}

|Configuration item|Description|
|:-----------------|:----------|
|X axis|Generally, the X axis is an ordered data type \(time series\).|
|Y axis|You can configure one or more columns of data to correspond to the value interval of the Y axis.|
|Aggregate column|The information requires to be aggregated in the third dimension.|
|Legend| The location where the legend is in the graph. You can configure the legend to the top, bottom, left, or right of the graph. |
|Padding|The distance between the coordinate axis and the graph boundary.|
|Chart type|Provides the area chart \(the default one\), line chart, and column chart \(stacked\).|

## Procedure { .section}

1.  On the query page, enter the query statement in the search box, select the time interval, and then click **Search**.
2.  Click the Graph tab and select the flow chart ![](https://cdn.yuque.com/lark/2018/png/60648/1523259620923-eda62d27-7223-4054-9eef-42825f52f2ae.png).
3.  Configure the graph properties.

## Example {#section_icr_zkb_5db .section}

The flow chart is applicable to displaying the three-dimensional relationship \(time-type-value\).

```
* | select date_format(from_unixtime(__time__ - __time__% 60), '%H:%i:%S') as minute, count(1) as c, request_method group by minute, request_method order by minute asc limit 100000
```

Select `minute` as the X Axis, `c` as the Y Axis, and `request_method` as the Aggregate Column.

![](images/5737_en-US.png "Flow chart")

