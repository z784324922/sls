# Ship data to MaxCompute by using DataWorks {#concept_rjc_m4q_zdb .concept}

You can not only ship logs to OSS storage, but also ship log data to MaxCompute by using the Data Integration function of DataWorks. Data Integration is a stable, efficient, and elastically scalable data synchronization platform provided by the Alibaba Group to external users. It provides offline batch data access channels for Alibaba Clouds big data computing engines \(including MaxCompute, AnalyticDB, and OSPS\).

For details about the regions in which this feature is available, see [DataWorks](https://www.alibabacloud.com/help/doc-detail/94682.htm).

## Scenarios  {#section_c3g_fnf_vdb .section}

-   Data synchronization between data sources \(LogHub and MaxCompute\) across regions
-   Data synchronization between data sources \(LogHub and MaxCompute\) with different Alibaba Cloud accounts
-   Data synchronization between data sources \(LogHub and MaxCompute\) with the same Alibaba Cloud account
-   Data synchronization between data sources \(LogHub and MaxCompute\) with a public cloud account and an AntCloud account

## Prerequisites {#section_cw1_gnf_vdb .section}

1.  Log Service, MaxCompute, and DataWorks have been activated.
2.  Log Service has successfully collected log data and LogHub has data to ship.
3.  An Access Key pair is enabled for the data source account.
4.  RAM authorization is configured when shipping across accounts is involved.

    For details, see [Perform authorization for log shipping across accounts](#section_nkr_5pf_vdb) in this document.


## Procedure {#section_jxb_hnf_vdb .section}

## Step 1 Create a data source {#section_nkh_hnf_vdb .section}

1.  On the DataWorks console, open the Data Integration page. Click the Data Source tab on the navigation bar on the left.
2.  On the Data Source page, click New Data Source in the upper right corner. The New Data Source page appears.
3.  Click **LogHub** in the **Message Queue** list. The New LogHub data source page appears.

    ![](images/5817_en-US.png "Add a data source")

4.  Set the configuration items for the data source.

    The following table describes the configuration items:

    |Configuration items|Description|
    |:------------------|:----------|
    |Data source name|A data source name may consist of letters, digits, and underscores. It must begin with a letter or underscore and cannot contain more than 60 characters.|
    |Data source description|A brief description of the data source, containing up to 80 characters.|
    |LOG Endpoint|Endpoint of Log Service, determined by your region, in the format of http://yyy.com . For more information, see [Service endpoint](../../../../../reseller.en-US/API Reference/Service endpoint.md).|
    |LOG Project|A Log Service project in MaxCompute to which the log data is sent. It must be an existing project. Must be a project that has been created.|
    |Access Id/Access Key|The Access Key of the data source account is equivalent to a logon password. You can enter the Access Key of the primary account or subaccount of the data source. After successful configuration, the current account is granted access to the account logs in the data source and thus can ship logs of the data source account through a synchronization task.|

    ![](images/5818_en-US.png "Create a LogHub data source")

5.  Click **Test Connectivity** Click **Finish** after **Connectivity test is successful** appears in the upper right corner of the page.

## Step 2 Configure a synchronization task {#section_bf5_5nf_vdb .section}

Click **Synchronization Task** in the navigation bar on the left and click Step 2 **Create a synchronization task** to configure the synchronization task.

Select **Wizard Mode** to configure the task on a visualized page more easily; or select **Script Mode** to configure your synchronization task with more customization.

**Wizard mode**

The configuration items of the task synchronization node include Select a Source, Select a Target, Field Mapping, and Channel Control.

1.  Select a source.

    **Data source**: Select the data source configured in Step 1. Set the configuration items according to the following table:

    |Configuration items|Description|
    |:------------------|:----------|
    |Data source|Select the name of the LogHub data source.|
    |Logstore|Name of the table from which the incremental data is exported. You must enable the Stream feature on the table when creating the table or using UpdateTable API later.|
    |Log start time|Start time of data consumption. The parameter defines the left border of a time range \(left closed and right open\) in the format of yyyyMMddHHmmss \(such as 20180111013000\) and can work with the scheduling time parameter in DataWorks.|
    |Log end time|End time of data consumption. The parameter defines the right border of a time range \(left closed and right open\) in the format of yyyyMMddHHmmss \(such as 20180111013010\) and can work with the scheduling time parameter in DataWorks.|
    |Batch size|Number of data entries read each time. The default value is 256.|

    After the configuration items are set, click the **Data Preview** drop-down button to show the **Data Preview** details. Verify that log data has been obtained, and then click **Next**.

    **Note:** Data preview presents several data entries selected from the LogHub. The preview result may differ from the synchronization data that you configure, because the synchronization data is configured with log start time and end time.

    ![](images/5819_en-US.png "Select a source")

2.  Select a target.

    1.  Select a MaxCompute data source and target table.

        If you have not created any MaxCompute table, click **Generate Target Table** in One Click on the right. Choose Create Data Table on the dialog box-up menu.

    2.  Fill in **Partition information**.

        Partition configuration supports regular expressions. For example, you can set the pt value of the partition “\*” to read data in all the pt partitions.

    3.  Choose **Clearing Rules**.

        You can choose to clear exiting data \(overwrite mode\) or retain existing data \(insertion mode\) before writing.

    After the configuration, click **Next**.

    ![](images/5820_en-US.png "Select a target")

3.  Map fields.

    Select the mapping between fields. You need to configure the field mapping relationship. Source Table Fields on the left correspond one-to-one with Target Table Fields on the right. You can click **Same row mapping** to select or deselect **Same row mapping**.

    **Note:** 

    -   If you need to manually add log fields as synchronous columns, use the [Script mode](reseller.en-US/User Guide/Data shipping/Ship data to MaxCompute/Ship data to MaxCompute by using DataWorks.md#section_gnq_k4f_vdb) configuration.
    -   You can enter constants. Each constant must be enclosed in a pair of single quotes, such as abc and 123.
    -   You can add scheduling parameters, such as $\{bdp.system.bizdate\}.
    -   You can enter functions supported by relational databases, such as now\(\) and count\(1\).
    -   If the value you entered cannot be parsed, the type is displayed as Not identified.
    ![](images/5821_en-US.png "Map fields")

4.  Control the tunnel.

    Configure the maximum job rate and dirty data check rules, as shown in the following figure:

    ![](images/5822_en-US.png "Control the tunnel")

    Configuration item descriptions:

    -   **DMU:**  Data migration unit, which measures the resources \(including CPU, memory, and network bandwidth\) consumed during data integration.
    -   **Concurrent job count**: Maximum number of threads used to concurrently read data from or write data into the data storage media in a data synchronization task.
5.  Preview and save the configuration.

    After completing the configuration, you can scroll up or down to view the task configurations. If no error exists, click **Save**.

    ![](images/5823_en-US.png "Preview and save the configuration")


## Script mode {#section_gnq_k4f_vdb .section}

To configure the task using a script, see the following script for reference:

```
{
"type": "job",
"version": "1.0",
"configuration": {
"reader": {
"plugin": "loghub",
"parameter": {
"datasource": "loghub_lzz",//Data source name. Use the data resource name you have added.
"logstore": "logstore-ut2",//Target Logstore name. A Logstore is a log data collection, storage, and query unit in the Log Service.
"beginDateTime": "${startTime}",//Start time of data consumption. The parameter defines the left border of a time range (left closed and right open)
"endDateTime": "${endTime}",//End time of data consumption. The parameter defines the right border of a time range (left closed and right open)
"batchSize": 256,//Number of data entries read each time. The default value is 256. 
"splitPk": "",
"column": [
"key1",
"key2",
"key3"
]
}
},
"writer": {
"plugin": "odps",
"parameter": {
"datasource": "odps_first",//Data source name. Use the data resource name you have added.
"table": "ok",//Target table name
"truncate": true,
"partition": "",//Shard information
"column": [//Target column name
"key1",
"key2",
"key3"
]
}
},
"setting": {
"speed": {
"mbps": 8,/Maximum job rate
"concurrent": 7//Number of concurrent jobs
}
}
}
}
```

## Step 3 Run the task {#section_gg5_4pf_vdb .section}

You can run the task in either of the following ways:

-   Directly run the task \(one-time running\)

    Click **Run** above the task to run the task directly on the data integration page. Set values for the custom parameters before running the task.

     ![](images/5824_en-US.png "Running task configuration") 

    As shown in the preceding figure, LogHub records between 10:10 and 17:30 are synchronized to MaxCompute.

-   Schedule the task

    Click **Submit** to submit the synchronization task to the scheduling system. The scheduling system automatically and periodically runs the task from the second day according to the configuration attributes.

    Set the schedule interval to 5 minutes, at a schedule period from 00:00 to 23:59, with startTime=$\[yyyymmddhh24miss-10/24/60\] 10 minutes before the  system to endTime=$\[yyyymmddhh24miss-5/24/60\] 5 minutes before the system.

    ![](images/5825_en-US.png "Scheduling configuration")


## Perform authorization for log shipping across accounts {#section_nkr_5pf_vdb .section}

To configure a log shipping task across accounts, perform authorization on the RAM.

-   **Perform authorization for log shipping across accounts**

    To ship data between primary accounts, you can enter the Access Key of the primary account of the data source in the step **Add LogHub Data Source**. Authorization is successful if the connectivity test passes.

    For example, to ship log data under account A to a MaxCompute table of account B through the DataWorks service activated with account B, configure a data integration task with account B and enter the Access Key of the primary account of account A in the step **Add LogHub Data Source**. After successful configuration, account B has the permission to read all log data under account A.

-   **Subaccount authorization**

    If you do not want to reveal the Access Key of the primary account or need to ship the log data collected by a subaccount, configure explicit authorization for the subaccount.

    -   **Assign management permissions to the subaccount**

        If you need to ship all log data under a primary account through a subaccount, perform the following steps for authorization and Access Key configuration.

        1.  Use primary account A to assign Log Service management permissions \(`AliyunLogFullAccess` and `AliyunLogReadOnlyAccess`\) to subaccount A1. For details, see [Grant RAM sub-accounts permissions to access Log Service](reseller.en-US/User Guide/Access control RAM/Grant RAM sub-accounts permissions to access Log Service.md).
        2.  Configure a data integration task with account B, and enter the Access Key of the subaccount of the data source in the step **Add LogHub Data Source**.
        After successful configuration, account B has the permission to read all log data under account A.

    -   **Assign the customization permission to the subaccount**

        If you need to ship specified log data under a primary account through a subaccount, perform the following steps for authorization and Access Key configuration.

        1.  Configure a custom authorization policy for subaccount A1 with the primary account A. For details on related authorization operations, see [Authorization - Overview](reseller.en-US/User Guide/Access control RAM/Authorization - Overview.md) and [Overview](../../../../../reseller.en-US/API Reference/RAM__STS/Overview.md).
        2.  Configure a data integration task with account B, and enter the Access Key of the subaccount of the data source in the step **Add LogHub Data Source**.
        When the above steps are successfully completed, account B has the permission to read specified log data under account A.

        **Example of custom authorization policy**:

        In this way, account B can synchronize only project\_name1 and project\_name2 data in Log Service through subaccount A1.

        ```
        {
        "Version": "1",
        "Statement": [
        {
        "Action": [
        "log:Get*",
        "log:List*",
        "log:CreateConsumerGroup",
        "log:UpdateConsumerGroup",
        "log:DeleteConsumerGroup",
        "log:ListConsumerGroup",
        "log:ConsumerGroupUpdateCheckPoint",
        "log:ConsumerGroupHeartBeat",
        "log:GetConsumerGroupCheckPoint"
        ],
        "Resource": [
        "acs:log:*:*:project/project_name1",
        "acs:log:*:*:project/project_name1/*",
        "acs:log:*:*:project/project_name2",
        "acs:log:*:*:project/project_name2/*"
        ],
        "Effect": "Allow"
        }
        ]
        }
        ```

    ![](images/5826_en-US.png "Custom authorization policy")


