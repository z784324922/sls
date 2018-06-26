# **Manage a collection configuration** {#concept_uq4_fln_fz .concept}

The Logtail client provides an easy way to collect logs from Elastic Compute Service \(ECS\) instances in the Log Service console. After installing the Logtail client, you must create a log collection configuration for the Logtail client. For how to install Logtail, see [Linux ](intl.en-US/User Guide/Logtail collection/Install/Linux .md) and [Install Logtail on Windows](intl.en-US/User Guide/Logtail collection/Install/Install Logtail on Windows.md). You can create and modify a Logtail configuration of a Logstore on the Logstore List page.

## Create a Logtail configuration {#section_gmp_lm1_ry .section}

For how to create a Logtail configuration in the Log Service console, see  [EN-US\_TP\_13062.dita](EN-US_TP_13062.dita) and [Syslog](intl.en-US/User Guide/Logtail collection/Data Source/Syslog.md).

## View Logtail configuration list {#section_jpr_mm1_ry .section}

1.  Log on to the Log Service console.
2.  On the Project List page, click the project name.
3.  On the Logstore List  page, click **Manage** at the right of the Logstore. The Logtail Configuration List page appears.

    All the configurations of this Logstore are displayed on this page, including the configuration name, data sources, and configuration details. When the data source is **Text**, the file path and file name are displayed under Configuration Details.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13077/5252_en-US.png "Logtail configuration list")

    **Note:** A file can be collected by only one configuration.


## Modify a Logtail configuration {#section_omw_pm1_ry .section}

1.  Log on to the Log Service console.
2.  On the Project List page, click the project name, or click **manage** on the right.
3.  On the Logstore List page, click **Manage**  at the right of the Logstore. The Logtail Configuration List page appears.
4.  Click the name of the Logtail configuration to be modified.

    You can modify the log collection mode and specify the machine groups that apply this modified configuration. The configuration modification process is the same as the configuration creation process.


## Delete a Logtail configuration {#section_vgw_rm1_ry .section}

1.  Log on to the Log Service console.
2.  On the Project List page, click the project name, or click **manage** on the right.
3.  On the Logstore List page, click **Manage** at the right of the Logstore. The Logtail Configuration List page appears.
4.  Click **Remove** at the right of the Logtail configuration to be deleted.

    After the configuration is deleted successfully, it is unbound from the machine groups that applied this configuration and Logtail stops collecting the log files of the deleted configuration.

    **Note:** You must delete all the Logtail configurations in a Logstore before deleting the Logstore.


