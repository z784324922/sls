# Single value graph {#concept_sqg_gnq_zdb .concept}

Single value graph is displayed in a rectangular box diagram by default. Rectangular box diagram is the simplest and most direct data representation, which visually and clearly displays the data at a certain point. It is generally used to represent the key information at a certain point in time. To display the scale type indicator, you can use a dial.

## Basic components {#section_yrj_tvv_tdb .section}

-   Main text 
-   Unit \(optional\)
-   Description \(optional\)
-   Category

## Configuration item {#section_sh5_5vv_tdb .section}

-   Rectangular box configuration instructions:

    |Rectangular box configuration|Description|
    |:----------------------------|:----------|
    |**Value column**|By default, the first line of data in this column is displayed.|
    |**Color**|The color in the diagram, including:    -   Font color
    -   Background color
|
    |**Text**|The attribute configurations related to the text, including:    -   Font size \(12 px–100 px\)
    -   Unit
    -   Unit font size \(12 px–100 px\)
    -   Description
    -   Description font size \(12 px–100 px\)
|
    |**Chart type**|Rectangular box|

-   Dial configuration instructions:

    |Dial configuration category|Configuration item |Description|
    |:--------------------------|:------------------|:----------|
    |**Value column **|Actual value|By default, the first line of data in this column is displayed.|
    |Unit|The unit of values in the dial.|
    |Dial max|The maximum value displayed on the dial. The default is 100.|
    |Number of color areas|The dial is divided into several numerical areas. Each area is displayed in a different color.The maximum number of color areas is 5. The default is 3.

|
    |Maximum value of each area|The maximum value of each area on the dial. By default, the maximum value of the last area is the maximum value on the dial and you do not need to specify this value.**Note:** By default, the dial contains three color areas which divide the dashboard equally. Changing **number of color area** does not change the value range of each default color area. Therefore, set the maximum value for each color area based on your needs.

|
    |Display title| You can add a single value graph of the dial type in the dashboard. By using **Display title**, you can display or hide the title of a single value graph in the dial form on the dashboard page. The item is disabled by default, that is, the dial title is not displayed.

 Clicking the enable button does not display the title on the current page. The title is displayed on the dashboard page after you create a report or modify the current report.

 |
    |**Color**|Area color|By default, three color areas exist and the colors are blue, yellow, and red.If you change the **Number of color areas** to a value greater than 3, the added areas are blue by default. You can change the color of each area.

|
    |Font color|The color of the numerical values displayed in the dial.|
    |**Text**|Font size|The font size of the text, in the range of 12 px–100 px.|
    |Description|The description of the numeric value.|
    |Font size of description content|The font size of description content, in the range of 12 px–100 px.|
    |**Chart type**|Dial|Displays the query results in a dial.|


## Procedure { .section}

1.  On the query page, enter the query statement in the search box, select the time interval, and then click **Search**.
2.  Select the number chart ![](https://cdn.yuque.com/lark/2018/png/60648/1523256493643-9ccad5de-5224-47d5-8d47-13443a97af15.png).
3.  Configure the graph properties.

    To display the values of the proportional class, select a dial, and configure the dial.

    **Note:** The Log Service numeric graph automatically perform normalization based on the numerical size. For example, `230000` is processed as `230K`. To define your own numeric format, please define the format in real-time analysis through [Mathematical calculation functions](intl.en-US/User Guide/Real-time analysis/Analysis grammar/Mathematical calculation functions.md).


## Example {#section_yyy_lwv_tdb .section}

Execute the following query analysis statement to view the number of visits.

```
* | select count(1) as c
```

Show analysis results in charts:

-   Rectangular box

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13149/15371506195729_en-US.png)

-   Dial

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13149/15371506197726_en-US.png)

     


