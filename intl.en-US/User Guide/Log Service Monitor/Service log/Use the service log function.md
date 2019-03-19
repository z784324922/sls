# Use the service log function {#concept_orq_blt_l2b .concept}

This topic describes how you can use the service log function. Specifically, it outlines how you can disable or enable the service log function, modify the log type and storage location. After you enable the server log function, Log Service stores all logs generated in the current Project to the specified Project. Note that this function is disabled by default.

## Prerequisites {#section_qj3_15p_m2b .section}

1.  A Project is created.
2.  Your RAM user account is authorized by your Alibaba Cloud account.

## Overview {#section_yfv_flt_ngb .section}

-   This function records operational logs generated in a specified Project and Logtail alarm logs. It also allows you to store the logs in a new Project or a specified existing Project.
-   This function automatically creates Logstores in the storage location you specified, separately storing operational logs and other logs.
-   Log Service provides five types of dashboards so that you can view and monitor the running status of logs in real time.

**Note:** 

-   Log Service creates Logstores and dashboards in your specified storage location. Note that the Logstore that is used to store operational logs is charged according to the standard billing method, whereas the Logstore that is used to store other logs can be used free of charge.
-   We recommend that logs generated within the same region be stored in the same Project automatically created by Log Service.

## Enable the service log function {#section_smj_zhr_m2b .section}

1.  Log on to the Log Service console and find the target Project.
2.  In the **Actions** column, click **Operations Log**.
3.  In the displayed dialog box, select a log type or types, which best meet your requirements.

    You can select **Operational logs** or **Other logs** or both for the **Enable Operations Log** field.

    -   **Operational logs**: Record all operations conducted on resources in a Project \(including creation, modification, deletion, update, writes, and reads\). They are stored in the **internal-operation\_log** Logstore of a specified Project.
    -   **Other logs**: Include metering logs at a Logstore granularity, consumption group delay logs, Logtail error information, heartbeat information, and statistical logs. They are stored in the **internal-diagnostic\_log** Logstore of a specified Project.
4.  Specify the **Log Storage**.

    -   If you choose **Automatic creation \(recommended\)**, Log Service will automatically create a Project named `log-service-{user ID}-{region}` in the current region. We recommend that all logs generated in this region be stored in this Project.
    -   If you choose an existing Project, service logs will be stored in the specified Project.
5.  Click **Confirm**.

## Modify the log type and storage location {#section_gwt_xb5_m2b .section}

1.  Log on with your Alibaba Cloud account to the Log Service console and find the target Project.
2.  In the **Actions** column, click **Operations Log**.
3.  Select the target option for the **Enable Operations Log** field.
4.  Select the target Project for the **Log Storage** field.

    **Note:** 

    -   We recommend that service logs be stored in the Project that is automatically created by Log Service and that logs generated in the same region be stored in the same Project.
    -   After you modify the log storage location, newly generated logs will be stored in the specified Project. The logs stored in the original Project will not be migrated to the new Project or deleted. If you do not need historical logs, you can manually delete them.

## Disable the service log function {#section_wxv_mpt_ngb .section}

**Note:** If the service log function is disabled, logs and dashboard information stored in the Project will not be deleted automatically. If you do not need them any more, you can delete the Project or Logstore where logs are stored manually.

1.  Log on with your Alibaba Cloud account to the Log Service console and find the target Project.
2.  In the **Actions** column, click **Operations Log**.
3.  Deselect all options for the **Enable Operations Log** field.
4.  Click **Confirm**.

## Grant permissions to RAM users {#section_cdl_nqy_pgb .section}

Before using your RAM user account to use Log Service functions, you must use your Alibaba Cloud account to grant related permissions to the RAM user. For more information, see [Grant RAM sub-accounts permissions to access Log Service](reseller.en-US/User Guide/Access control RAM/Grant RAM sub-accounts permissions to access Log Service.md). The following shows the corresponding policy:

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
      "Resource": "acs:log:*:*:project/{Project for which the service log function is enabled}/logging",
      "Effect": "Allow"
    }
  ]
}
```

