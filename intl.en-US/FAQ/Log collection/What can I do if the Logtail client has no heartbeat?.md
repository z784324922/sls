# What can I do if the Logtail client has no heartbeat? {#concept_nfs_hs3_bfb .concept}

If the heartbeat status of the Logtail machine group is abnormal when you use Logtail to collect logs, you can troubleshoot the error manually or by using the Logtail automatic diagnostic tool.

After you install Logtail on your server to collect logs, the Logtail client regularly sends heartbeat packets to the server. If the machine group status shows that the Logtail client has no heartbeat, the Logtail client is disconnected from the server. In this case, you can troubleshoot the error manually or by using the [Logtail automatic diagnostic tool](reseller.en-US/FAQ/Log collection/How do I use the Logtail automatic diagnostic tool?.md) as required.

-   **Automatic diagnosis**: Log Service provides the Logtail automatic diagnostic tool for Linux. For more information, see [How do I use the Logtail automatic diagnostic tool?](reseller.en-US/FAQ/Log collection/How do I use the Logtail automatic diagnostic tool?.md)

-   **Manual diagnosis**: If the Logtail diagnostic tool fails to troubleshoot the error or your server is running in Windows, troubleshoot the error by performing the following steps.


![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13294/156808150511589_en-US.png)

## 1. Check whether Logtail is installed {#section_ntl_4tj_bfb .section}

Run the following command to check whether Logtail is installed. If you do not install Logtail, see [Install Logtail in Linux](../reseller.en-US/Data Collection/Logtail collection/Install/Install Logtail in Linux.md) or [Install Logtail in Windows](../reseller.en-US/Data Collection/Logtail collection/Install/Install Logtail in Windows.md) to install it. You must install Logtail based on the region of your Log Service project and the network type.

Check the Logtail installation status:

-   Linux:

    ``` {#codeblock_5ro_c02_ug9}
    sudo /etc/init.d/ilogtaild status 
    ```

    If `ilogtail is running` is returned, Logtail is installed, as shown in the following example:

    ``` {#codeblock_fot_e21_a5t}
    [root@****************~]# sudo /etc/init.d/ilogtaild status 
    ilogtail is running
    ```

-   Windows:
    1.  On Control Panel, choose System and Security \> **Administrative Tools**, and then double-click **Services**.
    2.  Check the running status of LogtailDaemon and LogtailWorker. If they are running normally, Logtail is installed.

If Logtail is running, go to the next step.

## 2. Check whether the Logtail installation parameter is correct {#section_cgq_f5j_bfb .section}

