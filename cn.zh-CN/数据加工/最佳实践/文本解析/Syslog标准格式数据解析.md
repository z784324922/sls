# Syslog标准格式数据解析 {#concept_2022810 .concept}

本文档介绍如何使用LOG DSL中的GROK高效快捷的解析不同格式的Syslog日志。

## 概况 {#section_o11_bvn_xp5 .section}

Syslog是一种行业标准的协议，可用来记录设备的日志。在Unix类操作系统上，Syslog广泛应用于系统日志。Syslog日志消息既可以记录在本地文件中，也可以通过网络发送到接收Syslog的服务器。服务器可以对多个设备的Syslog消息进行统一的存储，或者解析其中的内容做相应的处理。常见的应用场景是网络管理工具、安全管理系统、日志审计系统。

## 问题 {#section_fno_ztz_11p .section}

长期以来，没有一个标准来规范Syslog的格式，导致Syslog的格式非常随意。甚至有些情况下没有任何格式，导致程序不能对Syslog消息进行解析，只能将它看作是一个字符串。所以如何解析不同格式的Syslog日志就变的非常重要。

## Syslog协议标准 {#section_ula_f3x_bnz .section}

目前业界存在常见两种Syslog日志协议，一个是2009年的RFC524协议，另外一个是2001年的RFC3164协议。以下为大家介绍这两种协议的不同之处，以便在后续实践中能够灵活应用GROK解析Syslog日志。

