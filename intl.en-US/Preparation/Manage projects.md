# Manage projects {#concept_mxk_414_vdb .concept}

You can create and delete projects in the Log Service console.

## Create a project {#section_ahq_ggx_ndb .section}

**Note:** 

-   Currently, you can create projects only in the Log Service console.
-   The project name must be globally unique among all Alibaba Cloud regions. If the project name that you entered already exists, the message **This project name is already in use by other user, please change other name** appears. You can enter another project name and try again.
-   To create a project, you must specify the Alibaba Cloud region based on the source of the logs to be collected and other actual conditions. To collect logs from an Alibaba Cloud Elastic Compute Service \(ECS\) instance, we recommend that you create the project in the same region as the ECS instance. This speeds up log collection, and collects logs on the Alibaba Cloud intranet without occupying the Internet bandwidth of the ECS instance.
-   After a project is created, its region cannot be changed, and the project cannot be migrated. Therefore, exercise caution when selecting the region of a project.
-   You can create up to 50 projects in all Alibaba Cloud regions with an Alibaba Cloud account.

 **Procedure** 

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).
2.  Click **Create Project** in the upper-right corner.
3.  Set **Project Name** and **Region**, select **Enable Operations Log** as required, and then click **Confirm**.

    |Parameter|Description|
    |---------|-----------|
    |Project Name|The name of the project. The project name can contain only lowercase letters, digits, and hyphens \(-\). It must start and end with a lowercase letter or digit and must be 3 to 63 bytes in length. **Note:** After a project is created, its name cannot be changed.

 |
    |Description|The brief description of the project. After the project is created, the description is displayed on the Projects page. To modify the description after the project is created, go to the Projects page and click Modify in the Actions column. **Note:** The description is 0 to 64 bytes in length and does not support the following characters: `<> " ' \`.

 |
    |Region|The Alibaba Cloud region where the project is located. The region cannot be modified after the project is created, and the project cannot be migrated among regions.|
    |Enable Operations Log|Specifies whether Log Service records the operations, access, and metering logs of all resources in the project. This feature takes effect in 1 or 2 minutes after it is enabled.|
    |Log Storage|The location where logs are stored. This parameter appears when you select an Enable Operations Log check box. Valid values:     -   Current project
    -   Automatic creation \(recommended\): specifies an automatically created project in the same region.
 |


![](images/7306_en-US.png "Create a project")

## Modify a project {#section_l24_nfj_n2b .section}

You can modify a project to modify the description of the project, enable or disable the service log feature, or modify the log storage location.

1.  Log on to the Log Service console.
2.  Find the target project. You can enter the project name in the search box in the upper-left corner of the Projects page and click **Search**.
3.  Click **Modify** in the **Actions** column of the project.
4.  In the dialog box that appears, modify the project configuration.

    **Note:** You cannot modify the project name or region.


## Delete a project {#section_on2_3gx_ndb .section}

In some cases \(for example, to disable the operations log feature or destroy all the logs in a project\), you may need to delete the entire project.

**Note:** After you delete a project, all its logs and configuration are permanently released and cannot be recovered. Therefore, exercise caution when deleting projects to avoid losing data.

1.  On the Projects page, select the project to be deleted.
2.  Click **Delete** in the Actions column.
3.  In the dialog box that appears, select the reason for deletion.

    If you select **Other issues**, describe the reason in the field below.

4.  Click **Confirm**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13023/15673883612574_en-US.png)


