# Progress bar {#concept_ptq_2j5_jhb .concept}

The progress bar shows the percentage. You can configure the properties of the progress bar to adjust its style and set display rules.

## Components {#section_ngj_fk5_jhb .section}

-   Actual value
-   \(Optional\) Unit
-   Total value

## Procedure {#section_yf3_rn5_jhb .section}

1.  On the search page of a Logstore, enter a query and analysis statement in the search box, set the time range, and then click **Search & Analysis**.
2.  Click ![procedure](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/156555/156574423744262_en-US.png) to select the progress bar.
3.  Configure the properties of the progress bar.

## Properties {#section_trh_i25_93b .section}

|Configuration item|Description|
|------------------|-----------|
|Actual Value|The actual value in the chart. The data in the first row of the specified column is displayed by default.|
|Unit|The unit of the value in the progress bar.|
|Total Value|The total value indicated by the progress bar. Default value: 100.|
|Maximum Value Column|The maximum value in the specified column. When **Use Query Results** is enabled, **Total Value** is replaced by **Maximum Value Column**. You can select the maximum value from query results for this configuration item.|
|Use Query Results|Specifies whether to select a value from query results. When Use Query Results is enabled, you can select the maximum value from query results for Maximum Value Column.|
|Edge Shape|The edge shape of the progress bar.|
|Vertical Display|Specifies whether to display the progress bar in vertical mode.|
|Font Size|The font size of the progress bar.|
|Thickness|The thickness of the progress bar.|
|Background Color|The background color of the progress bar.|
|Font Color|The font color of the progress bar.|
|Default Color|The default color of the progress bar.|
|Color Display Mode|The color display mode of the progress bar.|
|Start Color|The start color of the progress bar. This configuration item is available when **Color Display Mode** is set to **Gradient**.|
|End Color|The end color of the progress bar. This configuration item is available when **Color Display Mode** is set to **Gradient**.|
|Display Color|The display color of the progress bar. This configuration item is available when **Color Display Mode** is set to **Display by Rule**. **Note:** The value of **Actual Value** is compared with that of **Threshold** based on the condition specified by **Operator**. If the actual value matches the condition specified by **Operator**, the progress bar is displayed in the color specified by Display Color. Otherwise, the progress bar is displayed in the default color.

 |
|Operator|The condition for determining the display color of the progress bar. This configuration item is available when **Color Display Mode** is set to **Display by Rule**.|
|Threshold|The threshold for determining the display color of the progress bar. This configuration item is available when **Color Display Mode** is set to **Display by Rule**.|

## Scenarios {#section_rpk_3p5_jhb .section}

You can use the progress bar to display the percentage of a metric or the proportion of data.

``` {#codeblock_2am_0gr_n9r}
* | select diff[1],diff[2] from (select compare( pv , 86400) as diff from (select count(1) as pv from log))
```

When you select **Display by Rule** as the color display mode, the progress bar changes its color according to the specified condition. If the specified condition is not met, the progress bar is displayed in the default color.

