# Query OSS access logs {#concept_cw3_ghb_ygb .concept}

Log Service can query and analyze collected Object Storage Service \(OSS\) access logs in real time, and clearly display analysis results by using multiple visual charts.

## Configure query and analysis {#section_t3j_55s_zdb .section}

1.  After you have created the **association**, click **Configure Index** in the displayed dialog box to go to the Log Service console.

    ![Configure query and analysis](images/39791_en-US.png "Configure query and analysis")

2.  Log Service has indexes preconfigured for querying OSS logs. For more information about field description, see [Log fields](reseller.en-US/Data Collection/Cloud product collection/OSS access logs/Log fields.md). Confirm the configurations and click **Next**.

    ![Configure the indexes](images/39792_en-US.png "Configure the indexes")

    **Note:** By default, Log Service creates four specific dashboards for the Logstore associated with one or more buckets. After you complete the configurations, you can view these dashboards on the Dashboard page. You can also click **Analyze Log** next to a target Logstore on the Log Service page in the OSS console, and click the dashboard name in the left-side navigation pane to view the dashboard.

    ![Analyze Logs](images/39795_en-US.png "Analyze Logs")

3.  Configure the Log Shipper and extract, transform, and load \(ETL\) as needed, or directly click **Confirm**.

## Default dashboards {#section_kd4_x5s_zdb .section}

Log Service provides the following default dashboards:

-   **oss\_operation\_center: displays overall operation status.** 

    ![Analyze Logs](images/39796_en-US.png "c")

-   **oss\_access\_center: displays statistics of access logs.** 

    ![oss_access_center](images/39797_en-US.png "oss_access_center")

-   **oss\_performance\_center: displays statistics of performance.** 

    ![oss_performance_center](images/39798_en-US.png "oss_performance_center")

-   **oss\_audit\_center: displays statistics of file deletion and modification.** 

    ![oss_audit_center](images/39799_en-US.png "oss_audit_center")


For more information, see [New Feature: Real-time Analysis of OSS access logs](https://yq.aliyun.com/articles/435682?spm=a2c4g.11186623.2.7.jntiYI).

