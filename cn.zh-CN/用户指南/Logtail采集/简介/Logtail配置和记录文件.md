# Logtail配置和记录文件 {#concept_bxn_h4k_2fb .concept}

Logtail运行时会依赖一系列的配置文件并产生部分信息记录文件，本文档介绍常见文件的基本信息及路径。

**配置文件**：

-   [启动配置文件（ilogtail\_config.json）](#)
-   [AliUid配置文件](#)
-   [用户自定义标识文件（user\_defined\_id）](#)
-   [采集配置文件（user\_log\_config.json）](#)

**记录文件**：

-   [AppInfo记录文件（app\_info.json）](#)
-   [Logtail运行日志（ilogtail.LOG）](#)
-   [Logtail插件日志（logtail\_plugin.LOG）](#)
-   [容器路径映射文件（docker\_path\_config.json）](#)

## 启动配置文件（ilogtail\_config.json） {#section_jh3_dpk_2fb .section}

启动配置文件（ilogtail\_config.json）用来查看或配置Logtail的运行参数，文件类型为JSON。

安装Logtail后，您可以通过该文件：

-   修改Logtail的运行参数。

    可以通过修改启动配置文件来修改CPU使用阈值、常驻内存使用阈值等配置信息。

-   检验安装命令是否正确。

    该文件中的`config_server_address`和`data_server_list`取决于安装时选择的参数和安装命令，如果其中的区域和日志服务所在区域不一致或地址无法联通，说明安装时选择了错误的参数或命令。这时Logtail无法正常采集日志，需要重新安装。


**说明：** 

-   该文件必须为合法JSON，否则无法启动Logtail。
-   修改该文件后需重启Logtail才能生效。

默认配置项如下，您也可以参考[配置启动参数](intl.zh-CN/用户指南/Logtail采集/安装/配置启动参数.md)写入其他配置。

|配置项|说明|
|:--|:-|
|config\_server\_address|Logtail从服务端获取配置文件的地址，取决于安装时选择的参数和安装命令。请保证该地址能够联通，且其中的区域和日志服务所在区域一致。

|
|data\_server\_list|数据服务器地址，取决于安装时选择的参数和安装命令。请保证该地址能够联通，且其中的区域和日志服务所在区域一致。

|
|cluster|区域名称。|
|endpoint|[服务入口](../../../../../intl.zh-CN/API 参考/服务入口.md)。|
|cpu\_usage\_limit|CPU 使用阈值，以单核计算。|
|mem\_usage\_limit|常驻内存使用阈值。|
|max\_bytes\_per\_sec|Logtail 发送原始数据的流量限制，超过20 MB/s则不限流。|
|process\_thread\_count|Logtail 处理日志文件写入数据的线程数。|
|send\_request\_concurrency|异步并发的个数。Logtail 默认异步发送数据包，如果写入TPS很高，可以配置更高的异步并发。|

**文件地址**：

-   Linux：/usr/local/ilogtail/ilogtail\_config.json。
-   容器：该文件存储在Logtail容器中，文件地址配置在Logtail容器的环境变量`ALIYUN_LOGTAIL_CONFIG`中，可通过`docker inspect ${logtail_container_name} | grep ALIYUN_LOGTAIL_CONFIG`查看，例如/etc/ilogtail/conf/cn-hangzhou/ilogtail\_config.json。
-   Windows：
    -   x64：C:\\Program Files \(x86\)\\Alibaba\\Logtail\\ilogtail\_config.json。
    -   x32：C:\\Program Files\\Alibaba\\Logtail\\ilogtail\_config.json。

**文件示例**：

```
$cat /usr/local/ilogtail/ilogtail_config.json
{
    "config_server_address" : "http://logtail.cn-hangzhou-intranet.log.aliyuncs.com",
    "data_server_list" :
    [
        {
            "cluster" : "ap-southeast-2",
            "endpoint" : "cn-hangzhou-intranet.log.aliyuncs.com"
        }
    ],
    "cpu_usage_limit" : 0.4,
    "mem_usage_limit" : 100,
    "max_bytes_per_sec" : 2097152,
    "process_thread_count" : 1,
    "send_request_concurrency" : 4,
    "streamlog_open" : false
}
```

## AliUid配置文件 {#section_f4y_5rk_2fb .section}

AliUid配置文件中包含阿里云账号的AliUid账号信息，主要用于标识这台服务器有权限被该账号访问、采集日志。采集非本账号ECS、自建IDC的日志时，需要手动创建AliUid配置文件。详细说明以及配置参见[为非本账号ECS、自建IDC配置AliUid](intl.zh-CN/用户指南/Logtail采集/机器组/为非本账号ECS、自建IDC配置AliUid.md)。

**说明：** 

-   该文件为可选配置，仅在采集非本账号ECS、自建IDC日志时使用。
-   AliUid文件必须为主账号AliUid，不支持子账号。
-   AliUid文件只需配置文件名即可，文件不能有后缀。
-   一个Logtail可配置多个AliUid文件，Logtail容器仅可配置一个AliUid文件。

**文件地址**

-   Linux：`/etc/ilogtail/users/`。
-   容器：该AliUid直接配置在Logtail容器的环境变量`ALIYUN_LOGTAIL_USER_ID`中，可通过`docker inspect ${logtail_container_name} | grep ALIYUN_LOGTAIL_USER_ID`查看。
-   Windows：`C:\LogtailData\users\`。

**文件示例**

```
$ls /etc/ilogtail/users/
1559122535028493 1329232535020452
```

## 用户自定义标识文件（user\_defined\_id） {#section_cwh_ctk_2fb .section}

用户自定义标识文件（user\_defined\_id）用于配置自定义标识机器组，详细说明以及配置参见[创建用户自定义标识机器组](intl.zh-CN/用户指南/Logtail采集/机器组/创建用户自定义标识机器组.md)。

**说明：** 

-   该文件为可选配置，只有在配置自定义标识机器组时使用。
-   若配置多个自定义标识，使用换行符分隔。

**文件地址**

-   Linux：/etc/ilogtail/user\_defined\_id。
-   容器：该标识直接配置在Logtail容器的环境变量`ALIYUN_LOGTAIL_USER_DEFINED_ID`中，可通过docker inspect $\{logtail\_container\_name\} | grep ALIYUN\_LOGTAIL\_USER\_DEFINED\_ID查看。
-   Windows：C:\\LogtailData\\user\_defined\_id。

**文件示例**

```
$cat /etc/ilogtail/user_defined_id
aliyun-ecs-rs1e16355
```

## 采集配置文件（user\_log\_config.json） {#section_m31_xwk_2fb .section}

该文件记录Logtail从服务端获取的采集配置信息，文件类型为JSON，每次配置更新时会同步更新该文件。可通过该文件确认Logtail配置是否已经下发到该服务器。采集配置文件存在，且内容为最新，表示Logtail配置已下发。

**说明：** 

-   除手动配置密钥信息、数据库密码等敏感信息外，不建议修改该文件。
-   提交工单时，请上传此文件。

**文件地址**

-   Linux：/usr/local/ilogtail/user\_log\_config.json。
-   容器：/usr/local/ilogtail/user\_log\_config.json。
-   Windows
    -   x64：C:\\Program Files \(x86\)\\Alibaba\\Logtail\\user\_log\_config.json。
    -   x32：C:\\Program Files\\Alibaba\\Logtail\\user\_log\_config.json。

**文件示例**

```
$cat /usr/local/ilogtail/user_log_config.json
{
   "metrics" : {
      "##1.0##k8s-log-c12ba2028*****939f0b$app-java" : {
         "aliuid" : "16542189*****50",
         "category" : "app-java",
         "create_time" : 1534739165,
         "defaultEndpoint" : "cn-hangzhou-intranet.log.aliyuncs.com",
         "delay_alarm_bytes" : 0,
         "enable" : true,
         "enable_tag" : true,
         "filter_keys" : [],
         "filter_regs" : [],
         "group_topic" : "",
         "local_storage" : true,
         "log_type" : "plugin",
         "log_tz" : "",
         "max_send_rate" : -1,
         "merge_type" : "topic",
         "plugin" : {
            "inputs" : [
               {
                  "detail" : {
                     "IncludeEnv" : {
                        "aliyun_logs_app-java" : "stdout"
                     },
                     "IncludeLable" : {
                        "io.kubernetes.container.name" : "java-log-demo-2",
                        "io.kubernetes.pod.namespace" : "default"
                     },
                     "Stderr" : true,
                     "Stdout" : true
                  },
                  "type" : "service_docker_stdout"
               }
            ]
         },
         "priority" : 0,
         "project_name" : "k8s-log-c12ba2028c*****ac1286939f0b",
         "raw_log" : false,
         "region" : "cn-hangzhou",
         "send_rate_expire" : 0,
         "sensitive_keys" : [],
         "tz_adjust" : false,
         "version" : 1
      }
   }
}
```

## AppInfo记录文件（app\_info.json） {#section_acx_5tk_2fb .section}

AppInfo记录文件（app\_info.json）记录Logtail的启动时间、获取到的IP地址、hostname等信息。配置[IP地址机器组](intl.zh-CN/用户指南/Logtail采集/机器组/创建IP地址机器组.md)时，需要在该文件中查看Logtail获取到的IP地址。

通常情况下，Logtail根据以下规则获取服务器IP地址：

-   如果已在服务器文件/etc/hosts中设置了主机名与IP地址绑定，则自动获取绑定的IP地址。
-   如果没有设置主机名绑定，会自动获取本机的第一块网卡的IP地址。

**说明：** 

-   AppInfo记录文件仅用于记录Logtail内部信息，手动修改文件内容不能改变Logtail的基本信息。
-   若修改了服务器的hostname等网络配置，请重新启动Logtail以获取新的IP地址。

|字段|说明|
|:-|:-|
|UUID|服务器序列号。|
|hostname|主机名。|
|instance\_id|随机生成的Logtail唯一标识。|
|ip|Logtail获取到的IP地址。该字段为空时表示Logtail没有获取到IP地址，Logtail无法正常运行。请为服务器设置IP地址并重启Logtail。**说明：** 如果机器组为IP地址机器组，请确保机器组中配置的IP与此处显示的IP地址一致。若服务端机器组填写了错误的IP地址，请修改机器组内IP地址并保存，等待1分钟再查看。

|
|logtail\_version|Logtail客户端版本。|
|os|操作系统版本。|
|update\_time|Logtail最近一次启动时间。|

**文件地址**

-   Linux：/usr/local/ilogtail/app\_info.json。
-   容器：/usr/local/ilogtail/app\_info.json。
-   Windows
    -   x64：C:\\Program Files \(x86\)\\Alibaba\\Logtail\\app\_info.json。
    -   x32：C:\\Program Files\\Alibaba\\Logtail\\app\_info.json。

**文件示例**

```
$cat /usr/local/ilogtail/app_info.json
{
   "UUID" : "",
   "hostname" : "logtail-ds-slpn8",
   "instance_id" : "E5F93BC6-B024-11E8-8831-0A58AC14039E_172.20.3.158_1536053315",
   "ip" : "172.20.3.158",
   "logtail_version" : "0.16.13",
   "os" : "Linux; 3.10.0-693.2.2.el7.x86_64; #1 SMP Tue Sep 12 22:26:13 UTC 2017; x86_64",
   "update_time" : "2018-09-04 09:28:36"
}
```

## Logtail运行日志（ilogtail.LOG） {#section_dwy_rwk_2fb .section}

Logtail运行日志（ilogtail.LOG）记录Logtail客户端的运行信息，日志级别从低到高分别为`INFO`、`WARN`和`ERROR`，其中`INFO`类型日志无需关注。

**说明：** 

-   请先[诊断采集错误](intl.zh-CN/常见问题/日志采集/诊断采集错误.md)，根据具体的错误类型和Logtail运行日志排查问题。
-   若因Logtail采集异常提交工单时，请同时上传该日志。

**文件地址**

-   Linux：/usr/local/ilogtail/ilogtail.LOG。
-   容器：/usr/local/ilogtail/ilogtail.LOG。
-   Windows
    -   x64：C:\\Program Files \(x86\)\\Alibaba\\Logtail\\logtail\_\*.log。
    -   x32：C:\\Program Files\\Alibaba\\Logtail\\logtail\_\*.log。

**文件示例**

```
$tail /usr/local/ilogtail/ilogtail.LOG
[2018-09-13 01:13:59.024679]    [INFO]    [3155]    [build/release64/sls/ilogtail/elogtail.cpp:123]    change working dir:/usr/local/ilogtail/
[2018-09-13 01:13:59.025443]    [INFO]    [3155]    [build/release64/sls/ilogtail/AppConfig.cpp:175]    load logtail config file, path:/etc/ilogtail/conf/ap-southeast-2/ilogtail_config.json
[2018-09-13 01:13:59.025460]    [INFO]    [3155]    [build/release64/sls/ilogtail/AppConfig.cpp:176]    load logtail config file, detail:{
   "config_server_address" : "http://logtail.ap-southeast-2-intranet.log.aliyuncs.com",
   "data_server_list" : [
      {
         "cluster" : "ap-southeast-2",
         "endpoint" : "ap-southeast-2-intranet.log.aliyuncs.com"
      }
]
```

## Logtail插件日志（logtail\_plugin.LOG） {#section_p3j_5wk_2fb .section}

Logtail插件日志（logtail\_plugin.LOG）记录容器标准输出、binlog、http等插件的运行信息，日志级别从低到高分别为`INFO`、`WARN`和`ERROR`，其中`INFO`类型日志无需关注。

[诊断采集错误](intl.zh-CN/常见问题/日志采集/诊断采集错误.md)时，若有**CANAL\_RUNTIME\_ALARM**等插件错误，可以根据Logtail插件日志进行排查。

**说明：** 若因插件异常提交工单时，请同时上传该日志。

**文件地址**

-   Linux：/usr/local/ilogtail/logtail\_plugin.LOG。
-   容器：/usr/local/ilogtail/logtail\_plugin.LOG。
-   Windows：不支持插件功能。

**文件示例**

```
$tail /usr/local/ilogtail/logtail_plugin.LOG
2018-09-13 02:55:30 [INF] [docker_center.go:525] [func1] docker fetch all:start
2018-09-13 02:55:30 [INF] [docker_center.go:529] [func1] docker fetch all:stop
2018-09-13 03:00:30 [INF] [docker_center.go:525] [func1] docker fetch all:start
2018-09-13 03:00:30 [INF] [docker_center.go:529] [func1] docker fetch all:stop
2018-09-13 03:03:26 [INF] [log_file_reader.go:221] [ReadOpen] [##1.0##sls-zc-test-hz-pub$docker-stdout-config,k8s-stdout]    open file for read, file:/logtail_host/var/lib/docker/containers/7f46afec6a14de39b59ee9cdfbfa8a70c2fa26f1148b2e2f31bd3410f5b2d624/7f46afec6a14de39b59ee9cdfbfa8a70c2fa26f1148b2e2f31bd3410f5b2d624-json.log    offset:40379573    status:794354-64769-40379963
2018-09-13 03:03:26 [INF] [log_file_reader.go:221] [ReadOpen] [##1.0##k8s-log-c12ba2028cfb444238cd9ac1286939f0b$docker-stdout-config,k8s-stdout]    open file for read, file:/logtail_host/var/lib/docker/containers/7f46afec6a14de39b59ee9cdfbfa8a70c2fa26f1148b2e2f31bd3410f5b2d624/7f46afec6a14de39b59ee9cdfbfa8a70c2fa26f1148b2e2f31bd3410f5b2d624-json.log    offset:40379573    status:794354-64769-40379963
2018-09-13 03:04:26 [INF] [log_file_reader.go:308] [CloseFile] [##1.0##sls-zc-test-hz-pub$docker-stdout-config,k8s-stdout]    close file, reason:no read timeout    file:/logtail_host/var/lib/docker/containers/7f46afec6a14de39b59ee9cdfbfa8a70c2fa26f1148b2e2f31bd3410f5b2d624/7f46afec6a14de39b59ee9cdfbfa8a70c2fa26f1148b2e2f31bd3410f5b2d624-json.log    offset:40379963    status:794354-64769-40379963
2018-09-13 03:04:27 [INF] [log_file_reader.go:308] [CloseFile] [##1.0##k8s-log-c12ba2028cfb444238cd9ac1286939f0b$docker-stdout-config,k8s-stdout]    close file, reason:no read timeout    file:/logtail_host/var/lib/docker/containers/7f46afec6a14de39b59ee9cdfbfa8a70c2fa26f1148b2e2f31bd3410f5b2d624/7f46afec6a14de39b59ee9cdfbfa8a70c2fa26f1148b2e2f31bd3410f5b2d624-json.log    offset:40379963    status:794354-64769-40379963
2018-09-13 03:05:30 [INF] [docker_center.go:525] [func1] docker fetch all:start
2018-09-13 03:05:30 [INF] [docker_center.go:529] [func1] docker fetch all:stop
```

## 容器路径映射文件（docker\_path\_config.json） {#section_xg3_zwk_2fb .section}

容器路径映射文件（docker\_path\_config.json）只有在采集容器文件时才会自动创建，用于记录容器文件和实际文件的路径映射关系。文件类型为JSON。

[诊断采集错误](intl.zh-CN/常见问题/日志采集/诊断采集错误.md)时，如果报错信息为**DOCKER\_FILE\_MAPPING\_ALARM**，表示执行Logtail命令添加Docker文件映射失败，可以通过容器路径映射文件排查问题。

**说明：** 

-   该文件为信息记录文件，任何修改操作均不会生效；删除后会自动创建，不影响业务的正常运行。
-   因容器日志采集异常而提交工单时，请同时在工单中上传此文件。

**文件地址**

/usr/local/ilogtail/docker\_path\_config.json。

**文件示例**

```
$cat /usr/local/ilogtail/docker_path_config.json
{
   "detail" : [
      {
         "config_name" : "##1.0##k8s-log-c12ba2028cfb444238cd9ac1286939f0b$nginx",
         "container_id" : "df19c06e854a0725ea7fca7e0378b0450f7bd3122f94fe3e754d8483fd330d10",
         "params" : "{\n   \"ID\" : \"df19c06e854a0725ea7fca7e0378b0450f7bd3122f94fe3e754d8483fd330d10\",\n   \"Path\" : \"/logtail_host/var/lib/docker/overlay2/947db346695a1f65e63e582ecfd10ae1f57019a1b99260b6c83d00fcd1892874/diff/var/log\",\n   \"Tags\" : [\n      \"nginx-type\",\n      \"access-log\",\n      \"_image_name_\",\n      \"registry.cn-hangzhou.aliyuncs.com/log-service/docker-log-test:latest\",\n      \"_container_name_\",\n      \"nginx-log-demo\",\n      \"_pod_name_\",\n      \"nginx-log-demo-h2lzc\",\n      \"_namespace_\",\n      \"default\",\n      \"_pod_uid_\",\n      \"87e56ac3-b65b-11e8-b172-00163f008685\",\n      \"_container_ip_\",\n      \"172.20.4.224\",\n      \"purpose\",\n      \"test\"\n   ]\n}\n"
      }
   ],
   "version" : "0.1.0"
}
```

