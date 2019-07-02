# Delimiter logs {#reference_p3p_h55_vdb .reference}

## Log introduction {#section_xbb_yyb_ry .section}

Delimiter logs use line breaks as the boundary. Each line is a log. The fields of each log are delimited by a fixed delimiter. A delimiter can contain a single character or multiple characters, including the tab \(\\t\), space, vertical bar \(|\), comma \(,\), and semicolon \(;\). Fields that contain the delimiter must be enclosed in a quote, which is double quotation marks \(" "\).

Comma-separated values \(CSV\) logs and tab-separated values \(TSV\) logs are common delimiter logs.

Log Service supports a delimiter in **single-character** or **multi-character** mode to delimit fields in each delimiter log.

## Single-character mode {#section_pzr_523_dbb .section}

In single-character mode, you must specify a delimiter. You can also specify a quote as required.

-   **Delimiter**: The fields of each log are delimited by a single-character delimiter, such as the tab \(`\t`\), the vertical bar \(`|`\), the space, the comma \(`,`\), the semicolon \(`;`\), or a non-printable character.

    **Note:** The double quotation mark \(`"`\) cannot be used as a delimiter.

    If a double quotation mark \("\) is included in a log but not used as a quote, it must be escaped and processed as double quotation marks \(`""`\). Log Service automatically restores double quotation marks \(`""`\) to a double quotation mark \(`"`\) when parsing fields. You can use a double quotation mark \(`"`\) on each border of a field as a quote, or use double quotation marks \(`""`\) in the content of a field. If the use of the double quotation mark \("\) does not comply with the format definitions of delimiter logs, you can consider using other methods such as the simple mode or full regex mode to parse fields.

    For example, when the comma \(,\) is used as the delimiter and a log field contains the double quotation mark \("\) and comma \(,\), this field must be enclosed in the quote and the double quotation mark \("\) in the content of this field must be escaped into double quotation marks \(`""`\). The log format after processing is `1999,Chevy,"Venture ""Extended Edition, Very Large""","",5000.00`. The log can be parsed into five fields as follows: `1999`, `Chevy`, `Venture "Extended Edition, Very Large"`, null field, and `5000.00`.

-   **Quote**: When log fields contain the delimiter, you must specify a quote to enclose such fields and ensure that they can be parsed properly. Log Service parses the content enclosed in the quote as a complete field. Only the delimiter can exist between fields.

    **Note:** If any characters other than the delimiter, such as the space or tab \(\\t\), exist between fields, you need to modify the log format.

    The quote can contain a single character, such as the tab \(`\t`\), the vertical bar \(`|`\), the space, the comma \(`,`\), the semicolon \(`;`\), or a non-printable character.

    For example, when the comma \(`,`\) is used as the delimiter and double quotation marks \(" "\) are used as the quote, the log format is `1997,Ford,E350,"ac, abs, moon",3000.00`. The log can be parsed into five fields as follows: `1997`, `Ford`, `E350`, `ac, abs, moon`, and `3000.00`. Among the five fields, `ac, abs, moon` enclosed in the quote is regarded as a complete field.


**Note:** Log Service allows you to use a non-printable character as a delimiter or quote. Non-printable characters are those whose decimal ASCII codes are in the range of 1 to 31 and 127. If you use a non-printable character as the delimiter or quote, you need to find the hexadecimal ASCII code of this character and enter this character in the following format: `0xthe hexadecimal ASCII code of the non-printable character`. For example, to use the non-printable character whose decimal ASCII code is 1 and hexadecimal ASCII code is 01, you need to enter `0x01`.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13046/156205024321597_en-US.png)

## Multi-character mode {#section_q22_s1z_pfb .section}

In **multi-character mode**, a delimiter can contain two or three characters, such as `||`, `&&&`, or `^_^`. In this mode, Log Service parses logs based on the delimiter only. You do not need to use a quote to enclose log fields.

**Note:** You need to ensure that log fields do not contain the delimiter, otherwise Log Service may incorrectly parse these fields.

For example, if the delimiter is set to `&&`, the log `1997&&Ford&&E350&&ac&abs&moon&&3000.00` can be parsed into five fields as follows: `1997`, `Ford`, `E350`, `ac&abs&moon`, and `3000.00`.

