# JSON logs {#reference_dsq_3v5_vdb .reference}

JSON logs are constructed in two structures:

-   Object: A collection of key/value pairs.
-   Array: An ordered list of values.

Logtail supports JSON logs of the object type. Logtail automatically extracts the keys and values from the first layer of an object as the names and values of fields respectively.  The field value can be the object, array, or basic type, for example, a string or number.  `\n` is used to separate the lines of JSON logs. Each line is extracted as a single log.

Logtail does not support automatic parsing of non-object data \(for example, JSON arrays\). You can use regular expressions for field extraction or use the simple mode for log collection by line.

## Log sample {#section_ets_2bc_ry .section}

```
{"url": "POST /PutData? Category=YunOsAccountOpLog&AccessKeyId=U0Ujpek********&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=pD12XYLmGxKQ%2Bmkd6x7hAgQ7b1c%3D HTTP/1.1", "ip": "10.200.98.220", "user-agent": "aliyun-sdk-java", "request": {"status": "200", "latency": "18204"}, "time": "05/May/2016:13:30:28"}
{"url": "POST /PutData? Category=YunOsAccountOpLog&AccessKeyId=U0Ujpek********&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=pD12XYLmGxKQ%2Bmkd6x7hAgQ7b1c%3D HTTP/1.1", "ip": "10.200.98.210", "user-agent": "aliyun-sdk-java", "request": {"status": "200", "latency": "10204"}, "time": "05/May/2016:13:30:29"}
```

## Configure Logtail to collect JSON logs {#section_dpf_3bc_ry .section}

For the complete process of collecting JSON logs by using Logtail, see [5-minute quick start](../../../../intl.en-US/Quick Start/5-minute quick start.md).  This document shows the detailed configuration  **Log Collection Mode** of Logtail.

1.  On the Logstore List, click the **Data Import Wizard**.
2.  Select the data type.

    Select the **text file** and click **Next**. 

3.  Configure the data source.
    1.  Enter the configuration name, Log Path, and select log collection mode as **JSON mode**.
    2.  Select whether to use the system time as the log time according to your requirements. You can enable or disable the **Use System Time function**.
        -   Enable **Use System Time function**

            Enabling this function means to use the time when Log Service collects the log as the log time, instead of extracting the time fields in the log.

        -   Disable **Use System Time function**

            Disabling this function means to extract the time fields from the log as the log time.

            If you select to disable the  **Use System Time** function, you must define the key of the extracted time field, and the time conversion format.  For example,  the `time` field \(05/May/2016:13:30:29\) in  JSON Object can be extracted as log time.  For how to configure the date format, see [Text logs - Configure time format](intl.en-US/User Guide/Logtail collection/Data Source/Text logs - Configure time format.md).

             ![](images/2638_en-US.png "JSON logs") 


