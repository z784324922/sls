# Overview {#concept_asf_lt1_ygb .concept}

When you access Object Storage Service \(OSS\), the system generates a large number of access logs. After you enable the logging feature for a bucket, OSS automatically generates an object by hour based on the predefined naming rules to store access logs for the bucket. Afterward, OSS writes the object to the specified target bucket.

## Enable OSS log storage {#section_lxp_1w1_ygb .section}

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).
2.  In the list of buckets on the left, click the name of the target bucket to go to the Overview tab page of the bucket.
3.  Click the Basic Settings tab, click **Configure** in the **Log** field, enable Log, and then set **Destination Bucket** and **Log Prefix**.
    -   Destination Bucket: Select the name of the bucket where you want to store logs from the drop-down list. You must select your own bucket that remains in the same data center as your Logstore.
    -   Log prefix: Enter the directory where the log is generated and the prefix of the log. The log is stored in the specified directory.
4.  Click **Save**.

## Log naming rules {#section_j4c_s1b_ygb .section}

The following example shows the naming rules for objects that store access logs:

<TargetPrefix\><SourceBucket\>YYYY-MM-DD-HH-MM-SS-<UniqueString\>

-   <TargetPrefix\>: the prefix that you have specified.
-   <SourceBucket\>: the name of the source bucket.
-   YYYY-MM-DD-HH-MM-SS: the time in China Standard Time \(UTC+8\) when the log is created. YYYY indicates a 4-digit year, MM indicates a 2-digit month, DD indicates a 2-digit day, HH indicates a 2-digit hour, MM indicates a 2-digit minute, and SS indicates a 2-digit second.
-   <UniqueString\>: the string that OSS generates to identify the object.

For example, the name of an object used to store OSS access logs can be:

MyLog-OSS-example2015-09-10-04-00-00-0000

-   MyLog is the log prefix that you have specified.
-   oss-example is the name of the source bucket.
-   2015-09-10-04-00-00 is the time in China Standard Time \(UTC+8\) when the log is created.
-   0000 indicates the string that OSS generates to identify the object.

