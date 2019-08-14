# Console embedment parameters {#concept_uwy_yd3_mgb .concept}

You can set relevant parameters to customize the display effects on webpages when the Log Service console is embedded into a self-built website.

Log Service allows you to [embed the console into a self-built website and access the console without logon](intl.en-US/Index and query/Query and visualization/Other visualization methods/Console sharing embedment.md). Then, you can quickly and conveniently query and analyze logs in a visualized manner. In addition, Log Service also provides parameters for you to customize the UI and integrate the console UI with third-party webpages.

## URL encoding {#section_dlr_4l3_mgb .section}

All UI parameters are URL-encoded in the following format:

``` {#codeblock_2zi_dfi_t8r}
https://sls4service.console.aliyun.com/next/project/$\{ProjectName\}/\{logsearch|savedsearch|dashboard\}/$\{LogstoreName\}/?Parameter 1&Parameter 2
```

Parameter types are as follows:

-   [Common parameters](#)
-   [Parameters of the raw log query page](#)
-   [Parameters of the saved search page](#)
-   [Common dashboard parameters](#) and [advanced dashboard parameters](#)

The following example shows the URL of the raw log query page, where the readOnly parameter is a UI customization parameter that specifies whether to hide all editing buttons on the page.

``` {#codeblock_9sk_qk7_u38}
https://sls4service.console.aliyun.com/next/project/projectaaa/logsearch/logstorebbb/?readOnly=true 
			
```

**Note:** 

-   All parameters except for $\{ProjectName\} and $\{LogstoreName\} must be placed after `/?` at the end of a URL.
-   You can set multiple parameters in a URL and separate them with an ampersand \(`&`\).

## Common parameters {#section_egr_ql3_mgb .section}

|Function|Parameter|Type|Required|Description|Example|
|:-------|:--------|:---|:-------|:----------|:------|
|**[Hide sidebars](#)**|hideTopbar|Boolean|No|Specifies whether to hide the top navigation bar.|hideTopbar=true|
|hideSidebar|Boolean|No|Specifies whether to hide the left-side navigation pane.|hideSidebar=true|
|**[Hide the back button in the navigation pane](#)**|hiddenBack|Boolean|No|Specifies whether to hide the back button on the Logstores page.|hiddenBack=true|
|**[Filter the resources listed in the navigation pane](#)**|keyFilter|JSON|No|The filtered resources listed in the navigation pane. Valid values: -   logstore
-   savedsearch
-   dashboard

 **Note:** 

-   You can use a hyphen \(`-`\) to specify fuzzy match.
-   The parameter value must be in JSON format and URI-encoded.

 |`{"logstore":["logstore-xx"],"savedsearch":["savedsearch-xx"],"dashboard":["dashboard-xx"]}`|
|**[Configure a time selector](#)**|queryTimeType|Long|No|The time range of queried data. Valid values:

-   \[1, 26\]: specifies an integer in the interval. Each integer corresponds to a time range. For more information, see the following table.
-   99: specifies a custom time range. In this case, you must set the `startTime` and `endTime` parameters.

 |queryTimeType=1|
|startTime|Timestamp \(date\)|No|The start time of queried data. This parameter is valid only when the `queryTimeType` parameter is set to 99.|startTime=1547776643|
|endTime|Timestamp \(date\)|No|The end time of queried data. This parameter is valid only when the `queryTimeType` parameter is set to 99.|endTime=1547776731|

|queryTimeType|Description|
|:------------|:----------|
|1|1 minute \(relative\)|
|2|5 minutes \(relative\)|
|3|15 minutes \(relative\)|
|4|1 hour \(relative\)|
|5|4 hours \(relative\)|
|6|1 day \(relative\)|
|7|1 week \(relative\)|
|8|30 days \(relative\)|
|9|This month \(relative\)|
|10|Custom \(relative\)|
|11|1 minute \(time frame\)|
|12|15 minutes \(time frame\)|
|13|1 hour \(time frame\)|
|14|4 hours \(time frame\)|
|15|1 day \(time frame\)|
|16|1 week \(time frame\)|
|17|30 days \(time frame\)|
|18|Today \(time frame\)|
|19|Yesterday \(time frame\)|
|20|The day before yesterday \(time frame\)|
|21|This week \(time frame\)|
|22|Last week \(time frame\)|
|23|This month \(time frame\)|
|24|This quarter \(time frame\)|
|25|This year \(time frame\)|
|26|Custom \(time frame\)|
|99|Custom time range. In this case, you must set the startTime and endTime parameters.|

-   **Hide sidebars** 

    -   URL

        ``` {#codeblock_l4p_5zn_imv}
        https://sls4service.console.aliyun.com/next/project/${ProjectName}/logsearch/${LogstoreName}/?hideTopbar=true&hideSidebar=true
        ```

    -   Display effects

        To hide the top navigation bar and left-side navigation pane on the search page, set parameters as follows: hideTopbar=true&hideSidebar=true. The following figure shows the display effects.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/106800/156575386737608_en-US.png)

-   **Hide the back button in the navigation pane** 
    -   URL

        To hide the back button in the upper-left corner of the raw log query page, set the hiddenBack parameter to true in the URL as follows:

        ``` {#codeblock_ypa_ahz_o5p}
        https://sls4service.console.aliyun.com/next/project/${ProjectName}/logsearch/${LogstoreName}/?hiddenBack=true
        ```

    -   Display effects

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/106800/156575386737609_en-US.png)

-   **Filter the resources listed in the navigation pane** 

    -   URL

        Set the keyFilter parameter in JSON format in the URL to filter the resources listed in the left-side navigation pane. For example, you need to display Logstores whose names contain `aegis-` and whose name is `500osslog`, saved search items whose names contain oss, and dashboards whose names contain ddos.

        The JSON-formatted value is `{"logstore":["aegis-","500osslog"],"savedserach":["oss"],"dashboard":["ddos"]}`, where `aegis-` specifies that all Logstores whose names contain `aegis` are queried in fuzzy match mode and `500osslog` specifies that the Logstore whose name is `500osslog` is queried in exact match mode. You can **use a hyphen \(`-`\) to specify fuzzy match**.

        ``` {#codeblock_und_bd6_avh}
        https://sls4service.console.aliyun.com/next/project/${ProjectName}/logsearch/${LogstoreName}/?keyFilter=%7B"logstore":%5B"aegis-","500osslog"%5D,"savedsearch":%5B"oss"%5D,"dashboard":%5B"ddos"%5D%7D 
        ```

    -   Display effects

        ![](images/37610_en-US.png)

-   **Configure a time selector** 
    -   URL

        ``` {#codeblock_x2a_6u4_k93}
        https://sls4service.console.aliyun.com/next/project/${ProjectName}/logsearch/${LogstoreName}/?queryTimeType=2
        ```

    -   Display effects

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/106800/156575386737611_en-US.png)


## Parameters of the raw log query page {#section_ovd_zq3_mgb .section}

|Parameter|Type|Description|Required|
|:--------|:---|:----------|:-------|
|ProjectName|String|The name of the project.|Yes|
|LogstoreName|String|The name of the Logstore.|Yes|
|queryString|String|The string to be queried.|No|
|readOnly|Boolean|Specifies whether to hide the editing and modifying buttons on the Logstores page, such as **Share**, **Index Attributes**, **Save Search**, and **Saved as Alarm**.|No|

Example

-   **Set the queryString parameter** 
    -   URL

        ``` {#codeblock_fzs_g78_jm8}
        https://sls4service.console.aliyun.com/next/project/${ProjectName}/logsearch/${LogstoreName}?queryString=* | select count(1) as pv ,status group by status
        ```

    -   Display effects

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/106800/156575386837618_en-US.png)

-   **Set the readOnly parameter** 
    -   URL

        ``` {#codeblock_qse_kh4_xjb}
        https://sls4service.console.aliyun.com/next/project/${ProjectName}/logsearch/${LogstoreName}?readOnly=true
        ```

    -   Display effects

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/106800/156575386837613_en-US.png)


## Parameters of the saved search page {#section_acb_1r3_mgb .section}

|Parameter|Type|Description|Required|
|:--------|:---|:----------|:-------|
|ProjectName|String|The name of the project.|Yes|
|savedSearchName|String|The name of the saved search.|Yes|

-   URL

    ``` {#codeblock_uwm_o2c_1xf}
    https://sls4service.console.aliyun.com/next/project/${ProjectName}/savedsearch/${savedSearchName} 
    					
    ```

-   Display effects

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/106800/156575386837612_en-US.png)


## Parameters of the dashboard page {#section_trb_1r3_mgb .section}

|Parameter|Type|Description|Required|
|:--------|:---|:----------|:-------|
|ProjectName|String|The name of the project.|Yes|
|dashboardName|String|The name of the dashboard.|Yes|
|filters|String|The filter conditions. The value of this parameter must be manually encoded. The original format is `encodeURIComponent('filters=key1:value1&filters=key2:value2`.

 After being encoded, the format is `"filters%3Dkey1%3Avalue1%26filters%3Dkey2%3Avalue2"`.|No|
|token|JSON string|The variables to be replaced. The value of this parameter must be manually encoded. The original format is `encodeURIComponent('token=[{"key": "projectName","value":"1"}, {"key": "xxx", "value": "yy"}]')`.

 After being encoded, the format is ``"token%3D%5B%7B%22key%22%3A%20%22projectName%22%2C%22value%22%3A%221%22%7D%2C%20%7B%22key%22%3A%20%22xxx%22%2C%20%22value%22%3A%20%22yy%22%7D%5D"``.|No|
|readOnly|Boolean|Specifies whether to hide the editing and setting buttons on the dashboard page, such as **Edit** and **Alerts**.|No|
|hiddenFilter|Boolean|Specifies whether to hide the filter conditions.|No|
|hiddenToken|Boolean|Specifies whether to hide the variables to be replaced.|No|
|autoFresh|String|The interval for refreshing the dashboard page, such as 30 seconds or 5 minutes. The minimum refresh interval is 15 seconds.|No|

-   Set the readOnly parameter in the URL to hide editing buttons on the dashboard page.
    -   URL

        ``` {#codeblock_k3i_lo6_7fx}
        https://sls4service.console.aliyun.com/next/project/${ProjectName}/dashboard/${LogstoreName}/?readOnly=true 
        ```

    -   Display effects

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/106800/156575386837614_en-US.png)

-   Add two filter conditions, namely, key1=value1 and key2=value2, for the dashboard page. Then, the system uses query and analysis statements to filter all charts based on the filter conditions and continues to use these statements.

    **Note:** You need to encode the value of the filters parameter.

    -   URL

        ``` {#codeblock_tk9_uof_am1}
        https://sls4service.console.aliyun.com/next/project/${ProjectName}/dashboard/${dashboardId}?encodeURIComponent('filters=key1:value1&filters=key2:value2') 
        ```

    -   Display effects

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/106800/156575386838975_en-US.png)

