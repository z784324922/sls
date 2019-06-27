# Syslog协议写入 {#concept_inz_rkc_ghb .concept}

日志服务支持使用Syslog协议直接采集数据，无需使用其他采集Agent转发采集。

## 相关限制 {#section_gjh_s5d_ghb .section}

-   Syslog协议必须为标准[RFC5424](https://tools.ietf.org/html/rfc5424)协议。
-   每条日志最大支持64KB。
-   为保证数据传输安全性，数据传输必须使用基于TCP的TLS1.2（Transport-level security）。

## 配置方式 {#section_rrx_yvd_ghb .section}

使用Syslog协议写入时，您需要将Syslog指向日志服务的地址，Syslog的端口为10009，请选择您对应project所在的服务入口。服务入口列表参考：[服务入口](../../../../intl.zh-CN/API 参考/服务入口.md#)。例如cn-hangzhou-intranet.log.aliyuncs.com:10009。同时您需要在Syslog中的STRUCTURED-DATA字段配置日志服务的Project、Logstore、AccessKey等信息。

|配置名|说明|示例|
|---|--|--|
|STRUCTURED-DATA|每个STRUCTURED-DATA必须具备一个name，且名称必须为Logservice。|Logservice|
|Project|用户名映射为日志服务中的Project名称。|test-project-1|
|Logstore|Topic映射为日志服务中的Logstore名称，请提前创建好Logstore。|test-logstore-1|
|access-key-id|您的AccessKey ID。建议使用子账号AK，请参考[授权](intl.zh-CN/用户指南/         访问控制 RAM/授权RAM 用户.md#)。|<yourAccessKeyId\>|
|access-key-secret|您的AccessKey Secret。建议使用子账号AK，请参考[授权](intl.zh-CN/用户指南/         访问控制 RAM/授权RAM 用户.md#)。|<yourAccessKeySecret\>|

## 日志样例 {#section_usx_4xd_ghb .section}

日志服务会自动解析接收到的Syslog协议，为避免AccessKey信息泄露，默认会将上报的Logservice字段删除。上传到日志服务的字段（详细信息，请参考[RFC5424协议](https://tools.ietf.org/html/rfc5424)）如下：

|字段名|说明|
|---|--|
|\_\_source\_\_|Syslog中的hostname字段。|
|\_\_topic\_\_|固定为syslog-forwarder。|
|\_\_facility\_\_|Syslog中的facility（设备/模块）信息。|
|\_\_program\_\_|进程名。|
|\_\_serverity\_\_|日志严重性。|
|\_\_priority\_\_|日志优先级。|
|\_\_unixtimestamp\_\_|日志中的时间戳（单位纳秒）。|
|content|Syslog的msg字段。|

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/150480/156160579442014_zh-CN.png)

## 配置示例 {#section_iwm_jyd_ghb .section}

用户希望将主机上的Syslog日志直接采集到日志服务，日志服务Project名为`test-project-1`，Logstore名为`test-logstore-1`，Project所在地域为杭州（cn-hangzhou），具有写入权限的子账号AccessKey ID为`<yourAccessKeyId>`、AccessKey Secret为`<yourAccessKeySecret>`。

-   示例1：使用Rsyslog转发到日志服务

    通常Linux主机都会默认安装Rsyslog作为系统日志处理工具，您可配置Rsyslog直接将系统日志转发到日志服务。Rsyslog不同版本配置文件格式略有不同，您可通过 `man rsyslogd` 查看Rsyslog版本。

    **说明：** 您需要确保Rsyslog已经安装了gnutls模块，若没有安装，可以使用 `sudo apt-get install rsyslog-gnutls` 、`sudo yum install rsyslog-gnutls` 安装。

    -   Rsyslog V8及以上

        其中`$DefaultNetstreamDriverCAFile`配置为系统的根证书位置。

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

    -   Rsyslog V7及以下

        其中`$DefaultNetstreamDriverCAFile`配置为系统的根证书位置。

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

    1.  将上述两个版本内容分别替换为您的实际参数，并添加到Rsyslog配置文件尾，通常Rsyslog配置文件地址为：/etc/rsyslog.conf 。
    2.  使用`sudo service rsyslog restart`、`sudo /etc/init.d/syslog-ng restart` 或 `systemctl restart rsyslog`重启Rsyslog。
    3.  若没有Syslog产生，可使用`logger`命令产生几条测试日志，例如`logger hello world!`。
-   示例2：使用Syslog-ng转发到日志服务

    Syslog-ng是一款开源的日志管理Daemon，为Unix以及Unix-like的系统提供Syslog协议的实现。通常Syslog-ng的安装方式为：`sudo yum install syslog-ng` 、 `sudo apt-get install syslog-ng`。

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

    **说明：** ECS默认会安装Rsyslog接管Syslog，由于Rsyslog和Syslog-ng无法同时工作，如果您要使用Syslog-ng请先卸载Rsyslog。

    1.  将上述内容替换为您的实际参数，添加到Syslog-ng配置文件尾，通常Syslog-ng配置文件地址为：`/etc/syslog-ng/syslog-ng.conf` 。
    2.  使用`sudo /etc/init.d/syslog-ng restart`、`sudo service syslog-ng restart`、`sudo systemctl restart syslog-ng`重启Syslog-ng。
    3.  若没有Syslog产生，可使用`logger`命令产生几条测试日志，例如`logger hello world!`。

## 常见问题与排查 {#section_rn5_vb2_ghb .section}

-   手动上传日志

    您可使用ncat命令模拟上报Syslog，以此检查网络连通性以及AccessKey是否具备上报权限。若您的机器上没有安装ncat，可使用`sudo yum install nmap-ncat`命令安装。例如下述命令则向日志服务发送一条Syslog，其中日志服务Project名为`test-project-1`，Logstore名为`test-logstore-1`，Project所在地域为杭州（cn-hangzhou），具有写入权限的子账号AccessKey ID为`<yourAccessKeyId>`、AccessKey Secret为`<yourAccessKeySecret>`。

    **说明：** 

    -   上报的时间为0时区时间，例如2019-03-28T03:00:15.003Z的东八区时间为：2019-03-28T11:00:15.003。
    -   ncat命令不会判断连接中断，请在ncat命令执行30秒内输入待发送的信息并输入回车触发发送。
    ```
    [root@iZbp145dd9fccuidd7g**** ~]# ncat --ssl cn-hangzhou.log.aliyuncs.com 10009 
    <34>1 2019-03-28T03:00:15.003Z mymachine.example.com su - ID47 [logservice project="test-project-1" logstore="test-logstore-1" access-key-id="<yourAccessKeyId>" access-key-secret="<yourAccessKeySecret>"] this is a test message
    ```

    输入成功后，可通过日志服务控制台预览日志（具体操作步骤请参考[日志预览](intl.zh-CN/用户指南/实时消费/普通消费.md#) ）。

-   诊断采集错误

    若手动上传日志失败，您可通过诊断采集错误查看具体报错信息，详细操作步骤请参考[诊断采集错误](../../../../intl.zh-CN/常见问题/日志采集/诊断采集错误.md#)。

-   查看Rsyslog报错日志

    Rsyslog日志默认保存在`/var/log/message`中，您可直接通过vim命令查看。

    -   Rsyslog报错信息：

        ```
        dlopen: /usr/lib64/rsyslog/lmnsd_gtls.so: cannot open shared object file: No such file or directory
        ```

        该错误是因为没有安装gnutls模块，可以使用 `sudo apt-get install rsyslog-gnutls` 或`sudo yum install rsyslog-gnutls` 安装并重启Rsyslog。

    -   Rsyslog报错信息：

        ```
        unexpected GnuTLS error -53 - this could be caused by a broken connection. GnuTLS reports:Error in the push function
        ```

        该错误是因为TCP连接长期闲置时被强制关闭，Rsyslog会进行自动重连，您无需关注。

-   查看Syslog-ng报错日志

    Syslog-ng日志默认保存在Journal日志中，您可直接通过`systemctl status syslog-ng.service`和 `journalctl -xe`查看详细日志。

    Syslog-ng启动报错：

    ```
    Job for syslog-ng.service failed because the control process exited with error code. See "systemctl status syslog-ng.service" and "journalctl -xe" for details
    ```

    出现该报错，请检查配置文件格式是否合法或配置是否存在冲突（例如不支持配置多个internal\(\);）。


