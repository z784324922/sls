# Use the Syslog protocol to collect data to Log Service {#concept_inz_rkc_ghb .concept}

This topic describes how to use the Syslog protocol to collect data to Log Service without using the collection agent to forward collected data. The following sections contain information about the limits, required configurations, sample logs, and application examples detailing possible uses of the Syslog protocol.

## Limits {#section_gjh_s5d_ghb .section}

-   The Syslog protocol must be the standard [RFC5424](https://tools.ietf.org/html/rfc5424) protocol.
-   Each log can be up to 64 KB.
-   You must use TLS 1.2 \(based on TCP\) to guarantee data transmission security.

## Required configurations {#section_rrx_yvd_ghb .section}

-   You must associate Syslog with Log Service.

    Specifically, the port number of Syslog \(10009\) and the service endpoint of the target Project are required. For example, `cn-hangzhou-intranet.log.aliyuncs.com:10009` associates Syslog with a Project. The service endpoint of a Project in Log Service is a URL that is used to access the Project. For more information, see [Service endpoint](../../../../reseller.en-US/API Reference/Service endpoint.md#).

-   You must set the `STRUCTURED-DATA` field \(contains the parameters related to Log Service \) of Syslog as follows.

    |Configuration|Description|Example|
    |-------------|-----------|-------|
    |`STRUCTURED-DATA`|This field requires a name. This name must be Logservice.|Logservice|
    |`Project`|The user name is mapped to a project name.|test-project-1|
    |`Logstore`|The topic is mapped to a Logstore name. You must create a Logstore before you can collect data.

 |test-logstore-1|
    |`access-key-id`|Indicates your AccessKey ID. We recommend that you use the AK of a RAM user. For more information, see [Grant a RAM user the permissions to access Log Service](reseller.en-US/User Guide/Access control RAM/Grant a RAM user the permissions to access Log Service.md#).

 |<yourAccessKeyId\>|
    |`access-key-secret`|Indicates your AccessKey Secret. We recommend that you use the AK of a RAM user. For more information, see [Grant a RAM user the permissions to access Log Service](reseller.en-US/User Guide/Access control RAM/Grant a RAM user the permissions to access Log Service.md#).

 |<yourAccessKeySecret\>|


## Sample logs {#section_usx_4xd_ghb .section}

Log Service automatically resolves the received Syslog data. Furthermore, Log Service removes the Logservice field to avoid the AccessKey from being leaked. The following are the fields uploaded to Log Service \(for more information about fields, see [RFC5424 protocol](https://tools.ietf.org/html/rfc5424)\).

|Field|Description|
|-----|-----------|
|\_\_source\_\_|Indicates the hostname field in Syslog.|
|\_\_topic\_\_|This field is fixed as syslog-forwarder.|
|\_\_facility\_\_|Indicates the facility \(device or module\) information in Syslog.|
|\_\_program\_\_|Indicates a process name.|
|\_\_serverity\_\_|Indicates the log severity.|
|\_\_priority\_\_|Indicate the log priority.|
|\_\_unixtimestamp\_\_|Indicates a log timestamp in seconds.|
|content|Indicates the msg field in Syslog.|

![](images/42014_en-US.png)

## Application examples {#section_iwm_jyd_ghb .section}

For each of the following application examples, the required parameters are set as follows to have the Syslog logs be directly collected on a host to Log Service:

-   The project in Log Service is set as `test-project-1`.
-   The Logstore in Log Service is set as `test-logstore-1`.
-   The region where the project is located is set as `cn-hangzhou`.
-   The AccessKey ID of a RAM user that has the write permissions is set as `<yourAccessKeyId>`, and the AccessKey Secret of the RAM user is set as `<yourAccessKeySecret>`.

-   Application example 1: Use Rsyslog to forward system logs to Log Service

    Rsyslog is installed in a Linux host to process system logs. You can use Rsyslog to forward the system logs to Log Service. The Rsyslog configuration file format varies by Rsyslog versions. You can run the man rsyslogd command to view which Rsyslog version is being used.

    **Note:** Make sure that you have installed the gnutls module in Rsyslog. If the module is not installed in Rsyslog, you can run the sudo apt-get install rsyslog-gnutls command or the sudo yum install rsyslog-gnutls command to install the module.

    -   Rsyslog V8 and later

        `$DefaultNetstreamDriverCAFile` indicates the directory where the system root certificate is located.

        ```
        # Setup disk assisted queues 
                 $WorkDirectory /var/spool/rsyslog # where to place spool files 
                 $ActionQueueFileName fwdRule1     # unique name prefix for spool files 
                 $ActionQueueMaxDiskSpace 1g       # 1gb space limit (use as much as possible) 
                 $ActionQueueSaveOnShutdown on     # save messages to disk on shutdown 
                 $ActionQueueType LinkedList       # run asynchronously 
                 $ActionResumeRetryCount -1        # infinite retries if host is down 
                 $ActionSendTCPRebindInterval 100  # close and re-open the connection to the remote host every 100 of messages sent. 
                 #RsyslogGnuTLS set to default ca path 
                 $DefaultNetstreamDriverCAFile /etc/ssl/certs/ca-bundle.crt 
                 template(name="LogServiceFormat" type="string" 
                 string="<%pri%>1 %timestamp:::date-rfc3339% %HOSTNAME% %app-name% %procid% %msgid% [logservice project=\"test-project-1\" logstore=\"test-logstore-1\" access-key-id=\"<yourAccessKeyId>\" access-key-secret=\"<yourAccessKeySecret>\"] %msg%\n" 
                 ) 
                 # Send messages to Loggly over TCP using the template. 
                 action(type="omfwd" protocol="tcp" target="cn-hangzhou.log.aliyuncs.com" port="10009" template="LogServiceFormat" StreamDriver="gtls" StreamDriverMode="1" StreamDriverAuthMode="x509/name" StreamDriverPermittedPeers="*.log.aliyuncs.com")
        ```

    -   Rsyslog V7 and earlier

        `$DefaultNetstreamDriverCAFile` indicates the directory where the system root certificate is located.

        ```
        # Setup disk assisted queues 
                 $WorkDirectory /var/spool/rsyslog       # where to place spool files 
                 $ActionQueueFileName fwdRule1           # unique name prefix for spool files $ActionQueueMaxDiskSpace 1g             # 1gb space limit (use as much as possible) $ActionQueueSaveOnShutdown on           # save messages to disk on shutdown 
                 $ActionQueueType LinkedList             # run asynchronously 
                 $ActionResumeRetryCount -1              # infinite retries if host is down $ActionSendTCPRebindInterval 100        # close and re-open the connection to the remote host every 100 of messages sent. 
                 # RsyslogGnuTLS set to default ca path 
                 $DefaultNetstreamDriverCAFile /etc/ssl/certs/ca-bundle.crt 
                 $ActionSendStreamDriver gtls 
                 $ActionSendStreamDriverMode 1 
                 $ActionSendStreamDriverAuthMode x509/name 
                 $ActionSendStreamDriverPermittedPeer cn-hangzhou.log.aliyuncs.com 
                 template(name="LogServiceFormat" type="string" string="<%pri%>1 %timestamp:::date-rfc3339% %HOSTNAME% %app-name% %procid% %msgid% [logservice project=\"test-project-1\" logstore=\"test-logstore-1\" access-key-id=\"<yourAccessKeyId>\" access-key-secret=\"<yourAccessKeySecret>\"] %msg%\n") 
                 *.* action(type="omfwd" protocol="tcp" target="cn-hangzhou.log.aliyuncs.com" port="10009" template="LogServiceFormat")
        ```

    1.  Select the required version of Rsyslog, set either of the preceding configuration template according to your needs, and add the template to the end of the Rsyslog configuration file.

        **Note:** Generally, the directory to store the Rsyslog configuration file is /etc/rsyslog.conf.

    2.  Run the sudo service rsyslog restart and sudo /etc/init.d/syslog-ng restart commands or the systemctl restart rsyslog command to restart Rsyslog.
    3.  If no system logs are generated, run the logger command to generate a few logs for test. For example, `logger hello world!`.
-   Example 2: Use Syslog-ng to forward system logs to Log Service

    Syslog-ng is an open-source daemon for log management that helps Unix and Unix-like operating systems support the Syslog protocol. You can run the `sudo yum install syslog-ng` command or the `sudo apt-get install syslog-ng` command to install Syslog-ng.

    ```
    ### Syslog-ng Logging Config for LogService ### 
            template LogServiceFormat { 
            template("<${PRI}>1 ${ISODATE} ${HOST:--} ${PROGRAM:--} ${PID:--} ${MSGID:--} [logservice project=\"test-project-1\" logstore=\"test-logstore-1\" access-key-id=\"<yourAccessKeyId>\" access-key-secret=\"<yourAccessKeySecret>\"] $MSG\n"); template_escape(no); 
            }; 
            destination d_logservice{ 
            tcp("cn-hangzhou.log.aliyuncs.com" port(10009) 
            tls(peer-verify(required-untrusted)) 
            template(LogServiceFormat)); 
            }; 
            log { 
            source(s_src); # default use s_src 
            destination(d_logservice); 
            }; 
            ### END Syslog-ng Logging Config for LogService ###
    ```

    **Note:** By default, Rsyslog is installed in an ECS instance to process system logs. If you want to use Syslog-ng, you need to uninstall Syslog-ng first because Rsyslog and Syslog-ng cannot work together.

    1.  Set the preceding configuration template as needed, and add the template to the end of the Syslog-ng configuration file.

        **Note:** The typical directory to store the Syslog-ng configuration file is /etc/syslog-ng/syslog-ng.conf.

    2.  Run the sudo /etc/init.d/syslog-ng restart, sudo service syslog-ng restart, and sudo systemctl restart syslog-ng commands to restart Syslog-ng.
    3.  If no system logs are generated, run the logger command to generate a few logs for test. For example, logger hello world!.

## Troubleshoot exceptions {#section_rn5_vb2_ghb .section}

-   Manually upload a log to check the network connectivity

    You can use the ncat command to simulate uploading system logs. This method can check the network connectivity and whether the AccessKey has the permissions to upload logs. If ncat is not installed in your server, you can run the sudo yum install nmap-ncat command to install it. For example, the following command is run to send a system log to Log Service:

    ```
    [root@iZbp145dd9fccuidd7g**** ~]# ncat --ssl cn-hangzhou.log.aliyuncs.com 10009 
           <34>1 2019-03-28T03:00:15.003Z mymachine.example.com su - ID47 [logservice project="test-project-1" logstore="test-logstore-1" access-key-id="<yourAccessKeyId>" access-key-secret="<yourAccessKeySecret>"] this is a test message
    ```

    The project in Log Service is set as `test-project-1` and the Logstore is set as `test-logstore-1`. The region where the project is located is set as cn-hangzhou. The AccessKey ID of a RAM user that has the write permissions is set as `<yourAccessKeyId>`, and the AccessKey Secret of the RAM user is set as `<yourAccessKeySecret>`.

    **Note:** 

    -   When you run the ncat command to upload a log, you must specify your current time for the log in ISO 8601 format. For example, if you upload a log at 2019-03-28T11:00:15.003, you must convert it to 2019-03-28T03:00:15.003Z before you add it to the ncat command.
    -   The ncat commands cannot identify network connection interrupt. Therefore, you need to enter messages and press Enter within 30 seconds after you run a ncat command.
    After you run the command, you can preview logs in the Log Service console. For more information, see [Preview log data](reseller.en-US/User Guide/Real-time subscription and consumption/Preview log data.md#).

-   Diagnose log collection errors

    If you fail to manually upload logs, you can diagnose log collection errors to view error information. For more information, see [Diagnose collection errors](../../../../reseller.en-US/FAQ/Log collection/Diagnose collection errors.md#).

-   Check Rsyslog error logs

    Rsyslog logs are saved in the /var/log/message directory be default. You can run a vim command to view the directory.

    -   Rsyslog error

        ```
        dlopen: /usr/lib64/rsyslog/lmnsd_gtls.so: cannot open shared object file: No such file or directory
        ```

        This error occurred because the gnutls module is not installed. You can run the sudo apt-get install rsyslog-gnutls command or the sudo yum install rsyslog-gnutls command to install and restart Rsyslog.

    -   Rsyslog error

        ```
        unexpected GnuTLS error -53 - this could be caused by a broken connection. GnuTLS reports:Error in the push function
        ```

        This error occurred because the TCP connection was idle for long time and then was forcibly shutdown. Rsyslog can automatically connect to Log Service.

-   Check Syslog-ng error logs

    Syslog-ng logs are saved in the Journal logs by default. You can run the systemctl status syslog-ng.service and journalctl -xe command to check detailed logs.

    Syslog-ng initialization error

    ```
    Job for syslog-ng.service failed because the control process exited with error code. See "systemctl status syslog-ng.service" and "journalctl -xe" for details
    ```

    You must check that the Syslog-ng configuration file format is valid and that the settings in the file do not conflict \(for example, multiple internal\(\); are not set\).


