# Manage a project {#concept_mxk_414_vdb .concept}

In the Log Service console, you can: Create a project.  Delete a project.

## Create a project  {#section_ahq_ggx_ndb .section}

**Note:** 

-   Currently, Log Service can only create projects in the console.
-   The project name must be globally unique among all Alibaba Cloud regions.  The message “**Project XXX  already exists**” is displayed if the project name you entered has already been used by another user. Enter another project name and try again. 
-   To create a project, you must specify the Alibaba Cloud region based on the  source of the logs to be collected and other actual conditions. To collect logs from an Alibaba Cloud Elastic Compute Service \(ECS\) instance, we recommend  that you create the project in the same region as the ECS  instance to speed up log collection, and collect logs by using Alibaba Cloud intranet \(without occupying the Internet bandwidth of the ECS instance\).
-   The region in which the project resides cannot be changed after the project is created. Log Service currently does not support migrating projects, so proceed with caution when selecting the region in which the project resides.
-   You can create up to 50 projects in all Alibaba Cloud regions.

**Procedure**

1.  Log on to the Log Service console.
2.  Click **Create Project** in the upper-right corner.
3.  Enter the **Project Name** and select the **Region**. Then, click    **Confirm**.

    |Configuration items|Description|
    |-------------------|-----------|
    |Project name|Enter the project name. The name can be 3–63 characters long, contain lowercase letters, numbers, and hyphens \(-\), and must begin and end with a  lowercase letter or number.**Note:** The project name cannot be modified after the project is created.

|
    |Description|Enter a simple description for the project. After the project is created, the description is displayed on the Project List  page. It can be modified by clicking Modify at the right of the project  on the Project List.|
    |Region |You must specify an Alibaba Cloud region for each project. The region cannot be modified after the project is created, and the project cannot be migrated among regions. |


## Delete a project  {#section_on2_3gx_ndb .section}

You can delete a project in some situations, such as disabling Log Service and  deleting all the logs in a project.  Log Service allows you to delete a project in the console. 

**Note:** After a project is deleted, all the log data and configuration information managed by this project are permanently released and are not recoverable.  Therefore, proceed with caution when deleting  a project to avoid data loss.

1.  Log on to the Log Service console. 
2.  On the Project List page, click **Delete** 

