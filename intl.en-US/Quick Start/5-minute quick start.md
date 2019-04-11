# 5-minute quick start {#concept_gpw_x2w_ydb .concept}

Log Service is a platform provided by Alibaba Cloud for collecting, storing, and querying massive logs. You can use Log Service to centrally collect all the logs from the service cluster. It also supports real-time consumption and query.

This document demonstrates the basic workflow of configuring Logtail to collect Alibaba Cloud Elastic Compute Service \(ECS\) logs in the Windows environment. This case is related to the basic functions of Log Service, such as collecting logs and querying logs in real time, and is an entry-level user guide of Log Service.

## Log service operation process {#section_qnh_hr4_12b .section}

![](images/5841_en-US.png "Procedures")

## Step 1. Getting started {#section_lyq_jr4_12b .section}

## 1. Activate Log Service {#section_b5t_gr4_12b .section}

Use a registered Alibaba Cloud account to log on to the Log Service product page and click   **Get it Free**.

## 2. Create an AccessKey \(optional\) {#section_c5t_gr4_12b .section}

**Note:** If you want to write data using SDK, create a primary account or sub-account AccessKey. Log collection does not require the creation of AccessKey.

In the Log Service console, hover your mouse over your avatar in the upper-right corner and click **accesskeys** in the displayed drop-down list.  In the dialog box, click   **Continue to manage AccessKey** to go to the Access Key Management page. Then, create an AccessKey. Make sure the status is set to Enabled. Then, create an AccessKey. Make sure the status is set to **Enabled** .

![](images/5842_en-US.png "Enable AK")

## 3. Create a project {#section_e5t_gr4_12b .section}

If you have logged on to the Log Service console for the first time, the system prompts you to create a project. You can also click **Create Project** in the upper-right corner to create a project.

When creating a project, you must specify the **Project Name** and **Region** based on your actual needs. Among the regions, **cn-shanghai-internal-prod-1** and **cn-hangzhou-internal-prod-1** are used for internal Log Service, while the other regions are in the public cloud.

![](images/5843_en-US.png "Create a project")

## 4. Create a Logstore {#section_g5t_gr4_12b .section}

After creating a project, you is be prompted to create a Logstore. You can also go to the project and click **Create** in the upper-right corner. When creating a Logstore, you must specify how you are going to use these logs.

![](images/5844_en-US.png "Creating a Logstore")

## Step 2. Install Logtail client on ECS instance {#section_ykp_qr4_12b .section}

## 1. Download the installation package {#section_i5t_gr4_12b .section}

