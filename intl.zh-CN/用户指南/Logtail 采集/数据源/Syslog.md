# Syslog {#concept_czq_zrc_wdb .concept}

Logtail支持在本地配置TCP端口，接收syslog Agent通过TCP协议转发过来的syslog数据，Logtail解析接收到的数据并转发至LogHub中。

## 前提条件 {#section_mtm_5sc_wdb .section}

设置使用Logtail收集日志前，您需要安装Logtail。Logtail支持Windows和Linux两大操作系统，安装方法参见 [Linux](intl.zh-CN/用户指南/Logtail 采集/安装/Linux.md) 和[Windows](intl.zh-CN/用户指南/Logtail 采集/安装/Windows.md)。

## 步骤 1 在日志服务管理控制台创建Logtail syslog配置 {#section_rbk_wdw_tcb .section}

1.  在日志服务云控制台单击目标项目，进入Logstore列表。
2.  选择目标Logstore，并单击**数据接入向导**图标，进入数据接入流程。
3.  选择数据源类型。

    单击**自定义数据**中的**Syslog**，并单击**下一步**。

4.  指定 Logtail配置的名称。

    配置名称只能包含小写字母、数字、连字符（-）和下划线（\_），且必须以小写字母和数字开头和结尾，长度为 3~63 字节。

    **说明：** 配置名称设置后不可修改。

5.  填写**Tag设置**。

    如何设置Tag，请参考[Syslog-采集参考](intl.zh-CN/用户指南/Logtail 采集/数据源/Syslog-采集参考.md)。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13063/2869_zh-CN.png "设置Tag")

6.  酌情配置**高级选项**。

    请选择是否打开**本地缓存**。当日志服务不可用时，日志可以缓存在机器本地目录，服务恢复后进行续传。默认开启缓存，最大缓存值1GB。

7.  根据页面提示，应用Logtail配置到机器组。

    确认勾选所需的机器组并单击 **应用到机器组** 将配置应用到机器组。

    如果您还未创建机器组，需要先创建一个机器组。有关如何创建机器组，参见 [创建机器组](intl.zh-CN/用户指南/Logtail 采集/机器组/创建机器组.md)。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13066/2904_zh-CN.png "应用到机器组")


## 步骤 2 配置Logtail使协议生效 {#section_wtx_xdw_tcb .section}

从机器Logtail安装目录下找到 `ilogtail_config.json`，一般在 `/usr/local/ilogtail/`目录下面。根据需求修改和syslog相关的配置。

1.  确认syslog功能已开启。

    true表示syslog功能处于打开状态，false表示关闭状态。

    ```
    "streamlog_open" : true
    ```

2.  配置syslog用于接收日志的内存池大小。程序启动时会一次性申请指定大小的内存，请根据机器内存大小以及实际需求填写，单位是MB。

    ```
    "streamlog_pool_size_in_mb" : 50
    ```

3.  配置缓冲区大小。需要配置Logtail每次调用socket io rcv 接口使用的缓冲区大小，单位是byte。

    ```
    "streamlog_rcv_size_each_call" : 1024
    ```

4.  配置日志syslog格式。

    ```
    "streamlog_formats":[]
    ```

5.  配置TCP绑定地址和端口。需要配置Logtail用于接收syslog日志的TCP绑定地址和端口，默认是绑定0.0.0.0下的11111端口。

    ```
    "streamlog_tcp_addr" : "0.0.0.0",
    "streamlog_tcp_port" : 11111
    
    ```

6.  配置完成后重启Logtail。重启Logtail要执行以下命令关闭Logtail客户端，并再次打开。

    ```
    sudo /etc/init.d/ilogtaild stop
    sudo /etc/init.d/ilogtaild start
    
    ```


## 步骤 3 安装rsyslog并修改配置 {#section_o2w_l2w_tcb .section}

如果机器已经安装rsyslog，忽略这一步。

1.  安装rsyslog。

    安装方法请参见：

    -   [Ubuntu 安装方法](http://www.rsyslog.com/ubuntu-repository/)
    -   [Debian 安装方法](http://www.rsyslog.com/debian-repository/)
    -   [RHEL/CENTOS 安装方法](http://www.rsyslog.com/rhelcentos-rpms/)
2.  修改配置。

    在 `/etc/rsyslog.conf` 中根据需要修改配置，例如：

    ```
    $WorkDirectory /var/spool/rsyslog # where to place spool files
    $ActionQueueFileName fwdRule1 # unique name prefix for spool files
    $ActionQueueMaxDiskSpace 1g # 1gb space limit (use as much as possible)
    $ActionQueueSaveOnShutdown on # save messages to disk on shutdown
    $ActionQueueType LinkedList # run asynchronously
    $ActionResumeRetryCount -1 # infinite retries if host is down
    # 定义日志数据的字段
    $template ALI_LOG_FMT,"0.1 sys_tag %timegenerated:::date-unixtimestamp% %fromhost-ip% %hostname% %pri-text% %protocol-version% %app-name% %procid% %msgid% %msg:::drop-last-lf%\n"
    *.* @@10.101.166.173:11111;ALI_LOG_FMT
    
    ```

    **说明：** 模板 `ALI_LOG_FMT` 中第二个域的值是 `sys_tag`，这个取值必须和步骤 1 中创建的一致，这个配置的含义是将本机接收到的所有（`\*.\*`）syslog 日志按照 `ALI_LOG_FMT`格式化，使用 TCP 协议转发到 10.101.166.173:11111。机器 10.101.166.173 必须在步骤 1 中的机器组中并且按照步骤 2 配置。

3.  启动rsyslog。

    ```
    sudo /etc/init.d/rsyslog restart
    ```

    启动之前请先检查机器上是否安装了其他syslog的Agent，比如 syslogd、sysklogd、syslog-ng 等，如果有的话请关闭。


上面三步完成之后就可以将机器上的syslog收集到日志服务了。

## 更多信息 {#section_nmy_btq_pdb .section}

有关syslog日志采集的更多信息以及如何格式化syslog数据，请参见[Syslog-采集参考](intl.zh-CN/用户指南/Logtail 采集/数据源/Syslog-采集参考.md)。

