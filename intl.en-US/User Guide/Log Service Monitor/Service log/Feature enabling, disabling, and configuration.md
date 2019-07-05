# Feature enabling, disabling, and configuration {#concept_orq_blt_l2b .concept}

You can enable or disable the service log feature and change the logging settings for a specified project in the project list. Log Service stores all the logs generated for the project to a new or existing project. By default, the service log feature is disabled.

## Prerequisites {#section_qj3_15p_m2b .section}

1.  Projects are created.
2.  [Permissions](#) are granted to a RAM user from your Alibaba Cloud account if you log on to the Log Service console as the RAM user.

## Background {#section_yfv_flt_ngb .section}

Log Service provides the service log feature to record operations logs and other logs \(such as Logtail alert logs\) of a specified project. Log Service stores the logs in a new or exiting project. Log Service automatically creates Logstores in your specified storage location to separately store operations logs and other logs. Log Service also provides five dashboards for various log scenarios so that you can view and monitor the running status of Log Service in real time.

**Note:** 

-   After you enable the service log feature, Log Service creates Logstores and dashboards in your specified storage location. The Logstore used to store **operations logs** is charged based on your specified billing method. The Logstore used to store **other logs** can be used free of charge.
-   We recommend that you store logs generated within the same region in the same project that is automatically created by Log Service.
-   Only service logs generated after feature enabling are recorded.

## Enable the service log feature {#section_smj_zhr_m2b .section}

1.  Log on to the Log Service console and find the target project.
2.  In the **Actions** column, click **Operations Log**.
3.  In the Enable Operations Log dialog box that appears, select the type of logs that you want to record in the **Enable Operations Log** field.

    You can select the **Operations logs** and **Other logs** check boxes as required.

    -   **Operations logs**: record all operations performed on resources in the target project, including the creation, modification, update, deletion, write, and read operations. These logs are stored in the **internal-operation\_log** Logstore of the target project.
    -   **Other logs**: include metering logs, logs about delayed consumption in a consumer group, and Logtail-related error, heartbeat, and statistical logs in each Logstore. These logs are stored in the **internal-diagnostic\_log** Logstore of the target project.
4.  Set **Log Storage**.

    -   If you select **Automatic creation \(recommended\)**, Log Service automatically creates a project named `log-service-{User ID}-{Current region}` in the same region as the target project. We recommend that you store logs generated within the same region in this created project.
    -   If you select an existing project, Log Service stores service logs in this project.
5.  Click **Confirm**.

    The service log feature is enabled. Log Service records the logs generated for the target project in the specified location in real time.

    ![](images/7234_en-US.png "Enable the service log feature")


## Change the log type and storage location {#section_gwt_xb5_m2b .section}

1.  Log on to the Log Service console with your Alibaba Cloud account and find the target project.
2.  In the **Actions** column, click **Operations Log**.
3.  Change the log type.

    Select or clear the Operations logs or Other logs check box as required in the **Enable Operations Log** field.

4.  Change the value of **Log Storage**.

    In the **Log Storage** drop-down list, select another project.

    **Note:** 

    -   We recommend that you store service logs in the project that is automatically created by Log Service. We also recommend that you store logs generated within the same region in the same project.
    -   After you change the value of **Log Storage**, new log data is stored in the specified project. The log data and dashboards stored in the original project are not automatically deleted or migrated to the newly specified project. If you no longer need the data, you can manually delete it.
    ![](images/7235_en-US.png "Change the log type and storage location")


## Disable the service log feature {#section_wxv_mpt_ngb .section}

**Note:** After the service log feature is disabled, the log data and dashboards stored in the project are not automatically deleted. If you no longer need the log data, you can manually delete the project or Logstore that stores the log data.

1.  Log on to the Log Service console with your Alibaba Cloud account and find the target project.
2.  In the **Actions** column, click **Operations Log**.
3.  Clear both the Operations logs and Other logs check boxes.

    ![](images/10092_en-US.png)

4.  Click **Confirm**.

## Authorize a RAM user {#section_cdl_nqy_pgb .section}

Before using the service log feature with a RAM user, you must obtain necessary permissions from your Alibaba Cloud account. For more information, see [Grant RAM sub-accounts permissions to access Log Service](reseller.en-US/User Guide/Access control RAM/Grant a RAM user the permissions to access Log Service.md). The following code lists the permission policies:

```
{
  "Version": "1",
  "Statement": [
    {
      "Action": [
        "log:CreateDashboard",
        "log:UpdateDashboard"
      ],
      "Resource": "acs:log:*:*:project/{Project where logs are stored}/dashboard/*",
      "Effect": "Allow"
    },
    {
      "Action": [
        "log:GetProject",
        "log:CreateProject",
        "log:ListProject"
      ],
      "Resource": "acs:log:*:*:project/*",
      "Effect": "Allow"
    },
    {
      "Action": [
        "log:List*",
        "log:Create*"
        "log:Get*",
        "log:Update*",
      ],
      "Resource": "acs:log:*:*:project/{Project where logs are stored}/logstore/*",
      "Effect": "Allow"
    },
    {
      "Action": [
        "log:*"
      ],
      "Resource": "acs:log:*:*:project/{Project for which the service log feature is enabled}/logging",
      "Effect": "Allow"
    }
  ]
}
```

