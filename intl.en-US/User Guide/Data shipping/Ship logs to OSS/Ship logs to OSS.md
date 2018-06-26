# Ship logs to OSS {#concept_h4x_h4q_zdb .concept}

Log Service can automatically archive Logstore data to Object Storage Service \(OSS\) to achieve more functions of logs.

-   OSS data supports lifecycle configuration for long-term log storage.
-   You can consume OSS data by using self-built programs and other systems \(for example, E-MapReduce\).

## Function advantages {#section_gg4_g4j_5cb .section}

Using Log Service to ship logs to OSS has the following advantages:

-   Ease of use.  You can configure to synchronize Logstore data of Log Service to OSS in the console.
-   Improved efficiency.  The log collection of Log Service centralizes logs of different machines, without repeatedly collecting logs from different machines to import to OSS.
-   Ease of management Shipping logs to OSS can fully reuse the log grouping in Log Service.  Logs in different projects and Logstores can be automatically shipped to different OSS bucket directories,  which facilitates the OSS data management.

## Prerequisites {#section_l1m_shf_vdb .section}

1.  Activate Log Service, create a project and Logstore, and collect log data.
2.  Activate OSS, create a bucket in the region where the Log Service project resides.
3.  Activate RAM access control.
4.  The Log Service project and OSS bucket must be located in the same region. Cross-region data shipping is not supported.

## Procedure {#section_bbs_h4j_5cb .section}

## Step 1. Resource Access Management \(RAM\) authorization {#section_cf5_p4j_5cb .section}

efore you perform a shipping task, Log Service must be granted a permission to write to OSS .

