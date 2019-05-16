# Download logs {#task_qf4_cf4_fhb .task}

This topic describes how to download a specific page of logs in CSV format, or all logs in TXT format, to a local host.

1.  Log on to the [Log Service console](https://partners-intl.console.aliyun.com/#/sls), and then click the target project name. 
2.  In the **LogSearch** column, click **Search**.
3.  On the **Raw Logs** tab page, click the icon ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/150102/155799155541716_en-US.png).
4.  Select the method to download logs. 
    -   Click **Download Log in Current Page** to download logs of the current page in the CSV format to your local host.
    -   Click **Download all logs in the CLI console** to download all logs by using the CLI tool.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/150102/155799155541721_en-US.png)

        1.  Install the command line tool. For more information, see [Log Service CLI User Guide](https://aliyun-log-cli.readthedocs.io/en/latest/README.html?spm=5176.2020520112.0.0.18f114da9o209y#installation).
        2.  Select [Security information management](https://usercenter.console.aliyun.com/?spm=5176.10560872.0.0.19b234c002pySx#/manage/ak) to copy your AccessID and AccessKey.
        3.  Select **Copy Command**, and use the AccessID and AccessKey copied in the preceding step to replace `【AccessID obtained in step 2】` and `【AccessKey obtained in step 2】` in the command.
        4.  Run the command in the CLI tool to download logs.

            **Note:** The logs are downloaded to the download\_data.txt file. This file is located in the directory where the command was run.


