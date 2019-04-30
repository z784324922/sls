# concept {#concept_l2c_2s3_y2b .concept}

1.  Log on to the [Log Service console](https://partners-intl.console.aliyun.com/#/sls), and then click the target project name.

|Configuration|Description|
|:------------|:----------|
|**Operation Type**| -   **Add to existing dashboard**: Add the current analysis chart to the existing dashboard.
-   **New dashboard**: Add the current analysis chart to the new dashboard.

 |
|**List of dashboards**|Select the existing dashboard name. **Note:** Only specified when **Type of operation** is set to **Add to existing dashboard**.

 |
|**Dashboard name**|New dashboard name. **Note:** Only specified when **Type of operation** is set to **New dashboard**.

 |
|**Chart name**|Name the current analysis chart. The name is displayed in the dashboard page as a chart title.|
|**Query statement**|Query statement for the current analysis chart.|
|**Token configuration**| Select a partial query statement in and click**Generate token** in**Query statement**, and placeholder variables can be generated. If the drill down event of other charts is to jump to this dashboard, jump to this dashboard when clicking on other charts, and the placeholder variable is replaced with the chart value that triggers the drill down event, and refresh the dashboard with the query statement after replacing the variable. For more information, see [Drill-down analysis](../../../../reseller.en-US/User Guide/Query and visualization/Dashboard/Drill-down analysis.md)。

 -   **Token key**: Name the placeholder variable. If the placeholder variable name is the same as the variable set by the dashboard chart that triggered the drill down event, the placeholder variable in the drill down event is replaced with the chart value that triggered the drill down event.
-   **Default Value**: The default value of the placeholder variable in the current dashboard.
-   **Generate results**: Confirm the variable configuration.

 |

