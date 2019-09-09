# Consumption history {#concept_221026 .concept}

This topic describes how to check the consumption items and their amount. It helps you understand the consumption details during your use of Log Service.

## View the consumption amount {#section_5s4_u5n_b2q .section}

1.  In the Log Service console, choose **Billing Management** \> **Billing Management** in the top navigation bar.
2.  In the Expenses center, choose **Purchases record** \> **Purchases details** in the left-side navigation pane.
3.  On the **Cloud products** tab, select **Simple Log Service \(SLS\)** from the Product drop-down list, select the query time range, and then click **Query**.
4.  The query results appear in the list. Click **Details** in the Operation column of a record.
5.  Click ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/188496/156802779145740_en-US.png) to view the amount of different consumption items.

## View the usage {#section_qoe_xla_ebm .section}

You can export the usage record to view the details of consumption items.

1.  In the Log Service console, choose **Billing Management** \> **Billing Management** in the top navigation bar.
2.  In the Expenses center, choose **Purchases record** \> **Usage record** in the left-side navigation pane.
3.  On the **Usage record** page, set Product, Service period, and Measurement granularity, enter the verification code, and then click **ExportCSV**.
4.  View the details of the consumption items in the exported CSV file.

    |Field|Description|Example|
    |-----|-----------|-------|
    |Project Space|The name of the project.|my-project|
    |Logging library|The name of the Logstore.|my-logstore|
    |Start Time|The start time of the statistical period.|2019/4/28 0:00|
    |End Time|The end time of the statistical period.|2019/4/28 1:00|
    |InflowSize|The inbound traffic. Unit: bytes.|518828|
    |Extranet outgoing traffic|The read traffic on the Internet. Unit: bytes.|0|
    |IndexSize|The index traffic. Unit: bytes.|1179911|
    |ShardSize|The average number of shards \(excluding read-only shards\) in the statistical period.|10|
    |IndexTTL|The data retention time. Unit: days.|90|
    |Region|The region of the project.|cn-shanghai-staging|
    |WriteCount|The number of write requests.|486|
    |OutflowSize|The outbound traffic. Unit: bytes.|0|
    |ReadCount|The number of read requests.|0|
    |RawStorageSize|The current storage size of raw data. Unit: bytes.|854980173|
    |IndexStorageSize|The current storage size of index data. Unit: bytes. The total storage size is the sum of the values of the RawStorageSize and IndexStorageSize fields.|1589594614|


