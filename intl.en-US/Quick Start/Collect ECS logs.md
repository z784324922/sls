# Collect ECS logs {#concept_tr2_2yr_xdb .concept}

This topic describes how to use Logtail to collect ECS logs in the Log Service console.

## Configuration process {#section_jq2_pzr_xdb .section}

1.  Install Logtail on your server.
2.  Configure a Logtail machine group.
3.  Create a Logtail Config and apply it to the machine group.

![](images/3871_en-US.png "Configuration process")

## Prerequisites {#section_f34_lys_xdb .section}

-   You have activated ECS and Log Service.
-   You have create a project and a Logstore. For more information, see [Preparations](../../../../reseller.en-US/User Guide/Preparation/Preparation.md).

    **Note:** If your ECS instance uses a classic network or a VPC, the ECS instance and the Log Service project must belong to the same region.

-   If the ECS instance is created under another Alibaba Cloud account, you cannot automatically obtain the ECS instance owner information. In this case, you must [configure an AliUid for the ECS instance](../../../../reseller.en-US/User Guide/Logtail collection/Machine Group/Configure AliUids for ECS servers under other Alibaba Cloud accounts or on-premises IDCs.md).

## Step 1: Install Logtail. {#section_xqh_bxs_xdb .section}

1.  Run the installation command.

    Choose a Logtail installation script according to the region to which the ECS instance belongs. For more information, see [Install Logtail in Linux](../../../../reseller.en-US/User Guide/Logtail collection/Install/Install Logtail in Linux.md) and [Install Logtail in Windows](../../../../reseller.en-US/User Guide/Logtail collection/Install/Install Logtail in Windows.md).

    For example, if your Linux ECS instance belong to the China \(Hangzhou\) region and uses a classic network, you can run the following command to install Logtail:

    ```
    wget http://logtail-release.oss-cn-hangzhou-internal.aliyuncs.com/linux64/logtail.sh; chmod 755 logtail.sh; sh logtail.sh install cn_hangzhou
    ```

2.  Check the Logtail run status by running the following command:

    ```
    /etc/init.d/ilogtaild status
    ```

    Logtail is successfully installed if `ilogtail is running` is returned.


![](images/3872_en-US.png "Install Logtail")

## Step 2: Configure a machine group. {#section_uy3_vxs_xdb .section}

1.  In the Log Service console, click the target project.
2.  On the Logstores page, click **Logtail Machine Group** in the left-side navigation pane.
3.  On the Machine Groups page, click **Create Machine Group**.
4.  In the displayed dialog box, enter your ECS intranet IP address and custom ID, and then click **Confirm**.

    **Note:** 

    -   Only ECS instances that belong to the same region with the Log Service project is supported.
    -   Only ECS instances in the same region as the Log Service project are supported.

![](images/3874_en-US.png "Configure a machine group")

## Step 3: Create a Logtail Config. {#section_wt2_vzs_xdb .section}

1.  On the **Logstores** page, find the target Logstore and click the **Data Import Wizard** icon.
2.  On the Select Data Source tab page, click **Text File** in the **Custom Data**.
3.  Configure the data source. For more information, see [Collect text logs](../../../../reseller.en-US/User Guide/Logtail collection/Text logs/Collect text logs.md). This topic uses the **Simple Mode** as an example.

    Enter the ECS log path in the **Log Path** text box, and then click **Next**.

    ![](images/3873_en-US.png "Simple mode")

4.  Select the machine group you created in **Step 2** and click **Apply to Machine Group**.

    ![](images/3875_en-US.png "Apply the data source to the created machine group")


You can then use Logtail to collect ECS logs. The following steps are for configuring indexes of collected logs or deliver logs.

## View logs {#section_e43_vbt_xdb .section}

Log on to the ECS console or run the `echo "test message" >> /var/log/message` command. The new logs are generated in your local directory `/var/log/message`. Logtail then collects the new logs and send them to Log Service.

On the Logstores page, find the target Logstore and click **Search** or **Preview** to view the logs collected by Logtail.

![](images/3876_en-US.png "View logs")

![](images/3877_en-US.png "Preview logs")

![](images/3878_en-US.png "Retrieve logs")

