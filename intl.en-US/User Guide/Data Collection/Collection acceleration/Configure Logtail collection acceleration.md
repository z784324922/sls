# Configure Logtail collection acceleration {#concept_opz_gml_dgb .concept}

After global acceleration is enabled, the Logtail that is installed in global acceleration mode automatically collects logs in global acceleration mode. For the Logtail that is installed before global acceleration is enabled, you need to manually switch the acceleration mode to global acceleration by performing the steps in this topic.

## Prerequisites {#section_mw1_4ml_dgb .section}

1.  [Enable HTTP acceleration](intl.en-US/User Guide/Data Collection/Collection acceleration/Enable Global Acceleration.md#section_sst_dsz_q2b).
2.  \(Optional\) [Enable HTTP acceleration](intl.en-US/User Guide/Data Collection/Collection acceleration/Enable Global Acceleration.md#section_sst_dsz_q2b).

    If you use HTTPS to access Log Service, make sure that HTTPS acceleration has been enabled and that you have configured HTTPS acceleration by following the instructions provided in [Enable HTTP acceleration](intl.en-US/User Guide/Data Collection/Collection acceleration/Enable Global Acceleration.md#section_sst_dsz_q2b).

3.  Make sure that acceleration functions normally

    by following the instructions provided in [Enable Global Acceleration](intl.en-US/User Guide/Data Collection/Collection acceleration/Enable Global Acceleration.md#ul_acl_jx1_r2b).


## Before you begin {#section_ncb_xml_dgb .section}

Before you configure Logtail collection acceleration, note that:

-   If the Logtail is installed after global acceleration enabling, you must set the installation mode to **global acceleration** by following the instructions provided in [Linux ](intl.en-US/User Guide/Logtail collection/Install/Linux.md). Then, the Logtail collects logs using global acceleration mode methods.
-   If the Logtail is installed before global acceleration is enabled, you must switch the Logtail collection mode to global acceleration by performing the steps in this topic.

## Switch the Logtail collection mode to global acceleration. {#section_u53_qml_dgb .section}

1.  Stop the Logtail.
    -   In Linux, run `/etc/init.d/ilogtaild stop` as the admin user.
    -   In Windows:
        1.  In Control Panel, choose **System and Security** \> **Administrative Tools**.
        2.  Open the **Services** program and locate the LogtailWorker file.
        3.  Right-click the file and click **Stop** in the shortcut menu.
2.  Modify the Logtail startup configuration file ilogtail\_config.json.

    Change the endpoint in data\_server\_list to `log-global.aliyuncs.com` by following the instructions provided in [Startup configuration file \(ilogtail\_config.json\)](intl.en-US/User Guide/Logtail collection/Overview/Logtail configuration and recording files.md#section_jh3_dpk_2fb).

3.  Start the Logtail.
    -   In Linux, run `/etc/init.d/ilogtaild start` as the admin user.
    -   In Windows:
        1.  In Control Panel, choose **System and Security** \> **Administrative Tools**.
        2.  Open the **Services** program and locate the LogtailWorker file.
        3.  Right-click the file and click **Start** in the shortcut menu.