## Sample logs {#section_owv_zyb_ry .section}

-   **Logs whose fields are delimited by a single-character delimiter** 

    ``` {#codeblock_3s1_rek_8z3}
    05/May/2016:13:30:28,10.10. *. *,"POST /PutData? Category=YunOsAccountOpLog&AccessKeyId=****************&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=******************************** HTTP/1.1",200,18204,aliyun-sdk-java
    05/May/2016:13:31:23,10.10. *. *,"POST /PutData? Category=YunOsAccountOpLog&AccessKeyId=****************&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=******************************** HTTP/1.1",401,23472,aliyun-sdk-java
    ```

-   **Logs whose fields are delimited by a multi-character delimiter** 

    ``` {#codeblock_htb_bmc_v92}
    05/May/2016:13:30:28&&10.200. **. **&&POST /PutData? Category=YunOsAccountOpLog&AccessKeyId=****************&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=pD12XYLmGxKQ%2Bmkd6x7hAgQ7b1c%3D HTTP/1.1&&200&&18204&&aliyun-sdk-java
    05/May/2016:13:31:23&&10.200. **. **&&POST /PutData? Category=YunOsAccountOpLog&AccessKeyId=****************&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=******************************** HTTP/1.1&&401&&23472&&aliyun-sdk-java
    ```


## Configure Logtail to collect delimiter logs {#section_hth_dzb_ry .section}

For more information about how to configure Logtail to collect delimiter logs, see [Collect text logs](reseller.en-US/User Guide/Logtail collection/Text logs/Collect text logs.md). You can select the corresponding configuration based on your network deployment and actual situation.

1.  On the Logstores page of the target project in the Log Service console, click the **Data Import Wizard** icon of the target Logstore.
2.  Select a data source.

    Select **Text File** and go to the next step****.

3.  Configure the data source.
    1.  Enter the configuration name and log path. Then, select **Delimiter Mode** as the log collection mode.
    2.  Enter the log sample and select the delimiter and quote.

        Select the appropriate delimiter and quote based on the log format. Otherwise, Log Service may fail to parse logs.

        **Note:** If you use a non-printable character as the delimiter or quote, you need to find the hexadecimal ASCII code of this character and enter this character in the following format: `0xthe hexadecimal ASCII code of the non-printable character`. For example, to use the non-printable character whose decimal ASCII code is 1 and hexadecimal ASCII code is 01, you need to enter `0x01`.

         ![](images/2631_en-US.png "Configure the data source")

    3.  Specify the keys in the log extraction results.

        After you enter a log sample and select a delimiter, Log Service extracts log fields according to your selected delimiter, and defines them as values. You must specify the key for each value.

        In the preceding log sample, the non-printable character `0x01` is used as the delimiter and `0x02` is used as the quote. The log is parsed into six fields. Enter time, ip, url, status, latency, and user-agent as the keys of six fields, respectively.

    4.  Determine whether to upload a log with some fields missing.

        Configure whether to upload a log whose number of parsed fields is less than the number of configured keys. If you enable Incomplete Entry Upload, the log is uploaded. If you disable Incomplete Entry Upload, the log is discarded.

        For example, if you set the delimiter to the vertical bar \(`|`\), the log sample `11|22|33|44|55` can be parsed into the following fields: `11`, `22`, `33`, `44`, and `55`. You can set their keys to `A`, `B`, `C`, `D`, and `E`, respectively. If you enable **Incomplete Entry Upload** and Log Service collects the log `11|22|33|55`, the `55` field is uploaded as the value of the `D` key. If you disable **Incomplete Entry Upload**, Log Service discards the log because the fields and keys do not match.

    5.  Specify the log time.

        You can use the system time or a log field \(for example, the time field 05/May/2016:13:30:29\) as the log time. For more information about how to configure the time format, see [Text logs - Configure time format](reseller.en-US/User Guide/Logtail collection/Text logs/Text logs - Configure time format.md).

        ![](images/2632_en-US.png "Specify the log time")

    6.  Preview logs in the console to confirm whether logs are collected.

