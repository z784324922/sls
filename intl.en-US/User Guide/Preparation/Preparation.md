# Preparation {#task_nn1_y2x_ndb .task}

Log Service provides multiple log collection methods. You can use Log Service to collect Elastic Compute Service \(ECS\) logs, local server logs, IoT device logs, and other cloud product logs.

Before using Log Service, you must first make the following preparations.

1.   Activate Log Service 

    Log on to the Log Service product page with your registered Alibaba Cloud account. Click **Get it Free**. The Enable Service page appears.  Select the **I agree with Log Service Agreement of Service** check box and then click Enable Now to activate Log Service. You will be automatically redirected to the Log Service console.

2.   eate and enable AccessKey \(for API/SDKs \). 

    An AccessKey is required to collect logs by using Logtail.  Before using Log Service, you must create an AccessKey.

    In the [Log Service console](https://partners-intl.console.aliyun.com/#/sls), hover your mouse over your avatar in the upper-right corner. Select **accesskeys** from the drop-down list . Click Continue to manage **AccessKey**  in the appeared dialog box. The **Access Key  Key Management page** appears. Create an AccessKey and check whether the created  AccessKey is **enabled**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13022/15381293062434_en-US.png)

3.   Create a project When you log on to the Log Service console for the first time, the system prompts you to create a project.  You can also click  **Create a project **  in the upper-right corner.

    You can also modify project description and delete a project. For more information, see [Manage a project](reseller.en-US/User Guide/Preparation/Manage a project.md).

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13022/15381293062436_en-US.png)

4.   Create Logstore. The system prompts you to create a Logstore after you create a project.  You can also click the project name and then click  **Create** in the upper-right corner. When creating a Logstore, you must specify how you are going to use these logs.

    You can also modify or delete the Logstore. For more information, see [Manage a Logstore ](reseller.en-US/User Guide/Preparation/Manage a Logstore .md).

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13022/15381293062437_en-US.png)

5.   Manage shards \(optional\) When creating a Logstore, you can select the number of shards based on the volume and generation speed of your logs. You can also change the number of shards by splitting or merging shards when modifying the Logstore.

    For more information about splitting and merging shards, see [Manage a Shard](reseller.en-US/User Guide/Preparation/Manage a Shard.md).

6.   Perform RAM authorization \(optional\) If you need to collect cloud product logs or post Log Service data to OSS or another product for storage and analysis, you must grant the relevant permissions for Log Service or other cloud products.

    To use a sub-account to perform operations in Log Service, you must grant permissions to the sub-account in the Resource Access Management \(RAM\) console.

    For more information about the authorization policies and procedure, see [Authorization - Overview](reseller.en-US/User Guide/Access control RAM/Authorization - Overview.md).


