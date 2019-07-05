# Individual value plot {#concept_sqg_gnq_zdb .concept}

The individual value plot highlights a single value. Individual value plots include the following types:

-   **Rectangle Frame**: displays a general value.
-   **Dial**: shows how close the current value is to the configured threshold.
-   **Compare Numb Chart**: shows the SQL query results of parallel and period-on-period comparison functions. For more information about the analysis syntax, see [Interval-valued comparison and periodicity-valued comparison functions](reseller.en-US/User Guide/Index and query/Analysis grammar/Interval-valued comparison and periodicity-valued comparison functions.md).

Rectangle Frame is selected by default. A rectangle frame is the simplest and most direct data representation that visually and clearly displays the data at a certain point. It is generally used to show the key information at a certain time point. To display a proportional metric, you can use a dial.

## Components {#section_yrj_tvv_tdb .section}

-   Main text
-   \(Optional\) Unit
-   \(Optional\) Description
-   Type

## Procedure {#section_qch_zph_kgb .section}

1.  On the search page of a Logstore, enter a query and analysis statement in the search box, set the time range, and then click **Search & Analysis**.
2.  On the Graph tab, click ![](https://cdn.yuque.com/lark/2018/png/60648/1523256493643-9ccad5de-5224-47d5-8d47-13443a97af15.png) to select the individual value plot.
3.  On the right-side **Properties** tab, configure the properties of the individual value plot.

    **Note:** Log Service automatically normalizes data in charts based on the value. For example, `230000` is processed as `230K`. You can use [Mathematical calculation functions](reseller.en-US/User Guide/Index and query/Analysis grammar/Mathematical calculation functions.md) to define your own numeric format in the real-time analysis phase.


## Properties {#section_sh5_5vv_tdb .section}

-   The following table describes the configuration items of a rectangle frame.

    |Configuration item|Description|
    |:-----------------|:----------|
    |**Chart Types**|The type of the chart. Select Rectangle Frame.|
    |**Value Column**|The value displayed in the chart. The data in the first row of the specified column is displayed by default.|
    |**Unit**|The unit of data, displayed after the value.|
    |**Unit Font Size**|The font size of the unit. You can drag the slider to adjust the font size. Valid values: \[10, 100\]. Unit: pixels.|
    |**Description**|The description of the value, displayed under the value.|
    |**Description Font Size**|The font size of the value description. You can drag the slider to adjust the font size. Valid values: \[10, 100\]. Unit: pixels.|
    |Format|The format in which data is displayed.|
    |**Font Size**|The font size of the value. You can drag the slider to adjust the font size. Valid values: \[10, 100\]. Unit: pixels.|
    |**Font Color**|The color of the number and text. You can select a recommended color or customize a color.|
    |**Background Color**|The color of the background. You can select a recommended color or customize a color.|

-   The following table describes the configuration items of a dial.

    |Configuration item|Description|
    |:-----------------|:----------|
    |**Chart Types**|The type of the chart. Select **Dial** to display query results on a dial.|
    |**Actual Value**|The actual value in the chart. The data in the first row of the specified column is displayed by default.|
    |**Unit**|The unit of the value on the dial.|
    |**Font Size**|The font size of the value and unit. Valid values: \[10, 100\]. Unit: pixels.|
    |**Description**|The description of the value, displayed under the value.|
    |**Description Font Size**|The font size of the value description. You can drag the slider to adjust the font size. Valid values: \[10, 100\]. Unit: pixels.|
    |**Dial Maximum**|The maximum value of the scale on the dial. Default value: 100.|
    |Maximum Value Column|The maximum value in the specified column. When Use Query Results is enabled, **Dial Maximum** is replaced by Maximum Value Column. You can select the maximum value from query results for this configuration item.|
    |Use Query Results|Specifies whether to select a value from query results. When Use Query Results is enabled, you can select the maximum value from query results for Maximum Value Column.|
    |Format|The format in which data is displayed.|
    |**Colored Regions**|The number of value regions that the dial is divided into. Each region is displayed in a different color. Valid values: 2, 3, 4, and 5. Default value: 3.

 |
    |**Region Max Value**|The maximum value of the scale in each colored region of the dial. By default, the maximum value in the last region is the maximum value on the dial. You do not need to specify this value. **Note:** A dial is evenly divided into three colored regions by default. If you change the value of **Colored Regions**, the dial is still evenly divided based on the changed value. You can set the maximum value for each colored region based on your needs.

 |
    |**Font Color**|The color of the value on the dial.|
    |**Region**|The colored region that the dial is divided into. A dial is evenly divided into three regions by default, which are displayed in blue, yellow, and red, respectively. If you set **Colored Regions** to a value greater than 3, the added regions are displayed in blue by default. You can change the color of each region.

 |
    |**Show Title**| Specifies whether to display the title of the dial when you add it to a dashboard as an individual value plot. You can enable or disable **Show Title** to show or hide the title of the individual value plot on the dashboard page. Default value: disabled, indicating that the title of the dial is not displayed.

 After you enable Show Title, the title of the dial is not displayed on the current page. You need to create or modify a dashboard and view the title on the dashboard page.

 |

-   The following table describes the configuration items of a comparison chart.

    |Configuration item|Description|
    |:-----------------|:----------|
    |Chart Types|The type of the chart. Select Compare Numb Chart to display query results in a comparison chart.|
    |Show Value|The value displayed in the center of the comparison chart. This value is generally set to the statistical result calculated by the relevant comparison function in the current time range.|
    |Compare Value|The value used to compare with the threshold. This value is generally set to the comparison result between the statistical result calculated by the relevant comparison function in the current time range and that in the previous time range.|
    |Font Size|The font size of the show value. Valid values: \[10, 100\]. Unit: pixels.|
    |**Unit**|The unit of the show value, displayed after the value.|
    |**Unit Font Size**|The font size of the unit for the show value. Valid values: \[10, 100\]. Unit: pixels.|
    |**Compare Unit**|The unit of the compare value, displayed after the value.|
    |**Compare Font Size**|The font size of the compare value and its unit. Valid values: \[10, 100\]. Unit: pixels.|
    |Description|The description of the show value and its growth trends, displayed under the value.|
    |**Description Font Size**|The font size of the value description. Valid values: \[10, 100\]. Unit: pixels.|
    |**Trend Comparison Threshold**|The value used to measure the variation trend of the compare value. For example, the difference between the compare value and the threshold is -1:

    -   If you set **Trend Comparison Threshold** to 0, a down arrow is displayed on the page, indicating a value decrease.
    -   If you set **Trend Comparison Threshold** to -1, the system determines that the value remains unchanged and does not display the trend on the page.
    -   If you set **Trend Comparison Threshold** to -2, an up arrow is displayed on the page, indicating a value increase.
 |
    |Format|The format in which data is displayed.|
    |Font Color|The color of the show value and value description.|
    |**Growth Font Color**|The font color of the compare value that is greater than the threshold.|
    |**Growth Background Color**|The background color displayed when the compare value is greater than the threshold.|
    |**Decrease Font Color**|The font color displayed when the compare value is less than the threshold.|
    |**Decrease Background Color**|The background color displayed when the compare value is less than the threshold.|
    |**Equal Background Color**|The background color displayed when the compare value is equal to the threshold.|


## Example {#section_yyy_lwv_tdb .section}

Run the following query and analysis statements to view the number of visits and display the analysis results in charts:

-   Rectangle frame

    ```
    * | select count(1) as pv
    ```

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13149/15623133855729_en-US.png)

-   Dial

    ```
    * | select count(1) as pv
    ```

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13149/15623133857726_en-US.png)

-   Comparison chart

    View the comparison of today's visits and yesterday's visits:

    ```
    * | select diff[1],diff[2], diff[1]-diff[2] from (select compare( pv , 86400) as diff from (select count(1) as pv from log))
    ```

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13149/15623133869590_en-US.png)


