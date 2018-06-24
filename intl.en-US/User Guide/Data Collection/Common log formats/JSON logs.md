# JSON logs {#reference_dsq_3v5_vdb .reference}

JSON logs are constructed in two structures:

-   Object: A collection of key/value pairs.Object: a collection of name/value pairs \).
-   ray: An ordered list of values.

Logtail supports JSON logs of the object type. Logtail automatically extracts the keys and values from the first layer of an object as the names and values of fields respectively. The field value can be the object, array, or  basic type,  for example, a string or number. \\n is used to separate the lines of JSON logs. Each line is extracted as a single log.

Logtail does not support automatically parsing non-object data such as JSON arrays. Use regular expressions to extract the fields or use the simple mode to collect logs by line.

## Log sample {#section_ets_2bc_ry .section}

```
{"url": "POST /PutData? Category=YunOsAccountOpLog&AccessKeyId=U0Ujpek********&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=pD12XYLmGxKQ%2Bmkd6x7hAgQ7b1c%3D HTTP/1.1", "ip": "10.200.98.220", "user-agent": "aliyun-sdk-java", "request": {"status": "200", "latency": "18204"}, "time": "05/May/2016:13:30:28"}
{"url": "POST /PutData? Category=YunOsAccountOpLog&AccessKeyId=U0Ujpek********&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=pD12XYLmGxKQ%2Bmkd6x7hAgQ7b1c%3D HTTP/1.1", "ip": "10.200.98.210", "user-agent": "aliyun-sdk-java", "request": {"status": "200", "latency": "10204"}, "time": "05/May/2016:13:30:29"}
```

## Configure Logtail to collect JSON logs {#section_dpf_3bc_ry .section}

For the complete process of collecting JSON logs by using Logtail, see [../../../../dita-oss-bucket/SP\_7/DNSLS11878107/EN-US\_TP\_13018.md](../../../../intl.en-US/Quick Start/5-minute quick start.md).  Select the corresponding configuration based on your network deployment and actual situation. This document only shows **how to configure data source** in Step 3 Configure data import wizard in details. Enter the Configuration Name and Log Path. Then, select JSON Mode as the log collection mode.

1.  Click the data access wizard chart in the logstore list interface to enter the data access wizard.
2.  Select the data type.

    Select the **text file** and click **Next**.

3.  Configure the data source.
    1.  Fill in the configuration name, Log Path, and select log collection mode as **JSON mode**.
    2.  Select whether or not to use the system time as the log time according to your requirements. You can enable or disable the **Use System Time function**.
        -   Enable **Use System Time function**

            Enabling this function means to use the time when Log Service collects the log as the log time, instead of extracting the time fields in the log.

        -   Disable **the Use System Time function**

            Disabling this function means to extract the time fields from the log as the log time.

            If you select to disable the  **Use System Time** function, you must define the key of the extracted time field, and the time conversion format.  For example,  the `time` field \(05/May/2016:13:30:29\) in  JSON Object can be extracted as log time.  For how to configure the date format, see Logtail date format.

             ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13047/2638_en-US.png "JSON logs") 


