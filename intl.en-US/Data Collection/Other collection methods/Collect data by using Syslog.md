# Collect data by using Syslog {#concept_inz_rkc_ghb .concept}

Log Service allows you to collect data by using Syslog rather than using third-party agents to forward collected data.

## Limits {#section_gjh_s5d_ghb .section}

-   Syslog messages must conform to [RFC 5424](https://tools.ietf.org/html/rfc5424).
-   The maximum size of each log entry is 64 KB.
-   To ensure secure data transmission, TCP-based Transport Layer Security \(TLS\) 1.2 must be used for data transmission.

## Parameters {#section_rrx_yvd_ghb .section}

When collecting data by using Syslog, you must associate Syslog with the endpoint of Log Service. The port number of Syslog is 10009. You must use the service endpoint of your project. For more information about service endpoints, see [Service endpoint](../../../../reseller.en-US/API Reference/Service endpoint.md#). Example: `cn-hangzhou-intranet.log.aliyuncs.com:10009`. In addition, you must specify a project, Logstore, and AccessKey pair for Log Service in the STRUCTURED-DATA field of each Syslog message.

|Parameter|Description|Example|
|---------|-----------|-------|
|STRUCTURED-DATA name|Each STRUCTURED-DATA field must have a name and the name must be Logservice.|Logservice|
|Project|A project in Log Service. Create a project in Log Service before you collect data.|test-project-1|
|Logstore|A Logstore in Log Service. You must create a Logstore before you collect data.|test-logstore-1|
|access-key-id|Your AccessKey ID. We recommend that you use the AccessKey ID of a RAM user account. For more information, see [Authorize a RAM user to access Log Service](reseller.en-US/Access control RAM/Grant a RAM user the permissions to access Log Service.md#).|<yourAccessKeyId\>|
|access-key-secret|Your AccessKey Secret. We recommend that you use the AccessKey Secret of a RAM user account. For more information, see [Grant a RAM user the permission to access Log Service](reseller.en-US/Access control RAM/Grant a RAM user the permissions to access Log Service.md#).|<yourAccessKeySecret\>|

## Sample log {#section_usx_4xd_ghb .section}

Log Service parses received Syslog messages. To ensure the security of the AccessKey pair, the Logservice field is deleted by default. The following table describes fields that are received by Log Service.

|Field name|Description|
|----------|-----------|
|\_\_source\_\_|The HOSTNAME field defined in Syslog.|
|\_\_topic\_\_|The value of this field is syslog-forwarder.|
|\_\_facility\_\_|The facility \(device/module\) information defined in Syslog.|
|\_\_program\_\_|The process name.|
|\_\_serverity\_\_|The severity of the log entry.|
|\_\_priority\_\_|The priority of the log entry.|
|\_\_unixtimestamp\_\_|The timestamp of the log entry. Unit: nanosecond.|
|content|The MSG field defined in Syslog.|

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/150480/156695863042014_en-US.png)

For more information, see [RFC 5424](https://tools.ietf.org/html/rfc5424).

## Sample configuration {#section_iwm_jyd_ghb .section}

You want to collect Syslog messages from your host and write them into Log Service. The project in Log Service is named `test-project-1` and the Logstore is named `test-logstore-1`. The project resides in the China \(Hangzhou\) region. The AccessKey ID of the RAM user account with the write permission is `<yourAccessKeyId>`, and the AccessKey Secret is `<yourAccessKeySecret>`.

-   Example 1: Use Rsyslog to forward logs to Log Service

    Rsyslog is installed on Linux hosts by default to process system logs. You can configure Rsyslog to forward system logs to Log Service. The configuration files of Rsyslog differ between versions. You can run the `man rsyslogd` command to view the Rsyslog version.

    **Note:** You must ensure that you have installed the rsyslog-gnutls package. You can run the `sudo apt-get install rsyslog-gnutls` or `sudo yum install rsyslog-gnutls` command to install the rsyslog-gnutls package.

    1.  Use parameters specific to your environment when referring to the sample code provided for the following two versions and add the code to the end of the Rsyslog configuration file. The address of the Rsyslog configuration file is /etc/rsyslog.conf.
        -   Rsyslog V8 and later

            Set `$DefaultNetstreamDriverCAFile` to the location of the root certificate in the system.

            ``` {#codeblock_3ky_7yo_aqe}
            # Set up disk assisted queues 
            $WorkDirectory /var/spool/rsyslog # where to place spool files 
            $ActionQueueFileName fwdRule1     # unique name prefix for spool files 
            $ActionQueueMaxDiskSpace 1g       # 1gb space limit (use as much as possible) 
            $ActionQueueSaveOnShutdown on     # save messages to disk on shutdown 
            $ActionQueueType LinkedList       # run asynchronously 
            $ActionResumeRetryCount -1        # infinite retries if host is down 
            $ActionSendTCPRebindInterval 100  # close and re-open the connection to the remote host every 100 of messages sent. 
            # RsyslogGnuTLS set to default ca path 
            $DefaultNetstreamDriverCAFile /etc/ssl/certs/ca-bundle.crt 
            template(name="LogServiceFormat" type="string" 
            string="<%pri%>1 %timestamp:::date-rfc3339% %HOSTNAME% %app-name% %procid% %msgid% [logservice project=\"test-project-1\" logstore=\"test-logstore-1\" access-key-id=\"<yourAccessKeyId>\" access-key-secret=\"<yourAccessKeySecret>\"] %msg%\n" 
            ) 
            # Send messages to Loggly over TCP using the template 
            action(type="omfwd" protocol="tcp" target="cn-hangzhou.log.aliyuncs.com" port="10009" template="LogServiceFormat" StreamDriver="gtls" StreamDriverMode="1" StreamDriverAuthMode="x509/name" StreamDriverPermittedPeers="*.log.aliyuncs.com")
            ```

        -   Rsyslog V7 and earlier

            Set `$DefaultNetstreamDriverCAFile` to the location of the root certificate in the system.

            ``` {#codeblock_lu0_na7_ryn}
            # Set up disk assisted queues 
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

    2.  Run the `sudo service rsyslog restart`, `sudo /etc/init.d/syslog-ng restart`, or `systemctl restart rsyslog` command to restart Rsyslog.
    3.  If no Syslog message is generated, you can run the `logger` command to generate several test logs, for example, `logger hello world!`.
-   Example 2: Use Syslog-ng to forward logs to Log Service

    Syslog-ng is an open-source log management daemon that enables you to collect data by using Syslog on Unix and Unix-like operating systems. You can run the `sudo yum install syslog-ng` or `sudo apt-get install syslog-ng` command to install Syslog-ng.

    ``` {#codeblock_am4_y02_604}
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

    **Note:** Rsyslog is installed on each ECS instance by default to take over Syslog. You must uninstall Rsyslog before you can use Syslog-ng because Rsyslog and Syslog-ng cannot work at the same time.

    1.  Use parameters specific to your environment when referring to the preceding sample code and add the code to the end of the Syslog-ng configuration file. The address of the Syslog-ng configuration file is /etc/syslog-ng/syslog-ng.conf .
    2.  Run the `sudo /etc/init.d/syslog-ng restart`, `sudo service syslog-ng restart`, or `sudo systemctl restart syslog-ng` command to restart Syslog-ng.
    3.  If no Syslog message is generated, you can run the `logger` command to generate several test logs, for example, `logger hello world!`.

## FAQ {#section_rn5_vb2_ghb .section}

-   How can I manually send a Syslog message to Log Service?

    You can use the ncat command to send a Syslog message to Log Service. This helps check network connectivity and whether the AccessKey pair is authorized to send Syslog messages. If ncat is not installed on your host, you can run the `sudo yum install nmap-ncat` command to install ncat. For example, you can run the following command to send a Syslog message to Log Service. The project in Log Service is named `test-project-1` and the Logstore is named `test-logstore-1`. The project resides in the China \(Hangzhou\) region. The AccessKey ID of the RAM user account with the write permission is `<yourAccessKeyId>`, and the AccessKey Secret is `<yourAccessKeySecret>`.

    **Note:** 

    -   The timestamp for sending Syslog messages uses UTC+0, such as 2019-03-28T03:00:15.003Z. However, in UTC+8, this time is 2019-03-28T11:00:15.003.
    -   The ncat command cannot be used to determine whether the connection is interrupted. You must enter the information to be sent and press Enter to trigger message sending within 30 seconds after running the ncat command.
    ``` {#codeblock_80v_bex_i6k}
    [root@iZbp145dd9fccuidd7g**** ~]# ncat --ssl cn-hangzhou.log.aliyuncs.com 10009 
    <34>1 2019-03-28T03:00:15.003Z mymachine.example.com su - ID47 [logservice project="test-project-1" logstore="test-logstore-1" access-key-id="<yourAccessKeyId>" access-key-secret="<yourAccessKeySecret>"] this is a test message
    ```

    After you send the Syslog message, you can preview it in the Log Service console. For more information, see [Preview logs](reseller.en-US/Real-time subscription and consumption/Consume logs.md#).

-   What can I do if a manually triggered Syslog message transmission fails?

    Troubleshoot the failure according to error messages. For more information, see [Diagnose collection errors](../../../../reseller.en-US/FAQ/Log collection/Diagnose collection errors.md#).

-   How can I view Rsyslog error messages?

    Rsyslog error messages are stored in /var/log/message. You can run the vim command to view Rsyslog error messages.

    -   Rsyslog error messages \(Example 1\):

        ``` {#codeblock_mhf_dap_0se}
        dlopen: /usr/lib64/rsyslog/lmnsd_gtls.so: cannot open shared object file: No such file or directory
        ```

        This error message is returned when the rsyslog-gnutls package is not installed. You can run the `sudo apt-get install rsyslog-gnutls` or `sudo yum install rsyslog-gnutls` command to install Rsyslog. Then, you must restart Rsyslog.

    -   Rsyslog error message \(Example 2\):

        ``` {#codeblock_qc9_uyq_yj4}
        unexpected GnuTLS error -53 - this could be caused by a broken connection. GnuTLS reports:Error in the push function
        ```

        This error message is returned when the TCP connection is closed because it has been idle for a long period of time. You can ignore the error because Rsyslog automatically reconnects to the server.

-   How can I view Rsyslog-ng error messages?

    Syslog-ng error messages are stored in journals by default. You can run `systemctl status syslog-ng.service` and `journalctl-xe` commands to view Rsyslog-ng error messages.

    The error message returned when Syslog-ng failed to start:

    ``` {#codeblock_9ab_orl_m3o}
    Job for syslog-ng.service failed because the control process exited with error code. See "systemctl status syslog-ng.service" and "journalctl -xe" for details
    ```

    If this error occurs, check whether the configuration file format is valid or whether configuration conflicts exist. For example, you cannot configure multiple `internal()` sources.


