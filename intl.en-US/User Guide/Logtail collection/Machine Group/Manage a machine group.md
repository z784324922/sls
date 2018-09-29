# Manage a machine group {#concept_hcp_l3q_zdb .concept}

Log Service manages all the Elastic Compute Service \(ECS\) instances whose logs need to be collected by using the Logtail client in the form of machine groups. You can go to the **Machine Groups** page by clicking a project name on the Project List page and then clicking LogHub - Collect \> Logtail Machine Group in the left-side navigation pane on the Logstore List page. You can create, modify, and delete a machine group, view the machine group list and status, manage the configurations, and use the machine group identification in the Log Service console.

## Create a machine group {#section_xyw_f1f_x1b .section}

You can define a machine group by using:

-   IP: Define the machine group name and add the intranet IP addresses of a group of machines.
-   User-defined identity: Define an identity for the machine group and configure the identity on the corresponding machine for association.

For how to create a machine group, see [Create a machine group](reseller.en-US/User Guide/Logtail collection/Machine Group/Create a machine group.md).

## View machine group list {#section_sff_2p1_ry .section}

1.  Log on to the Log Service console.
2.  On the Logstore List page, click Logtail Machine Group in the left-side navigation pane. The Machine Groups page appears. 

    View all of the machine groups in the project.

    ![](images/5264_en-US.png "View a list of machine groups")


## Modify a Machine Group {#section_hf4_jp1_ry .section}

After creating a machine group, you can adjust the ECS instances in the machine group as per your needs.

**Note:** The machine group name cannot be modified after the machine group is created.

1.  Log on to the Log Service console.
2.  On the Logstore List page, click Logtail Machine Group in the left-side navigation pane. The Machine Groups page appears. 

    All machine groups under the project are displayed.

3.  Click **Modify** at the right of the machine group.
4.  Modify the configurations and then click **Confirm**.

![](images/5265_en-US.png "Modify a Machine Group")

## View status {#section_ijx_mp1_ry .section}

To verify that the Logtail client is successfully installed on all ECS instances in a machine group, view the heartbeat status of the Logtail client.

1.  Log on to the Log Service console.
2.  On the Project List page, click the project name. On the Logstore List page, click LogHub - Collect \> Logtail Machine Group in the left-side navigation pane.  The Machine Groups page appears. 
3.  Click **Machine Status** at the right of the machine group.

    If the Logtail client is successfully installed on all ECS instances, If the heartbeat status of the ECS instances are OK. If the heartbeat status is FAIL, we recommend that you find the reason as instructed on the page. If the issue cannot be solved by yourself, open a ticket for help.

    ![](images/5266_en-US.png "View the machine group status")

    **Note:** 

    -   The heartbeat status OK indicates that the Logtail client properly connects to Log Service. After a machine is added to a machine group, a delay of several minutes might exist before you view the heartbeat status OK, so be patient.
    -   If the heartbeat status of an ECS instance is always FAIL, see [Linux ](reseller.en-US/User Guide/Logtail collection/Install/Linux .md) and [Install Logtail on Windows](reseller.en-US/User Guide/Logtail collection/Install/Install Logtail on Windows.md).

## Managing configurations {#section_gqq_rp1_ry .section}

Log Service manages all the servers whose logs need to be collected by using machine groups. One important management item is the collection configuration of the Logtail client. For more information, see [Collect text logs](reseller.en-US/User Guide/Logtail collection/Data Source/Collect text logs.md) and [Syslog](reseller.en-US//Syslog.md).  You can apply or delete a Logtail configuration to/from a machine group to decide what logs are collected, how the logs are parsed, and to which Logstore the logs are sent by the Logtail on each ECS instance.

1.  Log on to the Log Service console.
2.  On the Logstore List page, click Logtail Machine Group in the left-side navigation pane. The Machine Groups page appears. 
3.  Click **Config** at the right of the machine group.
4.  Select the Logtail configuration and click Add or Remove to add or remove the configuration to/from the machine group.

    After a Logtail configuration is added, it is issued to the Logtail client on each ECS instance in the machine group.  After a Logtail configuration is removed, it is removed from the Logtail client.


![](images/5267_en-US.png "Managing Machine Group configurations")

## Delete a machine group {#section_dqb_tp1_ry .section}

1.  Log on to the Log Service console.
2.  On the Project List page, click the project name. On the Logstore List page, click LogHub - Collect \> Logtail Machine Group in the left-side navigation pane.  The Machine Groups page appears. 
3.  Click **Delete** at the right of the machine group.
4.  Click **Confirm** in the appeared dialog box.

    ![](images/5268_en-US.png "Delete a machine group")


