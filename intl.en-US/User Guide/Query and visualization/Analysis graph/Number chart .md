# Number chart  {#concept_sqg_gnq_zdb .concept}

The number chart, as the easiest and most intuitive display type of data, shows the data on a point clearly and intuitively, and is generally used to indicate the key information on a time point.  Log Service number chart automatically normalizes the numeric values. For example, 230000 is processed as 230K. To customize the numeric format, you must do it in the real-time analysis phase \(for more information, see Mathematical calculation functions\).

## Basic components {#section_yrj_tvv_tdb .section}

-   Main text 
-   Unit \(optional\)
-   Description \(optional\)

## Configuration items {#section_sh5_5vv_tdb .section}

|Configuration item |Description |
|:------------------|:-----------|
|Value column |By default, the first line of data in this column is displayed.|
|Color|The color in the number chart, including:-   Font color
-   Background color

|
|Text|The attribute configurations related to the text, including:-   Font size \(12–100 px\)
-   Unit
-   Unit font size \(12–100 px\)
-   Description
-   Description font size \(12–100 px\)

|

## Procedure { .section}

1.  On the query page, enter the query statement in the search box, select the time interval, and then click **Search**. 
2.  Click the Graph tab and select the number chart \(![](https://cdn.yuque.com/lark/2018/png/60648/1523256493643-9ccad5de-5224-47d5-8d47-13443a97af15.png)\). 
3.  Configure the graph properties.

    **Note:** Log Service Number chart automatically normalize the data. For example, `230000` is processed as `230K`, if you want to define your own numeric format, see [Mathematical calculation functions](intl.en-US/User Guide/Real-time analysis/Analysis grammar/Mathematical calculation functions.md).


## Example  {#section_yyy_lwv_tdb .section}

Execute the following query analysis statement to view the number of visits. 

```
* | select
            count(1)
            as
            c
```

![](images/5729_en-US.png "Number chart")

