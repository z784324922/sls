# Manage LogShipper tasks {#task_jcf_skc_ry .task}

LogShipper is a function in Log Service that allows you to maximize your data value. You can ship the collected logs to Object Storage Service \(OSS\) in the console to store data for a long term or consume data together  with other systems such as E-MapReduce. After the LogShipper function is enabled, Log Service backend regularly ships the logs written to the Logstore to the corresponding cloud products. The Log Service console provides the OSS Shipper page for you to query the data shipping status within a specified time range,  which allows you to know the shipping progress and handle online issues in time.

On the Logstore List page, click **OSS** in the left-side navigation pane. The OSS Shipper page appears.  You can manage your LogShipper tasks in the following ways.

## Enable/disable LogShipper tasks {#section_zxp_s1t_1bb .section}

1.  Select the target Logstore on the **OSS Shipper page**.
2.  Click **Enable** or **Disable** to enable or disable the tasks.

    You must reconfigure the shipping rule after you enable the tasks again.


You must reconfigure the shipping rule after you enable the tasks again.

## Configure a shipping rule {#section_qjc_sbt_1bb .section}

After enabling the LogShipper tasks, click **Setting** to modify the shipping rule.

## View details of a LogShipper task {#section_myt_5bt_1bb .section}

You can filter the LogShipper tasks to be viewed based on the Logstore, time range, and task shipping status. Then, you can view the details of a specific LogShipper task on this page, such as the status, start time, end time, time when logs are received, and type.

A LogShipper task has three kinds of status.

|Status|Description|Operation|
|:-----|:----------|:--------|
|Success|Logs are successfully shipped.|No need to pay attention.|
|Running|Logs are being shipped.|Check whether or not logs are successfully shipped later.|
|Failed|Logs failed to be shipped. The LogShipper task has encountered an error because of external reasons and cannot be retried.|For more information, see Manage LogShipper tasks in Ship logs to OSS.|

## Delete shipping configuration {#section_ayl_zqh_tbb .section}

1.   On the Logstore list page, click Delete rule. 
2.  Click Confirm in the dialog box. Once deleted, you will no longer be able to create an offline archive configuration with the same name. Please choose carefully.