Download the Logtail installation package to an ECS instance. Click [here](http://logtail-release.oss-cn-hangzhou.aliyuncs.com/win/logtail_installer.zip) to download the Windows installation package.

## 2. Install Logtail {#section_j5t_gr4_12b .section}

Extract the installation package to the current directory and then enter the `logtail_installer` directory. Run cmd as an administrator, and run the installation command `.\logtail_installer.exe install cn_hangzhou`.

**Note:** You must run different installation commands according to the network environment and the region of Log Service. This document uses the ECS classic network in China East 1 \(Hangzhou\) as an example. For other areas, see [Install Logtail in Windows](../../../../../reseller.en-US/User Guide/Logtail collection/Install/Install Logtail in Windows.md).

For the installation commands of other regions, see [Install Logtail in Windows](../../../../../reseller.en-US/User Guide/Logtail collection/Install/Install Logtail in Windows.md) and [Install Logtail in Linux](../../../../../reseller.en-US/User Guide/Logtail collection/Install/Install Logtail in Linux.md).

## Step 3. Configure data import wizard {#section_k5t_gr4_12b .section}

In the Log Service console, click the project name to go to the Logstore List page. Click 1 at the **right** of the Logstore to enter the **Logtail configuration**. You can also click **Manage** at the right of the Logstore to create a configuration in the Logtail configuration list.

Logtail configuration process includes the following steps: Select Data Source, Configure Data Source , Search, Analysis, and Visualization, Shipper & ETL. The last two steps are optional.

## 1. Select data source {#section_l5t_gr4_12b .section}

Log Service supports the log collection of many cloud products, self-built softwares, and custom data. This document uses collecting text logs as an example. For more information, see [Collect text logs](../../../../../reseller.en-US/User Guide/Logtail collection/Text logs/Collect text logs.md).

Click **Text** in **Other Sources**, and then click Next.

## 2. Configure data source {#section_m5t_gr4_12b .section}

-   Specify the Configuration name and Log path.

    Follow the page prompts to enter the configuration name, log path, and log file name.  The log file name can be a full name, and supports wildcard matching at the same time.

-   Specify the log collection mode.

    Log Service currently supports parsing logs in simple mode, delimiter mode, JSON mode, full mode, or Alibaba Cloud custom mode. This document uses the delimiter mode as an example. For more information about the collection modes, see [Collect text logs](../../../../../reseller.en-US/User Guide/Logtail collection/Text logs/Collect text logs.md) and [Configure and parse text logs](../../../../../reseller.en-US/User Guide/Logtail collection/Text logs/Configure and parse text logs.md).

    ![](images/5845_en-US.png "Configure the data source")

-   Enter the log sample.

    You must enter the log sample if **Delimiter Mode** or **Full Mode** is selected as the log collection mode. Log Service supports parsing the log sample according to selected configuration when configuring Logtail. If the log sample failed to be parsed, you must modify the delimiter configurations or regular expressions. Enter the log sample to be parsed in the Log Sample field.

-   Specify the delimiter.

    You can specify the delimiter as a tab, a vertical line, or a space. You can also customize the delimiter. Select the delimiter according to your log format. Otherwise, logs fail to be parsed.

-   Specify the key in the log extraction results.

    After you enter the log sample and select the delimiter, Log Service extracts log fields according to your selected delimiter, and defines them as Value. You must specify the corresponding Key for the Value.

    ![](images/5846_en-US.png "Log content extraction results")

-   Configure the advanced options as needed.

    Generally, keep the default configurations of the advanced options. For how to configure the advanced options, see the related descriptions in [Collect text logs](../../../../../reseller.en-US/User Guide/Logtail collection/Text logs/Collect text logs.md).

-   Apply to the machine group.

    If you have not created a machine group before, create a machine group according to the page prompts. Then, apply the Logtail configuration to the machine group.

    **Note:** To create Armory to associate with the machine group, jump to the specified internal link as instructed on the page.


After completing these steps, Log Service begins to collect logs from the Alibaba Cloud ECS instance immediately. You can consume the collected logs in real time in the console and by using API/SDK.

To query, analyze, ship, or consume the logs, click **Next**.

**Note:** 

-   It can take up to 3 minutes for the Logtail configuration to take effect.
-   To collect IIS access logs, see  [Use Logstash to collect IIS logs](../../../../../reseller.en-US/User Guide/Other collection methods/Common log formats/Use Logstash to collect IIS logs.md).
-   For the Logtail collection errors, see [Query diagnosed errors](../../../../../reseller.en-US/FAQ/Log collection/Diagnose collection errors.md).

## Search, analysis, and visualization {#section_s5t_gr4_12b .section}

After the collection is configured, your ECS logs are collected in real time. To query and analyze the collected logs, configure the indexes in the data import wizard as follows.

You can click **Search** on the Logstore List page to go to the query page. Click **Enable** in the upper-right corner and configure the indexes on the displayed Search & Analysis page.

-   **Full text index attributes**

    You can enable the **Full Text Index Attributes**. Confirm whether to enable **Case Sensitive**, and confirm the **Token** contents.

-   **Key/value index attributes**

    Click the plus icon at the right of **Key** to add a line. Configure the **Key**, **Type**, **Alias**, **Case Sensitive**, and **Token**, and select whether to **enable analytics**.


**Note:** 

1.  Full text or key/value indexes attributes at least one must be enabled.  When both types are enabled, key/value index attributes prevail.
2.  When the index type is long or double, the Case Sensitive and Token attributes are unavailable.
3.  For how to configure indexes, see [Overview](../../../../../reseller.en-US/User Guide/Index and query/Overview.md).
4.  To use Nginx template or MNS template, configure the attributes on the Search & Analysis page after clicking **Enable** on the query page.

 ![](images/5848_en-US.png "query analysis") 

After configuring the query and analysis, click **Next** if you want to configure the log shipping. To experience the query and analysis, go back to the Logstore List page and click **Search** to go to the query page. You can enter the keyword, topic, or query & analysis statement, and select the time range to query logs. Log Service provides intuitive histograms to preview the query results. You can click the histogram to query logs in a more detailed time range. For more information, see [Overview](../../../../../reseller.en-US/User Guide/Index and query/Overview.md).

Log Service also supports querying and analyzing logs in many ways such as quick query and statistical graphs. For more information, see [Other functions](../../../../../reseller.en-US/User Guide/Index and query/Query/Other functions.md).

For example, to query all the logs within the last 15 minutes, you can set an empty query condition and select 15 min as the time range.

## 4. Shipping  {#section_jcr_nsq_12b .section}

Log Service not only supports collecting data with multiple sources and formats in batch, managing and maintaining the data, but also supports shipping log data to cloud products such as Object Storage Service \(OSS\) for calculation and analysis. 

To ship logs to [OSS](../../../../../reseller.en-US/User Guide/Data shipping/Ship logs to OSS.md), click **Enable**.

This document uses OSS storage as an example. See [Ship logs to OSS](../../../../../reseller.en-US/User Guide/Data shipping/Ship logs to OSS/Ship logs to OSS.md)to complete the authentication.

Click **Enable**, the OSS LogShipper dialog box appears. For descriptions about the configurations, see [Ship logs to OSS](../../../../../reseller.en-US/User Guide/Data shipping/Ship logs to OSS/Ship logs to OSS.md). After the configuration is complete, click **Confirm** to complete the shipping.

![](images/5910_en-US.png "Configure shipping")

Besides the basic functions such as accessing, querying, and analyzing logs, Log Service also provides many ways to consume logs. For more information, see User Guide.

