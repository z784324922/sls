# Configure Function Compute log consumption {#concept_tp3_b4q_zdb .concept}

Relying on the Function Compute service, Log Service provides a fully-hosted processing service for streaming data.

After configuring an ETL job, Log Service regularly retrieves updated data and triggers function execution, that is, incrementally consumes Log Service Logstore data to complete custom processing tasks in functions. Functions used to process data can be templates provided by Log Service or user-defined functions.

## Applicable scenario {#section_dwq_m2m_12b .section}

 **Data cleaning and processing** 

Log Service allows you to quickly collect, process, query, and analyze logs.

![](images/5804_en-US.png "Data cleaning and processing")

 **Data shipping** 

Log Service supports shipping data to the destination and constructs the data pipeline between cloud-based big data products.

![](images/5805_en-US.png "Data shipping")

## Working principles {#section_hwq_m2m_12b .section}

 **Trigger** 

A Log Service ETL job corresponds to a Function Compute trigger. After you create an ETL job, Log Service starts a timer based on the job configuration. The timer polls Logstore shard information. When a new log is written, the generated information which is composed of three elements < shard\_id, begin\_cursor, end\_cursor \> serves as a function event and triggers function execution.

Log Service ETL job is triggered based on time. For example, if the ETL job trigger interval is 60 seconds and data is consistently written to shard 0 of the Logstore, the function execution is triggered every minute for shard 0. If no new data is written to shard 0, the function execution is not triggered. The input for function execution is the cursor interval for the last 60 seconds. In the function, shard 0 data is read based on the cursor and then processed.

![](images/5806_en-US.png "Trigger")

## ETL functions {#section_jwq_m2m_12b .section}

You can use the function templates or user-defined functions. Before you get started, we recommend that you learn about the Basic concepts of Function Compute services.