Before installing Logtail, you must to specify the correct network endpoint. That is, you must select a correct Logtail installation parameter from [Table 1](../reseller.en-US/Data Collection/Logtail collection/Install/Install Logtail in Linux.md#table_eyz_pmv_vdb) based on the region of the Log Service project and decide how to install Logtail based on the [network type](../reseller.en-US/Data Collection/Logtail collection/Select a network type.md). If the installation script or parameter is incorrect, the Logtail client may have no heartbeat.

The Logtail configuration file ilogtail\_config.json records the Logtail installation parameter and the installation method that you used. This file is stored in:

-   Linux: /usr/local/ilogtail/ilogtail\_config.json
-   Windows x64: C:\\Program Files \(x86\)\\Alibaba\\Logtail\\ilogtail\_config.json
-   Windows x32: C:\\Program Files\\Alibaba\\Logtail\\ilogtail\_config.json

1.  Check the installation parameter.

    Check whether the region of the network endpoint recorded in the ilogtail\_config.json file is the same as that of your Log Service project.

    For example, the returned information in the following figure indicates that Logtail is installed on an ECS instance in China \(Hangzhou\).

    ![](images/21881_en-US.png "Check the installation parameter")

2.  Check the installation method.

    Telnet the domain name configured in the ilogtail\_config.json file to check whether Logtail is properly installed based on the network type of the server.

    For example, the domain name recorded in the ilogtail\_config.json file is `cn-hangzhou-intranet`. You can run the `telnet logtail.cn-hangzhou-intranet.log.aliyuncs.com 80` command to check the network connectivity. If Logtail is connected to the server, Logtail is properly installed.

    For example, run the following command to check whether Logtail is connected to an ECS instance that is running in Linux:

    ``` {#codeblock_m7a_on5_22d}
    [root@*********** ~]# telnet logtail.cn-hangzhou-intranet.log.aliyuncs.com 80
    Trying 100*0*7*5...
    Connected to logtail.cn-hangzhou-intranet.log.aliyuncs.com.
    Escape character is '^]'. 
    ```

    If the telnet command fails, the Logtail installation parameter is incorrect, so that the installation command is incorrect. For more information about how to select a correct installation parameter, see [Install Logtail in Linux](../reseller.en-US/Data Collection/Logtail collection/Install/Install Logtail in Linux.md) or [Install Logtail in Windows](../reseller.en-US/Data Collection/Logtail collection/Install/Install Logtail in Windows.md).


If Logtail is properly installed, go to the next step.

## 3. Check whether the IP address configuration of the machine group is correct {#section_nxh_bxj_bfb .section}

The server IP address obtained by the Logtail client must be configured in the machine group. Otherwise, the Logtail client has no heartbeat in the machine group or Logtail cannot collect logs.

The Logtail client obtains the server IP address as follows:

-   If no hostname is bound, the Logtail client obtains the IP address of the first NIC of the server.

-   If a hostname is bound in the /etc/hosts file, the Logtail client obtains the bound IP address. You can run the hostname command to view the hostname.


**Troubleshooting procedure**

1.  Check the server IP address obtained by the Logtail client.

    The `ip` field in the app\_info.json file indicates the server IP address obtained by the Logtail client. This file is stored in:

    -   Linux: /usr/local/ilogtail/app\_info.json
    -   Windows x64: C:\\Program Files \(x86\)\\Alibaba\\Logtail\\app\_info.json
    -   Windows x32: C:\\Program Files\\Alibaba\\Logtail\\app\_info.json
    **Note:** 

    -   Logtail cannot work if the `ip` field in the `app_info.json` file is empty. In this case, you must to configure an IP address for the server and restart Logtail.
    -   The `app_info.json` file only records information. Any modification to this file does not change the server IP address obtained by the Logtail client.
    ![](images/11585_en-US.png "Check the server IP address obtained by the Logtail client")

2.  Check the IP address configuration of the machine group.

    Log on to the Log Service console, click the target project, and then click Logtail Machine Group in the left-side navigation pane. On the Machine Groups page, click **Status** in the Actions column of the target machine group.

    ![](images/11586_en-US.png "Check the machine group status")

    If the server IP addresses configured in the machine group do not include the server IP address obtained by the Logtail client, you must to modify the IP address configuration.

    -   If an incorrect server IP address is configured in the machine group, modify the IP address and save it. Then, check the heartbeat status again in 1 minute.

    -   If you have modified the network configuration of the server where Logtail is installed, for example, /etc/hosts, you must restart Logtail to obtain the new server IP address. In addition, you must modify the server IP address configured in the machine group based on the `ip` field in the `app_info.json` file.


**Logtail restart methods** 

-   Linux:

    ``` {#codeblock_4pk_zfi_99g}
    sudo /etc/init.d/ilogtaild stop
    sudo /etc/init.d/ilogtaild start
    ```

-   Windows: On Control Panel, choose **System and Security** \> **Administrative Tools**, and then double-click Services. Find LogtailWorker and restart it.

If the server IP address obtained by the Logtail client is configured in the machine group, go to the next step.

## 4. Check whether an AliUid is configured for the ECS instance under another Alibaba Cloud account {#section_ww2_yzj_bfb .section}

If your ECS instance and Log Service project belong to different Alibaba Cloud accounts, or you use a server deployed in an on-premises IDC or provided by another cloud product vendor, you must [configure an AliUid](../reseller.en-US/Data Collection/Logtail collection/Machine Group/Configure AliUids for ECS servers under other Alibaba Cloud accounts or on-premises IDCs.md) to authorize the server where Logtail is installed.

Check whether a file named after your AliUid exists in the /etc/ilogtail/users directory.

If such a file does not exist, configure an AliUid. For more information, see [Configure AliUids for ECS servers under other Alibaba Cloud accounts or on-premises IDCs](../reseller.en-US/Data Collection/Logtail collection/Machine Group/Configure AliUids for ECS servers under other Alibaba Cloud accounts or on-premises IDCs.md).

**Note:** 

-   The AliUid must be an Alibaba Cloud account ID.

-   You can view your Alibaba Cloud account ID in the Alibaba Cloud console as follows: Move the point over your avatar and choose User Info \> Security Settings.

    ![](../DNSLS11850791/images/5286_en-US.png "View your Alibaba Cloud account ID")


If the problem persists, [submit a ticket](https://selfservice.console.aliyun.com/ticket/category/sls/today) to Log Service engineers. Along with the ticket, provide the information about your project, Logstore, and machine group, the app\_info.json and ilogtail\_config.json files, and the output of the Logtail automatic diagnostic tool.

