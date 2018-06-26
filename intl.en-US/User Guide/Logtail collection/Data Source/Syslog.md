# Syslog {#concept_czq_zrc_wdb .concept}

By using Logtail, you can configure TCP ports locally to receive syslog data forwarded by syslog agents by means of the TCP protocol. Logtail parses the received data and forwards it to LogHub.

## Prerequisites {#section_mtm_5sc_wdb .section}

You must install Logtail before using it to collect logs.  Logtail supports Windows and Linux operating systems. For the installation methods, see [EN-US\_TP\_13056.md](intl.en-US/User Guide/Logtail collection/Install/Linux .md) and [EN-US\_TP\_13057.md](intl.en-US/User Guide/Logtail collection/Install/Install Logtail on Windows.md).

## Step 1. Create a Logtail syslog configuration in the Log Service console {#section_rbk_wdw_tcb .section}

1.  Log on to the Log Service console, and click the target project to enter the Logstore List.
2.  On the Logstore List page, click the **Data Import Wizard** icon at the right of the Logstore.
3.  Select the data source type.

    Select **syslog** in **Other Sources**and click **Next**. 

4.    Specify the  Configuration Name.

    The configuration name can be 3–63 characters long, contain lowercase letters, numbers, hyphens \(-\), and underscores \(\_\), and must begin and end with a  lowercase letter or number.

    **Note:** The configuration name cannot be modified after the configuration is created.

5.  Specify **Tag Settings**. 

    For more information, see [EN-US\_TP\_13068.md](intl.en-US/User Guide/Logtail collection/Data Source/Syslog.md).

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13063/2869_en-US.png " Set tag")

6.  Set **Advanced Options** as needed. 

    Select whether to enable **Local Cache**.  When the Log Service is unavailable, logs can be cached in the local directory of the machine, and continue to be sent to Log Service after the service recovery. By default, this function is enabled, and at most 1 GB logs can be cached.

7.  Select the machine group and click

     **Apply to Machine Group** to apply the configuration to the selected  machine group.

    If you have not created a machine group, you must create one first.  For how to create a machine group, see [EN-US\_TP\_13080.md](intl.en-US/User Guide/Logtail collection/Machine Group/Create a machine group.md).

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13066/2904_en-US.png "Applied to the Machine group")


## Step 2. Configure Logtail to bring the protocol into effect {#section_wtx_xdw_tcb .section}

Find the `ilogtail_config.json` file  in the Logtail installation directory on the machine. Generally, it is in the `/usr/local/ilogtail/` directory. Modify the syslog configuration as needed.

1.  Confirm that the syslog function is enabled.

    true indicates the function is enabled. false indicates the function is disabled.

    ```
    "streamlog_open" : true
    ```

2.  Specify the size of the memory pool that syslog uses to receive logs  Logtail requests a specified size of memory at one time when started. Configure the size \(in MB\) according to the memory size of your machine and your actual requirements.

    ```
    "streamlog_pool_size_in_mb" : 50
    ```

3.  Specify the buffer size \(in bytes\).  You must specify the size of the buffer that Logtail uses  when calling the socket io rcv interface. 

    ```
    "streamlog_rcv_size_each_call" : 1024
    ```

4.  Specify the syslog log format.

    ```
    "streamlog_formats":[]
    ```

5.  Specify the TCP binding address and port.  You must specify the TCP binding address and port that Logtail uses to receive syslog data. By default, the binding address is 0.0.0.0 and the binding port is 11111.

    ```
    "streamlog_tcp_addr" : "0.0.0.0",
    "streamlog_tcp_port" : 11111
    
    ```

6.  After the configuration is complete, restart Logtail. To restart Logtail, run the following commands to stop the Logtail client and then start it again.

    ```
    sudo /etc/init.d/ilogtaild stop
    sudo /etc/init.d/ilogtaild start
    
    ```


## Step 3. Install rsyslog and modify its configurations {#section_o2w_l2w_tcb .section}

Skip this step if you have installed rsyslog on the machine.

1.  Install rsyslog. 

    For the installation method, see:

    -   [Ubuntu installation method](http://www.rsyslog.com/ubuntu-repository/)
    -   [Debian installation method](http://www.rsyslog.com/debian-repository/)
    -   [RHEL/CENTOS installation method](http://www.rsyslog.com/rhelcentos-rpms/)
2.  Modify configurations. 

    In `/etc/rsyslog.conf` , modify the configurations as needed, for example:

    ```
    $WorkDirectory /var/spool/rsyslog # where to place spool files
    $ActionQueueFileName fwdRule1 # unique name prefix for spool files
    $ActionQueueMaxDiskSpace 1g # 1gb space limit (use as much as possible)
    $ActionQueueSaveOnShutdown on # save messages to disk on shutdown
    $ActionQueueType LinkedList # run asynchronously
    $ActionResumeRetryCount -1 # infinite retries if host is down
    # Defines the fields of log data
    $template ALI_LOG_FMT,"0.1 sys_tag %timegenerated:::date-unixtimestamp% %fromhost-ip% %hostname% %pri-text% %protocol-version% %app-name% %procid% %msgid% %msg:::drop-last-lf%\n"
    *. * @@10.101.166.173:11111;ALI_LOG_FMT
    
    ```

    **Note:** In the template `ALI_LOG_FMT` , the value of the second field  is `sys_tag` . This value must be the same as the one entered in step 1.  This configuration indicates that all the \( \\\*.\\\* \) syslog data received`\*.\*` by this machine is formatted according to the  `ALI_LOG_FMT` template, and forwarded to 10.101.166.173:11111 by using the TCP protocol. The  machine  10.101.166.173 must be in the machine group selected in step 1 and configured according to step 2.

3.  Start rsyslog.

    ```
    sudo /etc/init.d/rsyslog restart
    ```

    Before starting rsyslog, check whether another syslog agent is installed on the machine, such as  syslogd, sysklogd, or syslog-ng. If yes, stop that syslog agent.


After completing the preceding three steps, you can collect syslog data on the machine to Log Service.

## Further information {#section_nmy_btq_pdb .section}

For more information about syslog collection, and how to format syslog data, see [EN-US\_TP\_13068.md](intl.en-US/User Guide/Logtail collection/Data Source/Syslog.md).