-   Function templates maintained by Log Service

    Function templates are maintained on GitHub. Click [aliyun-log-fc-functions](https://github.com/aliyun/aliyun-log-fc-functions) to access the GitHub.

-   User-defined functions

    Implement your own functions. The function configuration formats are related to the specific function implementations. For more information, see [Development guide for ETL function](reseller.en-US/User Guide/Real-time subscription and consumption/Use Fuction Compute to cosume LogHub Logs/Development guide.md).


## User Guide {#section_kkq_y2m_12b .section}

## Step 1 Authorize Log Service and prepare resources {#section_z1p_z2m_12b .section}

1.  On the [quick authorization](https://ram.console.aliyun.com/#/role/authorize?request=%7B%22Requests%22%3A%20%7B%22request1%22%3A%20%7B%22RoleName%22%3A%20%22AliyunLogETLRole%22%2C%20%22TemplateId%22%3A%20%22ETL%22%7D%7D%2C%20%22ReturnUrl%22%3A%20%22https%3A//sls.console.aliyun.com/%22%2C%20%22Service%22%3A%20%22Log%22%7D) page, click **Confirm Authorization Policy** to grant function trigger permission to Log Service.
2.  Create a Log Service project and a Logstore for function process logs.

    If you have not created a project or a Logstore before, create one by following [Preparation](reseller.en-US/User Guide/Preparation/Preparation.md) process.

    **Note:** Log Service project and Function Compute service must be in the same region.


## Step 2 Create a service {#section_xmf_dfm_12b .section}

1.  In the Function Compute console, click **Create Service**.
2.  Enter the **Service Name** and **Description**. Turn on the **Advanced Settings** switch.

    |Configuration item|Meaning|
    |:-----------------|:------|
    |Service name|The name of the Function Compute service to be created. Naming rules:     -   The name can contain uppercase letters, lowercase letters, numbers, hyphens \(-\), and underscores \(\_\).
    -   The name must begin with an uppercase letter, lowercase letter, or underscore \(\_\).
    -   The name is case sensitive and must contain 1–128 characters.
 |
    |Feature description|The description of the new service.|
    |Log project|The name of the Log Service project. The Logstore must be in the same region as the new Function Compute service.|
    |Log repository|The name of the Log Service Logstore. The Logstore must be in the same region as the new Function Compute service.|
    |Role Operation|Create a service role and create the corresponding permissions based on the selected system template. Authorize Function Compute to push logs to the specified Logstore. You can create a new role or select an existing role. To use an existing role, you must select a role that already exists.|
    |System Policies|Select a system authorization policy. Select the system authorization policies. Log Service supports two system authorization policies: `AliyunLogFullAccess` and `AliyunLogReadOnlyAccess`.|

    ![](images/5807_en-US.png "Create a service")

    After selecting the system authorization policy, click **Authorize**. The Role Templates page appears. Confirm your **role information** and **permission information**, including the **Policy Name**, **Policy Description**, and **Policy Details**. If you are creating a new role, you must confirm the **Role Name** and **Role Description**. In the **Policy Details**, you can refine the authorization policy to customize an authorization policy suitable for this role.

    After the successful authorization, click **OK** to go to the Overview page of the service.


## Step 3 Create a function and a trigger {#section_scl_4fm_12b .section}

1.  On the Overview page of the service, click **Create Function**.

    Select a **function template**.

    You can select a business template similar to your business model and modify it to create a function, or select a blank function template to customize the function.

    -   Log Service template: Log Service template: Log Service provides the business templates `logstore_replication` and `oss-shipper-csv`. You can create a function and a trigger based on these templates.
    -   Blank template: You can use the blank function template to create a blank function. Then, on the guide page, configure the trigger, function parameters, and write the relevant code to create a function.
2.  Configure the **trigger** and then click **Next**.

    If you select a template provided by Log Service, you can configure the trigger directly. If you select the blank template, you must first select the trigger type and then configure the trigger.

    Complete the required items to configure the trigger, such as the trigger name, the project name, and the Logstore name. A Log Service type trigger of Function Compute corresponds to an ETL job of Log Service.

    |Configuration item|Meaning|Value|
    |:-----------------|:------|:----|
    |Trigger Name|The name of the new trigger.|The trigger name must be 1–256 bytes long and can contain English letters, numbers, underscores \(\_\), and hyphens \(-\). It cannot start with a number or hyphen \(-\).|
    |Project name|The name of the Log Service project.|It must be the name of an existing project. This project must be in the same region as your service.|
    |Logstore name|The name of the Log Service project. This trigger regularly transmits the subscribed data of this Logstore to Function Compute for custom processing. You cannot change this parameter after the ETL job is created.|Select an existing Logstore and the Logstore must belong to the project selected in **Log Project Name**.|
    |Trigger log|Log Service regularly triggers the function execution of Function Compute. Exceptions during the trigger process and function execution statistics are recorded in this Logstore. You can create an index for the Logstore for future viewing.|It must be the name of an existing Logstore and the Logstore must belong to the project selected in **Log Project Name**.|
    |Invocation Interval|The interval at which Log Service triggers function execution. For example, when set to 60 seconds, Log Service reads the data location in the last 60 seconds for each Logstore shard, using this as a function event to call function execution. In the function, the user logic reads the shard data and performs computation. If the Logstore shards have a high traffic volume \(over 1 Mbit/s\), we recommend you set a shorter trigger interval to ensure the data volume processed by each function operation is of a reasonable size.|The value range is 3–600 seconds.|
    |Retries|If an error occurs when Log Service triggers function execution according to the set trigger interval \(such as insufficient permissions, network failure, or function execution return exception\), this parameter sets the maximum number of times the function can be re-triggered. If the function is re-triggered the maximum number of times and the operation is still unsuccessful, the trigger interval must elapse before Log Service attempts to trigger the function execution again. The impact of reties on the business varies according to the specific function code implementation logic.|The value range is 0–100 times.|
    |Function configuration|Log Service uses this configuration content as a part of the function event to pass into the function. The way in which this function is used is determined by the custom logic of the function. Different types of functions have different requirements for function configurations. For the vast majority of provided function templates, you must read the instructions when entering your parameters. When no parameters are passed in by default, enter: `{}`.|The configuration content must be a string in JSON Object format.|

    ![](images/5808_en-US.png "Trigger configuration")

    **Note:** You already have the permissions to read/write Logstore data and allow Log Service to call your function.

3.  Complete the **basic configurations** 

    such as Function Name and Function Handler.Then, click **Next**.

4.  Complete the function **permissions**.

    Confirm the **template authorization** and **trigger role authorization**. Then, click **Next**.

5.  Review your **Function Information** and **Trigger Information**. Then, click **Create**.

## View trigger logs {#section_dxq_m2m_12b .section}

Log on to the Log Service console and create an index for the trigger log Logstore configured in the job. This allows you to view task execution statistics.

## View function operation logs {#section_exq_m2m_12b .section}

Log on to the Log Service console to view detailed information in the function execution process. For more information, see [Logging](https://www.alibabacloud.com/help/doc-detail/52704.htm).

## FAQs {#section_g2f_dgm_12b .section}

 **I created a trigger, but it does not trigger function execution** 

1.  Make sure you have used [quick authorization](https://ram.console.aliyun.com/#/role/authorize?request=%7B%22Requests%22%3A%20%7B%22request1%22%3A%20%7B%22RoleName%22%3A%20%22AliyunLogETLRole%22%2C%20%22TemplateId%22%3A%20%22ETL%22%7D%7D%2C%20%22ReturnUrl%22%3A%20%22https%3A//sls.console.aliyun.com/%22%2C%20%22Service%22%3A%20%22Log%22%7D) to authorize Log Service to trigger function execution.
2.  Make sure the data in the job's Logstore is incrementally modified, as function execution is triggered when shard data changes.
3.  Log on to the Log Service console and check if any exceptions exist in the trigger logs and function operation logs.