-   Add multiple conditions for replacing variables.

    **Note:** You need to encode the value of the token parameter.

    -   URL

        ``` {#codeblock_a46_6mt_x9y}
        https://sls4service.console.aliyun.com/next/project/${ProjectName}/dashboard/${dashboardId}?encodeURIComponent('token=[{"key": "projectName","value":"1"}, {"key": "xxx", "value": "yy"}]') 
        ```

    -   Display effects

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/106800/156575386838976_en-US.png)

-   Set the autoFresh parameter in the URL to refresh the dashboard page every 5 minutes.
    -   URL

        ``` {#codeblock_zv2_q92_8h7}
        https://sls4service.console.aliyun.com/next/project/${ProjectName}/dashboard/${dashboard_name}/?autoFresh=5m
        ```

    -   Display effects

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/106800/156575386947224_en-US.png)


## Advanced parameters of the dashboard page {#section_ng5_sr3_mgb .section}

If you embed an iframe into the dashboard page, the height of the iframe cannot be determined. In this case, two scroll bars may appear at the same time as follows:

-   The scroll bar of the dashboard page outside the iframe.
-   The scroll bar on the dashboard page in the iframe.

To solve this problem, you can use the postMessage method dashboardHeight of Log Service to obtain the height of the dashboard page and use the obtained value as the height of the iframe.

The sample code is as follows:

**Note:** You need to replace parameters such as $\{projectName\} with actual values.

``` {#codeblock_8x9_3z2_mwv}
<! DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>POST message test</title>
</head>
<style>
  * {
    padding: 0;
    margin: 0;
  }

  iframe {
    display: block;
    width: 100%;
  }
</style>
<body>
  <script>
    window.addEventListener('message',function(e){
      console.log(e.data.dashboardHeight)
      document.getElementById('test').style.height = e.data.dashboardHeight + 'px'
    });
  </script>
  <div style="height: 700px;">somethings</div>
  <iframe id="test" src="http://sls4service.console.aliyun.com/next/project/${projectName}/dashboard/${dashboardName}?hideTopbar=true&product=${productCode}">
</body>
</html>
```