-   RFC5424协议

    RFC5424协议包含以下字段信息，具体信息请参见[官方协议](https://tools.ietf.org/html/rfc5424)。

    ``` {#codeblock_lwu_lf5_4jz}
    PRI VERSION SP TIMESTAMP SP HOSTNAME SP APP-NAME SP PROCID SP MSGID
    ```

    通过以下几个示例来对以上字段进行说明：

    ``` {#codeblock_irf_ds6_6e9}
    """
    Example1:
    <34>1 2019-07-11T22:14:15.003Z aliyun.example.com ali - ID47 - BOM'su root' failed for lonvick on /dev/pts/8
    """
    PRI -- 34
    VERSION -- 1
    TIMESTAMP -- 2019-07-11T22:14:15.003Z
    HOSTNAME -- aliyun.example.com
    APP-NAME -- ali
    PROCID -- 无
    MSGID -- ID47
    MESSAGE -- 'su root' failed for lonvick on /dev/pts/8
    """
    Example2:
    <165>1 2019-07-11T22:14:15.000003-07:00 192.0.2.1 myproc 8710 - - %% It's time to make the do-nuts.
    """
    PRI -- 165
    VERSION -- 1
    TIMESTAMP -- 2019-07-11T05:14:15.000003-07:00
    HOSTNAME -- 192.0.2.1
    APP-NAME -- myproc
    PROCID -- 8710
    STRUCTURED-DATA -- “-”
    MSGID -- “-”
    MESSAGE -- "%% It's time to make the do-nuts."
    """
    Example3: - with STRUCTURED-DATA
    <165>1 2019-07-11T22:14:15.003Z aliyun.example.com
               evntslog - ID47 [exampleSDID@32473 iut="3" eventSource=
               "Application" eventID="1011"] BOMAn application
               event log entry...
    """
    PRI -- 165
    VERSION -- 1
    TIMESTAMP -- 2019-07-11T22:14:15.003Z
    HOSTNAME -- aliyun.example.com
    APP-NAME -- evntslog
    PROCID -- "-"
    MSGID -- ID47
    STRUCTURED-DATA -- [exampleSDID@32473 iut="3" eventSource="Application" eventID="1011"]
    MESSAGE -- An application event log entry...
    ```

-   RFC3164协议

    RFC3164协议包含以下字段信息，具体信息请参见[官方协议](https://tools.ietf.org/html/rfc3164)。

    ``` {#codeblock_brz_t0a_y8j}
    PRI HEADER[TIME HOSTNAME] MSG
    ```

    通过以下示例来对以上字段进行说明：

    ``` {#codeblock_fux_okg_qzb}
    """
    <30>Oct 9 22:33:20 hlfedora auditd[1787]: The audit daemon is exiting.
    """
    PRI -- 30
    HEADER
    - TIME -- Oct 9 22:33:20
    - HOSTNAME -- hlfedora
    MSG
    - TAG -- auditd[1787]
    - Content --The audit daemon is exiting.
    ```


## 解决方案 {#section_5t0_iwn_cxg .section}

GROK解析Syslog常见格式

使用GROK对几种常用格式的Syslog进行解析。Syslog采集到Logstore上需要使用文本格式采集。具体的GROK规则请参见[GROK模式参考](cn.zh-CN/数据加工/数据加工语法/通用参考/GROK模式参考.md#)。

-   TraditionalFormat格式
    -   日志内容

        ``` {#codeblock_a5n_i9d_brg}
        May  5 10:20:57 iZbp1a65x3r1vhpe94fi2qZ systemd: Started System Logging Service.
        ```

    -   LOG DSL规则

        ``` {#codeblock_avl_762_g7f}
        """
        处理前日志格式：
          receive_time: 1558663265
          __topic__:
          content: May  5 10:20:57 iZbp1a65x3r1vhpe94fi2qZ systemd: Started System Logging Service.
        处理后日志格式：
          receive_time: 1558663265
          __topic__:
          content: May  5 10:20:57 iZbp1a65x3r1vhpe94fi2qZ systemd: Started System Logging Service.
          timestamp: May  5 10:20:57
          logsource: iZbp1a65x3r1vhpe94fi2qZ
          program: systemd
          message: Started System Logging Service.
        """
        e_regex('content', grok('%{SYSLOGBASE} %{GREEDYDATA:message}'))
        ```

-   FileFormat格式
    -   日志内容

        ``` {#codeblock_2kd_ql4_og1}
        2019-05-06T09:26:07.874593+08:00 iZbp1a65x3r1vhpe94fi2qZ root: 834753
        ```

    -   LOG DSL规则

        ``` {#codeblock_khl_rsu_l7u}
        """
        处理前日志格式：
          receive_time: 1558663265
          __topic__:
          content: 2019-05-06T09:26:07.874593+08:00 iZbp1a65x3r1vhpe94fi2qZ root: 834753
        处理后日志格式：
          receive_time: 1558663265
          __topic__:
          content: 2019-05-06T09:26:07.874593+08:00 iZbp1a65x3r1vhpe94fi2qZ root: 834753
          timestamp: 2019-05-06T09:26:07.874593+08:00
          hostname: iZbp1a65x3r1vhpe94fi2qZ
          program: root
          message: 834753
        """
        e_regex('content',grok('%{TIMESTAMP_ISO8601:timestamp} %{SYSLOGHOST:hostname} %{SYSLOGPROG} %{GREEDYDATA:message}'))
        ```

-   RSYSLOG\_SyslogProtocol23Format格式
    -   日志内容

        ``` {#codeblock_dsl_djp_xig}
        <13>1 2019-05-06T11:50:16.015554+08:00 iZbp1a65x3r1vhpe94fi2qZ root - - - twish
        ```

    -   LOG DSL规则

        ``` {#codeblock_0zj_1vz_kja}
        """
        处理前日志格式：
          receive_time: 1558663265
          __topic__:
          content: <13>1 2019-05-06T11:50:16.015554+08:00 iZbp1a65x3r1vhpe94fi2qZ root - - - twish
        处理后日志格式：
          receive_time: 1558663265
          __topic__:
          content: <13>1 2019-05-06T11:50:16.015554+08:00 iZbp1a65x3r1vhpe94fi2qZ root - - - twish
          priority: 13
          version: 1
          timestamp: 2019-05-06T11:50:16.015554+08:00
          hostname: iZbp1a65x3r1vhpe94fi2qZ
          program: root
          message: twish
        """
        e_regex('content',grok('%{POSINT:priority}>%{NUMBER:version} %{TIMESTAMP_ISO8601:timestamp} %{syslogHOST:hostname} %{PROG:program} - - - %{GREEDYDATA:message}'))
        ```

-   RSYSLOG\_DebugFormat格式
    -   日志内容

        ``` {#codeblock_kym_wqd_j9p}
        2019-05-06T14:29:37.558854+08:00 iZbp1a65x3r1vhpe94fi2qZ root: environment
        ```

    -   LOG DSL规则

        ``` {#codeblock_oar_fe8_s77}
        """
        处理前日志格式：
          receive_time: 1558663265
          __topic__:
          content: 2019-05-06T14:29:37.558854+08:00 iZbp1a65x3r1vhpe94fi2qZ root: environment
        处理后日志格式：
          receive_time: 1558663265
          __topic__:
          content: 2019-05-06T14:29:37.558854+08:00 iZbp1a65x3r1vhpe94fi2qZ root: environment
          timestamp: 2019-05-06T14:29:37.558854+08:00 
          hostname: iZbp1a65x3r1vhpe94fi2qZ
          program: root
          message: environment
        """
        e_regex('content',grok('%{TIMESTAMP_ISO8601:timestamp} %{SYSLOGHOST:hostname} %{SYSLOGPROG} %{GREEDYDATA:message}'))
        ```


以上Syslog配置文件常见日志文件格式已解析完成，下面介绍两种FluentSyslog日志格式。

-   FluentRFC5424格式
    -   日志内容

        ``` {#codeblock_gk8_fxy_7ex}
        <16>1 2013-02-28T12:00:00.003Z 192.168.0.1 fluentd 11111 ID24224 [exampleSDID@20224 iut='3' eventSource='Application' eventID='11211] Hi, from Fluentd!
        ```

    -   LOG DSL规则

        ``` {#codeblock_0wp_3pk_ys4}
        """
        处理前日志格式：
          receive_time: 1558663265
          __topic__:
          content: <16>1 2019-02-28T12:00:00.003Z 192.168.0.1 aliyun 11111 ID24224 [exampleSDID@20224 iut='3' eventSource='Application' eventID='11211] Hi, from Fluentd!
        处理后日志格式：
          receive_time: 1558663265
          __topic__:
          content: <16>1 2019-02-28T12:00:00.003Z 192.168.0.1 aliyun 11111 ID24224 [exampleSDID@20224 iut='3' eventSource='Application' eventID='11211] Hi, from aliyun!
          priority: 16
          version: 1
          timestamp: 2019-02-28T12:00:00.003Z
          hostname: 192.168.0.1
          ident: aliyun
          pid: 1111
          msgid: ID24224
          extradata: [exampleSDID@20224 iut='3' eventSource='Application' eventID='11211]
          message: Hi, from aliyun!
        """
        e_regex('content',grok('%{POSINT:priority}>%{NUMBER:version} %{TIMESTAMP_ISO8601:timestamp} %{SYSLOGHOST:hostname} %{WORD:ident} %{USER:pid} %{USERNAME:msgid} (?P<extradata>(\[(.*)\]|[^ ])) %{GREEDYDATA:message}'))
        ```

-   FluentRFC3164格式
    -   日志内容

        ``` {#codeblock_0ho_dft_0z4}
        <6>Feb 28 12:00:00 192.168.0.1 fluentd[11111]: [error] Syslog test
        ```

    -   LOG DSL规则

        ``` {#codeblock_xyb_8wj_od5}
        """
        处理前日志格式：
          receive_time: 1558663265
          __topic__:
          content: <6>Feb 28 12:00:00 192.168.0.1 aliyun[11111]: [error] Syslog test
        处理后日志格式：
          receive_time: 1558663265
          __topic__:
          content: <6>Feb 28 12:00:00 192.168.0.1 aliyun[11111]: [error] Syslog test
          priority: 6
          timestamp: Feb 28 12:00:00
          hostname: 192.168.0.1
          ident: aliyun
          pid: [1111]
          level: [error]
          message: Syslog test
        """
        e_regex('content', grok('%{POSINT:priority}>%{SYSLOGTIMESTAMP:timestamp} %{SYSLOGHOST:hostname} %{WORD:ident}(?P<pid>(\[[a-zA-Z0-9._-]+\]|[^:])): (?P<level>(\[(\w+)\]|[^ ])) %{GREEDYDATA:message}'))
        ```


以上解析过后的日志内容，还可以对priority进一步解析，并且匹配解析出来的facility和serverity信息，关于使用RFC5424协议更多内容请参见[e\_syslogrfc](../../../../cn.zh-CN/数据加工/数据加工语法/全局操作函数/字段值提取函数.md#section_fsa_oy2_ye3)。示例如下：

``` {#codeblock_1p7_l6j_5p3}
"""
处理前日志格式：
  receive_time: 1558663265
  __topic__:
  content: <13>1 2019-05-06T11:50:16.015554+08:00 iZbp1a65x3r1vhpe94fi2qZ root - - - twish
  priority: 13
  version: 1
  timestamp: 2019-05-06T11:50:16.015554+08:00
  hostname: iZbp1a65x3r1vhpe94fi2qZ
  program: root
  message: twish
处理后日志格式：
  receive_time: 1558663265
  __topic__:
  content: <13>1 2019-05-06T11:50:16.015554+08:00 iZbp1a65x3r1vhpe94fi2qZ root - - - twish
  priority: 13
  version: 1
  timestamp: 2019-05-06T11:50:16.015554+08:00
  hostname: iZbp1a65x3r1vhpe94fi2qZ
  program: root
  message: twish
  _facility_: 1
  _severity_: 5
  _severitylabel_: Notice: normal but significant condition
  _facilitylabel_: user-level messages
"""
e_syslogrfc("priority","SYSLOGRFC5424")
```