Go to [RAM quick authorization](https://ram.console.aliyun.com/#/role/authorize?request=%7B%22Requests%22%3A%20%7B%22request1%22%3A%20%7B%22RoleName%22%3A%20%22AliyunLogDefaultRole%22%2C%20%22TemplateId%22%3A%20%22DefaultRole%22%7D%7D%2C%20%22ReturnUrl%22%3A%20%22https%3A//sls.console.aliyun.com/%22%2C%20%22Service%22%3A%20%22Log%22%7D) page, on the displayed page, click **Agree to Authorize**.  After authorization is complete, Log Service has a corresponding write permission to OSS.

**Note:** 

-   For more information about how to modify the authorization policy and configure cross-account shipping task, see OSS Shipper - Advanced RAM authorization.
-   For more information about how to authorize sub-account to perform a shipping task, see [Grant RAM sub-accounts permissions to access Log Service](intl.en-US/User Guide/Access control RAM/Grant RAM sub-accounts permissions to access Log Service.md) to access Log Service.

## Step 2. Configure an OSS shipping rule in Log Service {#section_lcy_54j_5cb .section}

1.  Log on to the Log Service console.
2.  On the Project List page, click the project name.
3.  Select a Logstore, and click **OSS** in the left-side navigation pane.
4.  Click **Enable**, set the OSS LogShipper  configurations, and click Confirm.

    See the following table to complete the OSS shipping configurations.


|Configuration item|Description |Value range |
|:-----------------|:-----------|:-----------|
|OSS Shipping Name|The name of the OSS shipping.|The name can be 3–63 characters long, contain lowercase letters, numbers, hyphens \(-\), and underscores \(\_\), and must begin and end with a lowercase letter or number.|
|OSS Bucket|The name of the OSS bucket.|Must be an existing bucket name, and make sure the OSS bucket is in the same region as the  Log Service project.|
|OSS Prefix|The prefix of OSS. Data synchronized from Log Service to OSS is stored in this bucket directory.|Must be an existing OSS prefix.|
|Partition Format|Use %Y, %m, %d, %H, and %M to format the creation time of the LogShipper task to generate the partition string. This defines the directory hierarchy of the object files written to OSS, where a forward slash \(/\) indicates a level of OSS directory.  The following table describes how to define the OSS  target file path by using OSS prefix and partition format.|For more information about formatting, see [Strptime API](http://man7.org/linux/man-pages/man3/strptime.3.html).|
|RAM Role|The Arn and name of the RAM role. The RAM role is used to control the access permissions and is the identity for the OSS bucket  owner to create a role. The ARN of the RAM role can be viewed in the basic information of this role.|For example, `acs:ram::45643:role/aliyunlogdefaultrole`.|
|Shipping Size|Automatically control the interval of creating LogShipper tasks and configure the maximum size of an OSS object \(not compressed\).|The value range is 5–256. The unit is MB.|
|Storage Format|The storage format after log data is shipped to OSS.|Three formats are supported \([JSON storage](intl.en-US/User Guide/Data shipping/Ship logs to OSS/JSON storage.md), [Parquet storage](intl.en-US/User Guide/Data shipping/Ship logs to OSS/Parquet storage.md), and [CSV storage](intl.en-US/User Guide/Data shipping/Ship logs to OSS/CSV storage.md)\).|
|Compression|The compression method of OSS data storage.| -   Do Not Compress: The raw data is not compressed.
-   Compress \(snappy\): Use [snappy](https://google.github.io/snappy/) algorithm to compress data,  reducing the usage of OSS bucket storage space.

 |
|Shipping Time|The time interval between LogShipper tasks.|The default value is 300. The value range is 300–900.  The unit is second.|

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13179/5810_en-US.png "Delivery log")

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13179/5811_en-US.png "Role arn")

**Note:** Log Service concurrently implements data shipping at the backend. Large amounts of data may be processed by multiple shipping threads.  Each shipping thread jointly determines the frequency of task generation based on the size and time. When any condition is met, the shipping thread creates the task.

## Partition format {#section_tmz_ypj_5cb .section}

Each LogShipper task is written into an OSS file, with the path format of `oss:// OSS-BUCKET/OSS-PREFIX/PARTITION-FROMAT_RANDOM-ID`。 Use the LogShipper task created at 2017-01-20  19:50:43 as an example to describe how to use the partition format.

|OSS Bucket|OSS Prefix|Partition format|OSS file path|
|:---------|:---------|:---------------|:------------|
|test-bucket|test-table|%Y/%m/%d/%H/%M|`oss://test-bucket/test-table/2017/01/20/19/50/43_1484913043351525351_2850008`|
|test-bucket|log\_ship\_oss\_example|year=%Y/mon=%m/day=%d/log\_%H%M%s|`oss://test-bucket/log_ship_oss_example/year=2017/mon=01/day=20/log_195043_1484913043351525351_2850008.parquet`|
|test-bucket|log\_ship\_oss\_example|ds=%Y%m%d/%H|`oss://test-bucket/log_ship_oss_example/ds=20170120/19_1484913043351525351_2850008.snappy`|
|test-bucket|log\_ship\_oss\_example|%Y%m%d/|`oss://test-bucket/log_ship_oss_example/20170120/_1484913043351525351_2850008`|
|test-bucket|log\_ship\_oss\_example|%Y%m%d%H|`oss://test-bucket/log_ship_oss_example/2017012019_1484913043351525351_2850008`|

Analyze the OSS data by using big data platforms such as Hive and MaxCompute. To use the partition data, set each level of directory to key=value format \(Hive-style  partition\).

For example, oss://test-bucket/log\_ship\_oss\_example/year=2017/mon=01/day=20/log\_195043\_1484913043351525351\_2850008.

parquet can be set to three levels of partition columns: year, month, and day.

## LogShipper tasks management {#section_bxt_fqj_5cb .section}

After the LogShipper function is enabled, Log Service regularly starts the LogShipper tasks in the backend. You can view the status of the LogShipper tasks in the console. With LogShipper tasks management, you can:

View all the LogShipper tasks 

-   in the last two days and check their status. The status of a LogShipper task can be Success, Failed, and Running.  The status Failed indicates that the LogShipper task has encountered an error because of external reasons and cannot be retried. In this case, you must manually solve the problem.
-   For the failed LogShipper tasks created within two days, you can view the external reasons that cause the failure in the task list.  After fixing the external errors, you can retry all the failed tasks separately or in batches.

**Procedure**

1.  Log on to the Log Service console.
2.  On the Project List page, click the project name.
3.  Select a Logstore, and click **OSS** in the left-side navigation pane.

    You can view the information such as task start time, task end time, time when logs are received, data lines, and task status.

    If the LogShipper task fails, a corresponding error message is displayed in the console. The system retries the task based on the policy by default. You can also manually retry the task.


**Retry a task**

Generally, log data is synchronized to OSS within 30 minutes after being written to the Logstore.

By default, Log Service retries the tasks in the last two days based on the annealing policy. The minimum interval for retry is 15 minutes.  A task that has failed once can be retried in 15 minutes, a task that has failed twice can be retried in 30 minutes  \(2 x 15 minutes\), and a task that has failed three times can be retried in 60 minutes \(2 x 30 minutes\).

To immediately retry a failed task, click Retry All Failed Tasks in the console or specify a task and retry it by using APIs/SDKs.

**Failed tasks errors**

See the following common errors that cause the task failure.

|Error Message|Error cause|Handling method|
|:------------|:----------|:--------------|
|UnAuthorized|No permission.|Make sure that: - The OSS user has created a role. - The account ID in the role description is correct. - The role has been granted the permissions of writing OSS  buckets. - The role-arn is correctly configured.|
|ConfigNotExist|The configuration does not exist.|This error is generally caused by the deletion of a shipping rule. Retry the task after reconfiguring the shipping rule.|
|InvalidOssBucket|The OSS bucket does not exist.|Make sure that: The OSS bucket is in the same region as the Log Service project. The bucket  name is correctly configured|
|InternalServerError| The internal error of Log Service.|Retry the task.|

## OSS data storage {#section_ksx_rqj_5cb .section}

You can access the OSS data in the console or by using  APIs/SDKs.

To access OSS data in the console, log on to the OSS console, click  **a bucket name** in the left-side navigation pane.  For more information about OSS, see OSS documentation.

For more information about OSS, see OSS documentation.

**Object Address**

```
oss:// OSS-BUCKET/OSS-PREFIX/PARTITION-FROMAT_RANDOM-ID
```

-   Descriptions of path fields
    -   OSS-BUCKET and OSS-PREFIX indicate the OSS bucket name and directory prefix respectively, and are configured by the user. INCREMENTID  is a random number added by the system.
    -   PARTITION-FORMAT is defined as %Y/%m/%d/%H/%M, where %Y, %m, %d, %H, and %M indicate year, month, day, hour, and minute respectively. They are obtained by using strptime API to calculate the created time of the LogShipper task in Log Service.
    -   RANDOM-ID is the unique identifier of a LogShipper task.
-   Directory time

    The OSS data directory is configured according to the created time of LogShipper tasks. Assume that the data is shipped to OSS every five minutes. The LogShipper task created at 2016-06-23  00:00:00 ships the data that is written to Log Service after 2016-06-22 23:55. To analyze the complete logs of the full day of 2016-06-22,  in addition to all objects in the `2016/06/23/00/` directory, you must check  whether the objects in the first 10 minutes in the 2016/06/23/00/directory contain the log of 2016-06-22.


**Object storage format**

-   JSON

    For more information, see[JSON storage](intl.en-US/User Guide/Data shipping/Ship logs to OSS/JSON storage.md).

-   Parquet

    For more information, see[Parquet storage](intl.en-US/User Guide/Data shipping/Ship logs to OSS/Parquet storage.md).

-   CSV

    For more information, see[CSV storage](intl.en-US/User Guide/Data shipping/Ship logs to OSS/CSV storage.md).


