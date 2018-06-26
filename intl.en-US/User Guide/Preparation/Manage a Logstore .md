# Manage a Logstore  {#concept_xkb_zh5_vdb .concept}

A Logstore is a collection of resources created in a project. All data in a Logstore is from the same data source.  The Logstore is a unit to query, analyze, and ship the collected log data. In the Log Service console, you can: 

-   [Create a Logstore.](#section_v52_2jx_ndb)
-   [Modify Logstore configurations ](#section_evc_rjx_ndb)
-   [Delete a Logstore](#section_ezq_vjx_ndb)

## Create a Logstore. {#section_v52_2jx_ndb .section}

**Note:** 

-   Each Logstore must be created in the certain project.
-   Up to 10 Logstores can be created in each Log Service project.
-   The Logstore name must be unique in the project.
-   The data retention time can be modified after a Logstore is created.  Click **Modify** at the right**of the Logstore ** \> **on the Logstore List page, ** change the **Data Retention Time**  and then click **Modify**.

1.  On the Project List page,  click the project name. Click **Create** to create a Logstore. 

    You can also click  **Create** in the dialog box after creating a project. 

2.  Complete the configurations and click **Confirm**. 

    |Configuration item|Description|
    |------------------|-----------|
    |Logstore name |The Logstore name, which must be unique in the project where it belongs. The name can be 3–63 characters long, contain lowercase letters, numbers, hyphens \(-\), and underscores \(\_\), and  must begin and end with a lowercase letter or number.**Note:** The Logstore name cannot be modified after the Logstore is created. 

|
    |WebTracking |Select whether or not to enable the WebTracking function.  This function supports collecting log data from HTML, H5, iOS, or Android platform to Log Service.  Disabled by default.|
    |Data Retention Time |The time \(in days\) the collected logs are kept in the Logstore. It can be 1–365 days. Logs are deleted if the specified time is exceeded. It can be 1–365 days.  Logs are deleted if the specified time is exceeded.|
    |Number of shards |The number of shards for the Logstore. Each Logstore can create 1–10 shards  and each project can create at most 200 shards.|

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13024/2585_en-US.png)


## Modify Logstore configurations  {#section_evc_rjx_ndb .section}

After a Logstore is created, you can modify the Logstore configurations as needed.

1.  Log on to the Log Service console.
2.  On the Project List page, click the project name.
3.  On the Logstore List  page, click   **Modify** at the right of the Logstore.
4.  The Modify Logstore Attributes dialog box appears. Modify the Logstore configurations and then close the dialog box.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13024/2586_en-US.png)


## Delete a Logstore {#section_ezq_vjx_ndb .section}

You can delete a Logstore in some situations, such as abandoning a Logstore. Log Service allows you to delete a Logstore in the console. Log Service allows you to delete a Logstore in the console.

**Note:** 

-   After a Logstore is deleted, the log data stored in the Logstore is permanently deleted and unrecoverable. So proceed with caution.
-   Delete all the corresponding Logtail configurations before deleting a Logstore.

1.  Log on to the Log Service console.
2.  On the Project List page, click the project name.
3.  On the Logstore List  page, click  **Delete** at the right of the Logstore you are about to delete. 
4.  Click **OK** in the displayed dialog box.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13024/2587_en-US.png)


