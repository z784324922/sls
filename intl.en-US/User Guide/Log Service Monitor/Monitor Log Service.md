# Monitor Log Service {#concept_gmy_n4q_zdb .concept}

You can view the monitoring data of Log Service in the CloudMonitor console or Log Service console.

-   In the CloudMonitor console, you can view:
    -   Log reading/writing in Logstores
    -   Logs collected by agents \(Logtail\)
-   In the Log Service console, you can view:
    -   Current point of real-time subscription consumption \(Spark Streaming, Storm, and consumer library\)
    -   Log shipping status

This document describes how to view monitoring data in the **Alibaba Cloud CloudMonitor console**. For how to view monitoring data in the Log Service console, see  [View consumer group status](reseller.en-US/User Guide/Real-time subscription and consumption/View consumer group status.md), [Manage LogShipper tasks](reseller.en-US/User Guide/Data shipping/Manage LogShipper tasks.md) and [Set alarms](reseller.en-US/User Guide/Alarm and notification/Set alarms.md).

## Procedure {#section_nq5_15n_12b .section}

**Note:** You must authorize the sub-accounts before using them to configure the cloud monitoring.

1.  Log on to the Log Service console.
2.  On the Project List page, click the project name.
3.  Click the Monitor icon at the right of the Logstore to enter the CloudMonitor console.

    You can log on to the CloudMonitor console directly and then click **Cloud Service ** \> **Log Service**in the left-side navigation pane to enter the monitoring configuration page.

    Monitor the log data in CloudMonitor. For more information, see [Log Service monitoring](https://www.alibabacloud.com/help/doc-detail/28596.htm).

    ![](images/5827_en-US.png "Monitoring item description")


## See {#section_rq5_15n_12b .section}

[Log Service monitoring metrics](reseller.en-US/User Guide/Log Service Monitor/Log Service monitoring metrics.md).

## Set alarm rules {#section_sq5_15n_12b .section}

Click **Create Alarm Rule** in the upper-right corner of the Monitoring Charts page. Configure the related resource, alarm rules, and notification method.  For more information, see [Use CloudMonitor to set alarm rules](reseller.en-US/User Guide/Log Service Monitor/Use CloudMonitor to set alarm rules.md).

