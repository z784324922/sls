# Advanced settings {#concept_k2n_z3h_1gb .concept}

The Anti-DDoS Pro log collection function supports advanced management. You can click **Advanced Management** to go to the Logstores page in the Log Service console. You can perform advanced operations on Anti-DDoS Pro logs, including real-time subscription and consumption, data shipping, and other visualization operations.

In the upper-right corner of the Log Service page, click **Advanced Management** to go to the Log Service console. You can perform advanced operations including exporting log data and configuring log consumption.

![](images/33771_en-US.png)

## Export log data {#section_iqz_tdl_qfb .section}

1.  After you enable log collection, click the Download button on the right of the Raw Logs tab page.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/40474/156109717421221_en-US.png)

2.  In the Log Download dialog box that appears, click **Download Log in Current Page** to export the log entries displayed on this page into a file in CSV format.
3.  You can also click **Download All Logs Using Command Line Tool** to download all log entries.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/40474/156109717421222_en-US.png)

    1.  Click Documentation to see [User Guide for Alibaba Cloud CLI](https://aliyun-log-cli.readthedocs.io/en/latest/README_CN.html?spm=5176.10560872.0.0.19b234c002pySx#安装).
    2.  Install the command line tool.
    3.  Click [Security information management](https://usercenter.console.aliyun.com/?spm=5176.10560872.0.0.19b234c002pySx#/manage/ak) to view and copy the AccessKey ID and AccessKey Secret of the current user.
    4.  Click **Copy Command** and replace `AccessKeyId obtained in step 2` and `AccessKeySecret obtained in step 2` with the AccessKey ID and AccessKey Secret of the current user.
    5.  Run the command in the command line tool.
    After you run the command, log entries are automatically downloaded and saved to the **download\_data.txt** file in the directory where the command was run.


## Other advanced operations {#section_py3_ydl_qfb .section}

-   [Alerts and notifications](reseller.en-US/User Guide/Alarm/Alarm function overview.md)
-   [Real-time subscription and consumption](reseller.en-US/User Guide/Real-time subscription and consumption/Overview.md)
-   [Data shipping](reseller.en-US/User Guide/Data shipping/Overview.md)
-   [Integration with other visualization tools](reseller.en-US/User Guide/Query and visualization/Other visualization methods/Interconnection with Grafana.md)

