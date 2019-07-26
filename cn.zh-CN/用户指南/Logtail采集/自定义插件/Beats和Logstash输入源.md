# Beats和Logstash输入源 {#concept_tk3_gtz_c2b .concept}

Logtail支持Lumberjack协议作为数据输入，可将Beats系列软件（MetricBeat、PacketBeat、Winlogbeat、Auditbeat、Filebeat、Heartbeat等）、Logstash采集的数据转发到日志服务。

## 实现原理 {#section_ryt_ntz_c2b .section}

![实现原理](images/6263_zh-CN.png "实现原理")

基于Logstash、Beats系列软件对Lumberjack协议的支持，日志服务可以通过Logtail，同时对Logstash、Beats系列软件进行Logtail Lumberjack插件监听配置，实现数据采集。

## 配置项 {#section_jqt_4tz_c2b .section}

该输入源类型为: `service_lumberjack`。

**说明：** Logtail支持对采集的数据进行处理加工后上传，处理方式参见[数据处理配置](cn.zh-CN/用户指南/Logtail采集/自定义插件/处理采集数据.md)。

|配置项|类型|是否必须|说明|
|:--|:-|:---|:-|
|BindAddress|string|否|绑定地址，默认为`127.0.0.1:5044`，IP和端口号均可自定义。如需被局域网内其他主机访问，请设置为0.0.0.0:5044。|
|V1|bool|否|Lumberjack V1版本协议，默认为false。目前Logstash支持的协议版本为V1。|
|V2|bool|否|Lumberjack V2版本协议，默认为true。目前Beats系列软件支持的协议版本均为V2。|
|SSLCA|string|否|证书授权机构（Certificate Authority）颁发的签名证书路径，默认为空。如果是自签名证书可不设置此选项。|
|SSLCert|string|否|证书的路径，默认为空。|
|SSLKey|string|否|证书对应私钥的路径，默认为空。|
|InsecureSkipVerify|bool|否|是否跳过SSL安全检查，默认为false。|

## 使用限制 {#section_z2n_wtz_c2b .section}

-   该功能在Logtail 0.16.9及以上版本支持。
-   如需将Logstash采集的数据上传，请参考[Logstash-Lumberjack-Output](https://www.elastic.co/guide/en/logstash/current/plugins-outputs-lumberjack.html)。
-   如需将Beats（MetricBeat、PacketBeat、Winlogbeat、Auditbeat、Filebeat、Heartbeat等）系列软件采集的数据上传，请参考[Beats-Lumberjack-Output](https://www.elastic.co/guide/en/beats/metricbeat/current/logstash-output.html)。
-   同一Logtail可配置多个Lumberjack插件，但多个插件不能监听同一端口。
-   该插件支持SSL，Logstash采集的数据上传需要使用该功能。

## 操作步骤 {#section_ndb_ztz_c2b .section}

使用PacketBeat采集本地网络数据包，并使用Logtail Lumberjack插件上传到日志服务。详细配置如下：

1.  选择输入源。

    单击**接入数据**按钮，并在**接入数据**页面中选择**自定义数据插件**。

2.  选择日志空间。

    可以选择已有的Logstore，也可以新建Project或Logstore。

3.  创建机器组。

    在创建机器组之前，您需要首先确认已经安装了Logtail。 安装完Logtail后单击**确认安装完毕**创建机器组。如果您之前已经创建好机器组 ，请直接单击**使用现有机器组**。

4.  机器组配置。

    选择一个机器组，将该机器组从**源机器组**移动到**应用机器组**。

5.  数据源设置。

    请填写**配置名称**和**插件配置**。

    `inputs`部分为采集配置，是必选项；`processors`部分为处理配置，是可选项，由于Beats和Logstash输出的都是JSON格式数据，因此我们使用`processor_anchor`将json展开。

    关于处理配置部分请参考[处理采集数据](cn.zh-CN/用户指南/Logtail采集/自定义插件/处理采集数据.md)配置一种或多种采集方式。

    ``` {#codeblock_6mx_qms_2vd}
    {
      "inputs": [
        {
          "detail": {
            "BindAddress": "0.0.0.0:5044"
          },
          "type": "service_lumberjack"
        }
      ],
      "processors": [
        {
          "detail": {
            "Anchors": [
              {
                "ExpondJson": true,
                "FieldType": "json",
                "Start": "",
                "Stop": ""
              }
            ],
            "SourceKey": "content"
          },
          "type": "processor_anchor"
        }
      ]
    }
    					
    ```

6.  查询分析配置。

    默认已经设置好索引，您也可以手动进行修改。

7.  配置PacketBeat。

    配置PacketBeat输出方式为`Logstash`，具体配置方式请参见[PacketBeat-Logstash-Output](https://www.elastic.co/guide/en/beats/packetbeat/current/logstash-output.html)。

    在本示例中。配置如下：

    ``` {#codeblock_eg9_vkt_wsh}
    output.logstash:
      hosts: ["127.0.0.1:5044"]
    ```


## 示例 {#section_lkp_25z_c2b .section}

按照以上操作步骤配置处理方式后，可以尝试在本机输入命令`ping 127.0.0.1`，即可在日志服务控制台查看采集处理过的日志数据。

``` {#codeblock_bpf_aik_t93}
_@metadata_beat:  packetbeat
_@metadata_type:  doc
_@metadata_version:  6.2.4
_@timestamp:  2018-06-05T03:58:42.470Z
__source__:  **.**.**.**
__tag__:__hostname__:  *******
__topic__:  
_beat_hostname:  bdbe0b8d53a4
_beat_name:  bdbe0b8d53a4
_beat_version:  6.2.4
_bytes_in:  56
_bytes_out:  56
_client_ip:  192.168.5.2
_icmp_request_code:  0
_icmp_request_message:  EchoRequest(0)
_icmp_request_type:  8
_icmp_response_code:  0
_icmp_response_message:  EchoReply(0)
_icmp_response_type:  0
_icmp_version:  4
_ip:  127.0.0.1
_path:  127.0.0.1
_responsetime:  0
_status:  OK
_type:  icmp
			
```

