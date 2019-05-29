# Logtail configuration and recording files {#concept_bxn_h4k_2fb .concept}

The running of Logtail depends on a series of configuration files, which generates specific information recording files. This topic describes the basic information and paths of commonly generated files.

**Configuration files**:

-   [Startup configuration file \(ilogtail\_config.json\)](#)
-   [AliUid configuration file](#)
-   [User-defined identity file \(user\_defined\_id\)](#)
-   [Logtail Config file \(user\_log\_config.json\)](#)

 **Recording files**:

-   [AppInfo recording file \(app\_info.json\)](#)
-   [Logtail operational log file \(ilogtail.LOG\)](#)
-   [Logtail plug-in log file \(logtail\_plugin.LOG\)](#)
-   [Container path mapping file \(docker\_path\_config.json\)](#)

## Startup configuration file \(ilogtail\_config.json\) {#section_jh3_dpk_2fb .section}

The file is used to view or set Logtail running parameters. The file is in JSON format.

After installing Logtail, you can use the file to:

-   Modify Logtail running parameters.

    You can modify common settings, such as the CPU usage threshold and resident memory usage threshold by modifying the file.

-   Check whether installation commands are correct.

    In the file, `config_server_address` and `data_server_list` are determined by parameters and commands used during installation. If the region specified by the parameters is different from the region where Log Service resides or the address is inaccessible, incorrect parameters or commands are used during installation. In this case, Logtail cannot collect logs, and you need to reinstall it.


**Note:** 

-   The file must be valid JSON arrays. Otherwise, Logtail cannot be started.
-   The modified file can take effect only after Logtail is restarted.

The following table lists default configuration items. For details about other configuration items, see [Configure startup parameters](reseller.en-US/User Guide/Logtail collection/Install/Configure startup parameters.md).

|Configuration item|Description|
|:-----------------|:----------|
|config\_server\_address|Address of the configuration file Logtail obtains from your server. The address is determined by the parameters and commands you use during installation. The address must be accessible, and the region specified by the parameters must be the same as the region where Log Service resides.

 |
|data\_server\_list|Address of the data server, which is determined by the parameters and commands you use during installation The address must be accessible, and the region specified by the parameters must be the same as the region where Log Service resides.

 |
|cluster|Region name|
|endpoint|[Service endpoint](../../../../reseller.en-US/API Reference/Service endpoint.md)|
|cpu\_usage\_limit|CPU usage threshold, which is calculated by core|
|mem\_usage\_limit|Resident memory usage threshold|
|max\_bytes\_per\_sec|Maximum amount of raw data Logtail can send. The amount will not be limited if the data sending rate exceeds 20 Mbit/s.|
|process\_thread\_count|Number of threads Logtail uses to write data to log files|
|send\_request\_concurrency|Number of data packets Logtail can send concurrently and asynchronously. By default, Logtail sends data packets asynchronously. You can set the configuration item to a larger value if the write TPS is excessively high.|

**File address**:

-   Linux: /usr/local/ilogtail/ilogtail\_config.json
-   Container: The file is stored in the Logtail container, and the file address is configured through the environment variable `ALIYUN_LOGTAIL_CONFIG`. You can view the address through `Docker inspect $ {logtail_container_name} | grep ALIYUN_LOGTAIL_CONFIG`, for example, /Etc/ilogtail/CONF/CN-Hangzhou/FIG.
-   Windows:
    -   x64: C:\\Program Files \(x86\)\\Alibaba\\Logtail\\ilogtail\_config.json
    -   x32: C:\\Program Files\\Alibaba\\Logtail\\ilogtail\_config.json

**File example**:

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

## AliUid configuration file {#section_f4y_5rk_2fb .section}

The file contains the AliUid of your Alibaba Cloud account. AliUid is used to indicate that your Alibaba Cloud account has the permissions to access your server and collect logs. You need to manually create the AliUid configuration file when collecting logs from an ECS instance that does belong to your Alibaba Cloud account or from on-premises IDCs. For more information, see [Configure AliUids for ECS servers under other Alibaba Cloud accounts or on-premises IDCs](reseller.en-US/User Guide/Logtail collection/Machine Group/Configure AliUids for ECS servers under other Alibaba Cloud accounts or on-premises IDCs.md).

**Note:** 

-   This file is optional and is used only when you collect logs from an ECS instance that does belong to your Alibaba Cloud account or from on-premises IDCs.
-   The file can only contain the AliUid of your Alibaba Cloud account. It cannot contain the AliUid of any RAM user account under your Alibaba Cloud account.
-   The file name cannot contain any suffix.
-   Logtail can be configured with multiple AliUid configuration files, but a Logtail container can be configured with only one AliUid configuration file.

**File address** 

-   Linux: `/etc/ilogtail/users/`
-   Container: The file is directly configured through the environment variable `ALIYUN_LOGTAIL_USER_ID` in the Logtail container. You can view the file through `docker inspect ${logtail_container_name} | grep ALIYUN_LOGTAIL_USER_ID`.
-   Windows: `C:\LogtailData\users\`

**File example** 

```
$ls /etc/ilogtail/users/
155912253502**** 132923253502****
```

## User-defined identity file \(user\_defined\_id\) {#section_cwh_ctk_2fb .section}

The file is used to configure machine groups with custom identifiers. For more information, see [Create an ID to identify a machine group](reseller.en-US/User Guide/Logtail collection/Machine Group/Create an ID to identify a machine group.md).

**Note:** 

-   This file is optional and is used only when configuring machine groups with custom identifiers.
-   If multiple custom identifiers are configured for a machine group, they must be separated by delimiters.

**File address** 

-   Linux: /etc/ilogtail/user\_defined\_id
-   Container: The file is directly configured through the environment variable `ALIYUN_LOGTAIL_USER_DEFINED_ID` in the Logtail container. You can view the file through docker inspect $\{logtail\_container\_name\} | grep ALIYUN\_LOGTAIL\_USER\_DEFINED\_ID.
-   Windows: C:\\LogtailData\\user\_defined\_id

**File example** 

```
$cat /etc/ilogtail/user_defined_id
aliyun-ecs-rs1e16355
```

## Logtail Config file \(user\_log\_config.json\) {#section_m31_xwk_2fb .section}

The file contains Logtail Config information Logtail obtains from your server. The file is in JSON format and is updated with Logtail Config updates. The file is used to check whether Logtail Config sends logs to your server. If the file exists and the file content is up-to-date, the Logtail Config has sent logs.

**Note:** 

-   We recommend that you do not modify the file unless you need to manually configure keys and modify database passwords.
-   The file must be uploaded when you open a ticket.

**File address** 

-   Linux: /usr/local/ilogtail/user\_log\_config.json
-   Container: /usr/local/ilogtail/user\_log\_config.json
-   Windows
    -   x64: C:\\Program Files \(x86\)\\Alibaba\\Logtail\\user\_log\_config.json
    -   x32: C:\\Program Files\\Alibaba\\Logtail\\user\_log\_config.json

**File example** 

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

## AppInfo recording file \(app\_info.json\) {#section_acx_5tk_2fb .section}

The file contains various time information, such as the Logtail startup time and the time when Logtail obtains the IP address and host name. The IP address is needed when you configure [machine groups with IP addresses as identifiers](reseller.en-US/User Guide/Logtail collection/Machine Group/Create a machine group with an IP address as its identifier.md).

In normal cases, Logtail obtains the server IP address according to the following rules:

-   Logtail automatically obtains the IP address if the IP address has been attached to your host through the server file /etc/hosts.
-   Logtail automatically obtains the IP address of the first NIC on your host if no IP address is attached to your host.

**Note:** 

-   The file only contains internal information about Logtail. Manual modifications to the file content do not change basic Logtail information.
-   If you have modified network configurations of your server, for example, host name, you need to restart Logtail to obtain the new IP address.

|Field|Description|
|:----|:----------|
|UUID|Server serial number|
|hostname|Host name|
|instance\_id|Randomly generated identifier for uniquely indicating Logtail|
|ip|IP address obtained by Logtail. An empty field indicates that Logtail does not obtain the IP address and cannot function normally. In this case, you need to set an IP address for your server and restart Logtail. **Note:** If the target machine group uses an IP address as an identifier, the IP address configured in the machine group must be the same as the one specified by this field. If an incorrect IP address is configured on your server, you need to modify the IP address within the machine group, wait one minute, and then check again.

 |
|logtail\_version|Version of the Logtail client|
|os|OS version|
|update\_time|Time when Logtail is last started|

 **File address** 

-   Linux: /usr/local/ilogtail/app\_info.json
-   Container: /usr/local/ilogtail/app\_info.json
-   Windows
    -   x64: C:\\Program Files \(x86\)\\Alibaba\\Logtail\\app\_info.json
    -   x32: C:\\Program Files\\Alibaba\\Logtail\\app\_info.json

 **File example** 

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

## Logtail operational log file \(ilogtail.LOG\) {#section_dwy_rwk_2fb .section}

The file contains running information about the Logtail client. Log levels are ranked as follows in ascending order: `INFO`, `WARN`, `ERROR`. `INFO`-type logs can be ignored.

**Note:** 

-   First, you need to [diagnose collection exceptions](reseller.en-US/FAQ/Log collection/Diagnose collection errors.md) and troubleshoot errors according to specific error types and Logtail operational logs.
-   The file must be uploaded when you open a ticket due to Logtail collection exceptions.

 **File address** 

-   Linux: /usr/local/ilogtail/ilogtail.LOG
-   Container: /usr/local/ilogtail/ilogtail.LOG
-   Windows
    -   x64: C:\\Program Files \(x86\)\\Alibaba\\Logtail\\logtail\_\*.log
    -   x32: C:\\Program Files\\Alibaba\\Logtail\\logtail\_\*.log

 **File example** 

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

## Logtail plug-in log file \(logtail\_plugin.LOG\) {#section_p3j_5wk_2fb .section}

The file contains running information about the container stdout, binlogs, http plug-in, and other plug-ins. Log levels are ranked as follows in ascending order: `INFO`, `WARN`, `ERROR`. `INFO`-type logs can be ignored.

If there is any plug-in error, for example, **CANAL\_RUNTIME\_ALARM**, when you [diagnose collection exceptions](reseller.en-US/FAQ/Log collection/Diagnose collection errors.md), you can troubleshoot the error according to Logtail plug-in logs.

**Note:** The file must be uploaded when you open a ticket due to plug-in exceptions.

 **File address** 

-   Linux: /usr/local/ilogtail/logtail\_plugin.LOG
-   Container: /usr/local/ilogtail/logtail\_plugin.LOG
-   Windows: plug-in logs are not supported.

 **File example** 

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

## Container path mapping file \(docker\_path\_config.json\) {#section_xg3_zwk_2fb .section}

The file is automatically created only when container files are collected. The file is used to record the mapping between the path of container files and the actual file path. The file is in JSON format.

When you [diagnose collection exceptions](reseller.en-US/FAQ/Log collection/Diagnose collection errors.md), if an error indicating **DOCKER\_FILE\_MAPPING\_ALARM** is reported, Logtail fails to add Docker file mapping. In this case, you can use the file to troubleshoot the error.

**Note:** 

-   The file only contains information. Any modification to the file does not take effect. The file will be automatically recreated once is deleted. This does not impact services.
-   The file must be uploaded when you open a ticket due to container log collection exceptions.

 **File address** 

/usr/local/ilogtail/docker\_path\_config.json

 **File example** 

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

