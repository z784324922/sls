# Delimiter logs {#reference_p3p_h55_vdb .reference}

## Log introduction {#section_xbb_yyb_ry .section}

Delimiter logs use line breaks as the boundary. Each line is a log.  The fields of each log are connected by fixed delimiters, including tabs, spaces, vertical lines \(|\), commas \(,\), semicolons \(;\), and other single characters.  Fields containing delimiters are enclosed in double quotation marks \(“\). 

Comma-separated values \(CSV\) logs and tab-separated values \(TSV\) logs are common delimiter logs. 

## Log format  {#section_pzr_523_dbb .section}

A delimiter log is divided into several fields by delimiters, and supports two modes: **single character** and **multiple characters**.

-   **Single character mode **

    Single character mode divides logs by matching single characters, such as tabs \(`\t`\), spaces, vertical lines \(`|`\), commas \(`,`\), and semicolons \(`;`\). 

    **Note:** The double quotation mark \(`"`\) cannot be a delimiter, but is used as the quote of the default single character delimiter.

    Single character delimiters are often contained in log fields. To prevent log fields from being divided incorrectly, a double quotation mark \(`"`\) is used as the quote to isolate the log field. If double quotation marks \(“”\) are found in the log content but not used as the quote, they must be escaped and processed as  `""`.  You can either use a double quotation mark \(`"`\) in field border as the quote, or use double quotation marks \(`""`\) as field data. For other situations, use other modes, such as simple mode and full mode, to parse fields because the other conditions do not meet the format definition of delimiter logs. 

    -   **Double quotation mark \(“\) used as the quote**

        When the double quotation mark \(`"`\) is used as the quote, fields containing delimiters must be enclosed in a pair of quotes.   The quote must be located adjacent to the delimiter. Modify the format if any spaces, tabs, and other characters exist between them. 

        For example, comma \(`,`\) is the delimiter and double quotation mark \(“\) is the quote. The log format is `1997,Ford,E350,"ac,  abs, moon",3000.00` .  This log can be parsed into five fields,  `1997` , `Ford` , `E350` , `ac, abs,  moon` , and `3000.00`,  which is enclosed in quotes, is considered as a complete field.  `ac, abs,  moon`, which is enclosed in quotes, is considered as a complete field. 

    -   **Double quotation marks \(“”\) used as a part of log field**

        When double quotation marks \(“”\) are used as a part of the log field instead of the quote, they must be escaped and processed as `""`.  Restore it when parsing fields, that is, restoring `""` to `"`. 

        For example, a comma acts as a separator, double quotes, and comma as part of a log field, you must wrap the log field containing the comma in the quote, the double quotation marks in the log field are also escaped as the correct double quotation marks `""`. The log format after the processing is `1999,Chevy,"Venture ""Extended Edition, Very Large""","",5000.00`.  This log can be parsed into five fields,   `1999`、`Chevy`、`Venture "Extended Edition,  Very Large"`, a blank field, and `5000.00`. 

-   **Multiple character mode**

    In multiple character mode, a delimiter can contain two or three characters, such as `||`, `&&&`, `^_^`.  In this mode, logs are parsed completely by matching delimiters and you do not need to use the quote to enclose the logs.

    **Note:** Make sure that the full match of the delimiter does not appear in the log field. Otherwise, the field will be divided incorrectly.

    For example, if `&&` is the delimiter, the log `1997&&Ford&&E350&&ac&abs&moon&&3000.00` can be parsed into 5 fields, `1997`, `Ford`, `E350`, `ac&abs&moon`, and `3000.00`.


## Log sample {#section_owv_zyb_ry .section}

-   **Single character delimiter logs**

    ```
    05/May/2016:13:30:28,10.10.*.*,"POST /PutData?Category=YunOsAccountOpLog&AccessKeyId=****************&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=******************************** HTTP/1.1",200,18204,aliyun-sdk-java
    05/May/2016:13:31:23,10.10.*.*,"POST /PutData?Category=YunOsAccountOpLog&AccessKeyId=****************&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=******************************** HTTP/1.1",401,23472,aliyun-sdk-java
    ```

-   **Multiple character delimiter logs**

    ```
    05/May/2016:13:30:28&&10.200.**.**&&POST /PutData?Category=YunOsAccountOpLog&AccessKeyId=****************&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=pD12XYLmGxKQ%2Bmkd6x7hAgQ7b1c%3D HTTP/1.1&&200&&18204&&aliyun-sdk-java
    05/May/2016:13:31:23&&10.200.**.**&&POST /PutData?Category=YunOsAccountOpLog&AccessKeyId=****************&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=******************************** HTTP/1.1&&401&&23472&&aliyun-sdk-java
    ```


## Configure Logtail to collect delimiter logs {#section_hth_dzb_ry .section}

For the complete process of collecting logs by using Logtail, see [Python logs](reseller.en-US/User Guide/Other collection methods/Common log formats/Python logs.md). Select the corresponding configuration based on your network deployment and actual situation. 

1.  On the Logstore List page, click the **Data Import Wizard**.
2.  Select the data source. 

    Select the **text file** and click **Next**. 

3.  Configure the data source.
    1.  Enter the Configuration Name and Log Path. Then, select **Delimiter Mode** as the log collection mode. 
    2.  Enter the log sample and select the delimiter. 

        Select the correct delimiter based on your log format. Otherwise, the log data will fail to be parsed. 

        ![](images/2631_en-US.png "Select the data source. ")

    3.  Specify the key in the log extraction results. 

        After you enter the log sample and select the delimiter, Log Service extracts log fields according to your selected delimiter, and defines them as Value. You must specify the corresponding Key for the Value. 

        For the preceding log sample, use a comma \(`,`\) as the delimiter, and six fields are in the log. Set the keys as time, ip, url, status, latency, and user-agent. 

    4.  Specify the log time. 

        You can select to use the system time or a log field \(such as the time field,  05/May/2016:13:30:29\) as the log time. For how to configure the date format, see [Text logs - Configure time format](reseller.en-US/User Guide/Logtail collection/Text logs/Text logs - Configure time format.md). 

        ![](images/2632_en-US.png "Specify log time ")

    5.  Preview logs in the console, and confirm whether logs are successfully collected.

        ![](images/2633_en-US.png "Previewing logs")


