# Journal/Systemd日志输入源 {#concept_oc3_pc5_ngb .concept}

Logtail支持从原始的二进制文件中采集Linux系统的Journal（systemd）日志。

systemd是专用于 Linux 操作系统的系统与服务管理器。当作为启动进程（PID=1）运行时，它将作为初始化系统运行，启动并维护各种用户空间的服务。 systemd统一管理所有Unit的日志（包括内核和应用日志），配置文件一般在/etc/systemd/journald.conf中。

## 功能 {#section_qj4_tc5_ngb .section}

-   支持设置初始同步位置，后续采集会自动保存checkpoint，应用重启时不影响进程。
-   支持过滤指定的Unit。
-   支持采集内核日志。
-   支持自动解析日志等级。
-   支持以容器方式采集宿主机上的Journal日志，适用于Docker/Kubernetes场景。

## 限制说明 {#section_jm2_5c5_ngb .section}

-   Logtail支持版本为0.16.18及以上。
-   运行的操作系统需支持Journal日志格式。

## 应用场景 {#section_u5x_5c5_ngb .section}

-   监听内核事件，出现异常时自动告警。
-   采集所有系统日志，用于长期存储，减少磁盘空间占用。
-   采集软件（Unit）的输出日志，用于分析或告警。
-   采集所有Journal日志，可以从所有日志中快速检索关键词或日志，相比Journalctl查询大幅提升。

## 配置方式 {#section_ddp_wc5_ngb .section}

该输入源类型为：`service_journal`。

**说明：** Logtail支持对采集的数据进行处理加工后上传，处理方式参见[处理采集数据](cn.zh-CN/用户指南/Logtail采集/自定义插件/处理采集数据.md)。

|配置项|类型|是否必须|说明|
|:--|:-|:---|:-|
|JournalPaths|string数组|是|Journal日志路径，建议直接填写Journal日志所在文件夹，例如/var/log/journal。|
|SeekPosition|string|否|首次采集方式，可以配置为：-   head表示采集所有数据。
-   tail表示只采集配置应用后新的数据。

默认为tail。|
|Kernel|bool|否|-   true表示采集内核日志
-   false表示不采集内核日志

默认为true。|
|Units|string数组|否|指定采集的Unit列表，为空时则全部采集，默认为空。|
|ParseSyslogFacility|bool|否|是否解析syslog日志的facility字段，默认为false。|
|ParsePriority|bool|否|是否解析Priority字段，默认为false。|
|UseJournalEventTime|bool|否|是否使用Journal日志中的字段作为日志时间，默认为false，即使用采集时间作为日志时间。实时日志采集一般相差3秒以内。|

ParsePriority映射关系表如下：

```
"0": "emergency"
  "1": "alert"
  "2": "critical"
  "3": "error"
  "4": "warning"
  "5": "notice"
  "6": "informational"
  "7": "debug"
```

## 示例 {#section_bjs_ld5_ngb .section}

-   **示例1**

    从默认的`/var/log/journal`路径采集journal日志，采集配置为：

    ```
    {
      "inputs": [
        {
          "detail": {
            "JournalPaths": [
              "/var/log/journal"
            ]
          },
          "type": "service_journal"
        }
      ]
    }
    ```

    日志样例：

    ```
    MESSAGE:  rejected connection from "192.168.0.250:43936" (error "EOF", ServerName "")
    PACKAGE:  embed
    PRIORITY:  6
    SYSLOG_IDENTIFIER:  etcd
    _BOOT_ID:  fe919cd1268f4721bd87b5c18afe59c3
    _CAP_EFFECTIVE:  0
    _CMDLINE:  /usr/bin/etcd --election-timeout=3000 --heartbeat-interval=500 --snapshot-count=50000 --data-dir=data.etcd --name 192.168.0.251-name-3 --client-cert-auth --trusted-ca-file=/var/lib/etcd/cert/ca.pem --cert-file=/var/lib/etcd/cert/etcd-server.pem --key-file=/var/lib/etcd/cert/etcd-server-key.pem --peer-client-cert-auth --peer-trusted-ca-file=/var/lib/etcd/cert/peer-ca.pem --peer-cert-file=/var/lib/etcd/cert/192.168.0.251-name-3.pem --peer-key-file=/var/lib/etcd/cert/192.168.0.251-name-3-key.pem --initial-advertise-peer-urls https://192.168.0.251:2380 --listen-peer-urls https://192.168.0.251:2380 --advertise-client-urls https://192.168.0.251:2379 --listen-client-urls https://192.168.0.251:2379 --initial-cluster 192.168.0.249-name-1=https://192.168.0.249:2380,192.168.0.250-name-2=https://192.168.0.250:2380,192.168.0.251-name-3=https://192.168.0.251:2380 --initial-cluster-state new --initial-cluster-token abac64c8-baab-4ae6-8412-4253d3cfb0cf
    _COMM:  etcd
    _EXE:  /opt/etcd-v3.3.8/etcd
    _GID:  995
    _HOSTNAME:  iZbp1f7y2ikfe4l8nx95amZ
    _MACHINE_ID:  f0f31005fb5a436d88e3c6cbf54e25aa
    _PID:  10926
    _SOURCE_REALTIME_TIMESTAMP:  1546854068863857
    _SYSTEMD_CGROUP:  /system.slice/etcd.service
    _SYSTEMD_SLICE:  system.slice
    _SYSTEMD_UNIT:  etcd.service
    _TRANSPORT:  journal
    _UID:  997
    __source__:  172.16.1.4
    __tag__:__hostname__:  logtail-ds-8kqb9
    __topic__:  
    _monotonic_timestamp_:  1467135144311
    _realtime_timestamp_:  1546854068864309
    ```

-   **示例2**

    Kubernetes场景下，使用Logtail的DaemonSet模式采集宿主机的系统日志，由于日志中有很多并不重要的字段，使用处理插件只挑选较为重要的日志字段。

    采集配置为：

    ```
    {
      "inputs": [
        {
          "detail": {
            "JournalPaths": [
              "/logtail_host/var/log/journal"
            ],
            "ParsePriority": true,
            "ParseSyslogFacility": true
          },
          "type": "service_journal"
        }
      ],
      "processors": [
        {
          "detail": {
            "Exclude": {
              "UNIT": "^libcontainer.*test"
            }
          },
          "type": "processor_filter_regex"
        },
        {
          "detail": {
            "Include": [
              "MESSAGE",
              "PRIORITY",
              "_EXE",
              "_PID",
              "_SYSTEMD_UNIT",
              "_realtime_timestamp_",
              "_HOSTNAME",
              "UNIT",
              "SYSLOG_FACILITY",
              "SYSLOG_IDENTIFIER"
            ]
          },
          "type": "processor_pick_key"
        }
      ]
    }
    ```

    日志样例：

    ```
    MESSAGE:  rejected connection from "192.168.0.251:48914" (error "EOF", ServerName "")
    PRIORITY:  informational
    SYSLOG_IDENTIFIER:  etcd
    _EXE:  /opt/etcd-v3.3.8/etcd
    _HOSTNAME:  iZbp1i0czq3zgvxlx7u8ueZ
    _PID:  10590
    _SYSTEMD_UNIT:  etcd.service
    __source__:  172.16.0.141
    __tag__:__hostname__:  logtail-ds-dp48x
    __topic__:  
    _realtime_timestamp_:  1547975837008708
    ```


