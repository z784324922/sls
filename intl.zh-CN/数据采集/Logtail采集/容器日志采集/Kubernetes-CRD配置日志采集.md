# Kubernetes-CRD配置日志采集 {#concept_tfg_pl1_f2b .concept}

配置日志采集默认通过控制台方式配置。针对Kubernetes微服务开发模式，除控制台方式外，日志服务还提供CRD的配置方式日志，您可以直接使用kubectl对配置进行管理。

推荐您使用CRD方式进行采集配置管理，该方式与Kubernetes部署、发布流程的集成更加完善。

## 实现原理 {#section_ff4_drr_zdb .section}

![kub-CRD实现原理](images/2686_zh-CN.png "实现原理")

执行安装命令时会自动安装`alibaba-log-controller`的Helm包。Helm包中主要执行了以下操作：

1.  创建aliyunlogconfigs CRD（CustomResourceDefinition）。
2.  部署alibaba-log-controller的Deployment。
3.  部署Logtail的DaemonSet。

具体配置的内部工作流程如下：

1.  用户使用`kubectl`或其他工具应用aliyunlogconfigs CRD配置。
2.  alibaba-log-controller监听到配置更新。
3.  alibaba-log-controller根据CRD内容以及服务端状态，自动向日志服务提交logstore创建、配置创建以及应用机器组的请求。
4.  以DaemonSet模式运行的Logtail会定期请求配置服务器，获取新的或已更新的配置并进行热加载。
5.  Logtail根据配置信息采集各个容器（POD）上的标准输出或文件。
6.  最终Logtail将处理、聚合好的数据发送到日志服务。

## 配置方式 {#section_ir5_mm1_f2b .section}

**说明：** 如果您之前使用的DaemonSet方式部署的日志服务Logtail，将无法使用CRD的方式进行配置管理。升级方式参见本文档中DaemonSet部署方式迁移步骤。

您只需要定义AliyunLogConfig的CRD即可实现配置的创建，删除对应的CRD资源即可删除对应配置。CRD配置格式如下：

``` {#codeblock_ud9_w75_vi1}
apiVersion: log.alibabacloud.com/v1alpha1      ## 默认值，无需改变
kind: AliyunLogConfig                          ## 默认值，无需改变
metadata:
  name: simple-stdout-example                  ## 资源名，必须在集群内唯一
spec:
  project: k8s-my-project                      ## [可选]Project名，默认为安装时设置的project，若指定project请确保该project没有被别人占用
  logstore: k8s-stdout                         ## Logstore名，不存在时自动创建
  shardCount: 2                                ## [可选] logstore分区数，默认为2，支持1-10
  lifeCycle: 90                                ## [可选] 该logstore存储时间，默认为90，支持1-7300，7300天为永久存储
  logtailConfig:                               ## 详细配置
    inputType: plugin                          ## 采集的输入类型，一般为file或plugin
    configName: simple-stdout-example          ## 采集配置名，需要和资源名(metadata.name)一致
    inputDetail:                               ## 详细配置信息，具体请参考示例
      ...
```

配置完成并应用配置后，会自动创建alibaba-log-controller。

## 查看配置 {#section_zyq_4m1_f2b .section}

您可以通过Kubernetes CRD或控制台查看配置。

控制台查看配置参见[管理采集配置](../../../../cn.zh-CN/数据采集/Logtail采集/机器组/管理采集配置.md#)。

**说明：** 使用CRD管理方式，若您在控制台更改配置，下一次执行CRD更新配置时，会覆盖控制台的更改内容。

-   使用`kubectl get aliyunlogconfigs`查看当前所有配置。

    ``` {#codeblock_yhh_52z_zvo}
    [root@iZbp1dsbiaZ ~]# kubectl get aliyunlogconfigs
    NAME                   AGE
    regex-file-example     10s
    regex-stdout-example   4h
    simple-file-example    5s
    ```

-   使用`kubectl get aliyunlogconfigs ${config_name} -o yaml`查看详细配置和状态。

    配置中`status`字段显示配置执行的结果，若配置应用成功，`status`字段中的`statusCode`为200。若`statusCode`非200，则说明配置应用失败。

    ``` {#codeblock_18u_48h_xz0}
    [root@iZbp1dsbiaZ ~]# kubectl get aliyunlogconfigs simple-file-example -o yaml
    apiVersion: log.alibabacloud.com/v1alpha1
    kind: AliyunLogConfig
    metadata:
    annotations:
      kubectl.kubernetes.io/last-applied-configuration: |
        {"apiVersion":"log.alibabacloud.com/v1alpha1","kind":"AliyunLogConfig","metadata":{"annotations":{},"name":"simple-file-example","namespace":"default"},"spec":{"logstore":"k8s-file","logtailConfig":{"configName":"simple-file-example","inputDetail":{"dockerFile":true,"dockerIncludeEnv":{"ALIYUN_LOGTAIL_USER_DEFINED_ID":""},"filePattern":"simple.LOG","logPath":"/usr/local/ilogtail","logType":"common_reg_log"},"inputType":"file"}}}
    clusterName: ""
    creationTimestamp: 2018-05-17T08:44:46Z
    generation: 0
    name: simple-file-example
    namespace: default
    resourceVersion: "21790443"
    selfLink: /apis/log.alibabacloud.com/v1alpha1/namespaces/default/aliyunlogconfigs/simple-file-example
    uid: 8d3a09c4-59ae-11e8-851d-00163f008685
    spec:
    lifeCycle: null
    logstore: k8s-file
    logtailConfig:
      configName: simple-file-example
      inputDetail:
        dockerFile: true
        dockerIncludeEnv:
          ALIYUN_LOGTAIL_USER_DEFINED_ID: ""
        filePattern: simple.LOG
        logPath: /usr/local/ilogtail
        logType: common_reg_log
      inputType: file
    machineGroups: null
    project: ""
    shardCount: null
    status:
    status: OK
    statusCode: 200
    ```


## 配置示例 {#section_vrk_rm1_f2b .section}

## 容器标准输出 {#section_c3g_wm1_f2b .section}

容器标准输出中，需要将`inputType`设置为`plugin`，并将具体信息填写到`inputDetail`下的`plugin`字段，详细配置字段及其含义请参考[容器标准输出](cn.zh-CN/数据采集/Logtail采集/容器日志采集/容器标准输出.md)。

-   极简采集方式

    采集除了环境变量中配置`COLLECT_STDOUT_FLAG=false`之外所有容器的标准输出（stdout和stderr）。

    ``` {#codeblock_rt9_z02_4s3}
    apiVersion: log.alibabacloud.com/v1alpha1
    kind: AliyunLogConfig
    metadata:
      # your config name, must be unique in you k8s cluster
      name: simple-stdout-example
    spec:
      # logstore name to upload log
      logstore: k8s-stdout
      # logtail config detail
      logtailConfig:
        # docker stdout's input type is 'plugin'
        inputType: plugin
        # logtail config name, should be same with [metadata.name]
        configName: simple-stdout-example
        inputDetail:
          plugin:
            inputs:
              -
                # input type
                type: service_docker_stdout
                detail:
                  # collect stdout and stderr
                  Stdout: true
                  Stderr: true
                  # collect all container's stdout except containers with "COLLECT_STDOUT_FLAG:false" in docker env config
                  ExcludeEnv:
                    COLLECT_STDOUT_FLAG: "false"
    ```

-   自定义处理采集方式

    采集grafana的access log，并将access log解析成结构化数据。

    grafana的容器配置中包含环境变量为：`GF_INSTALL_PLUGINS=grafana-piechart-....`，通过配置`IncludeEnv`为`GF_INSTALL_PLUGINS: ''`指定Logtail只采集该容器的标准输出。

    ![](images/2687_zh-CN.png "自定义处理采集方式")

    grafana的access log格式如下：

    ``` {#codeblock_70p_5zw_naq}
    t=2018-03-09T07:14:03+0000 lvl=info msg="Request Completed" logger=context userId=0 orgId=0 uname= method=GET path=/ status=302 remote_addr=172.16.64.154 time_ms=0 size=29 referer=
    ```

    使用正则表达式解析access log，具体配置如下：

    ``` {#codeblock_5yv_3uo_hhi}
    apiVersion: log.alibabacloud.com/v1alpha1
    kind: AliyunLogConfig
    metadata:
      # your config name, must be unique in you k8s cluster
      name: regex-stdout-example
    spec:
      # logstore name to upload log
      logstore: k8s-stdout-regex
      # logtail config detail
      logtailConfig:
        # docker stdout's input type is 'plugin'
        inputType: plugin
        # logtail config name, should be same with [metadata.name]
        configName: regex-stdout-example
        inputDetail:
          plugin:
            inputs:
              -
                # input type
                type: service_docker_stdout
                detail:
                  # 只采集stdout，不采集stderr
                  Stdout: true
                  Stderr: false
                  # 只采集容器环境变量中配置key为"GF_INSTALL_PLUGINS"的stdout 
                  IncludeEnv:
                    GF_INSTALL_PLUGINS: ''
            processors:
              -
                # 使用正则表达式处理
                type: processor_regex
                detail:
                  # docker 采集的数据默认key为"content"
                  SourceKey: content
                  # 正则表达式提取
                  Regex: 't=(\d+-\d+-\w+:\d+:\d+\+\d+) lvl=(\w+) msg="([^"]+)" logger=(\w+) userId=(\w+) orgId=(\w+) uname=(\S*) method=(\w+) path=(\S+) status=(\d+) remote_addr=(\S+) time_ms=(\d+) size=(\d+) referer=(\S*).*'
                  # 提取出的key
                  Keys: ['time', 'level', 'message', 'logger', 'userId', 'orgId', 'uname', 'method', 'path', 'status', 'remote_addr', 'time_ms', 'size', 'referer']
                  # 保留原始字段
                  KeepSource: true
                  NoKeyError: true
                  NoMatchError: true
    ```

    配置应用后，采集到日志服务的数据如下：

    ![](images/2689_zh-CN.png "采集到的日志数据")


## 容器文件 {#section_qlv_zm1_f2b .section}

-   极简文件

    采集环境变量配置中含有key为`ALIYUN_LOGTAIL_USER_DEFINED_ID`的容器内日志文件，文件所处的路径为`/data/logs/app_1`，文件名为`simple.LOG`。

    ``` {#codeblock_52x_uix_871}
    apiVersion: log.alibabacloud.com/v1alpha1
    kind: AliyunLogConfig
    metadata:
      # your config name, must be unique in you k8s cluster
      name: simple-file-example
    spec:
      # logstore name to upload log
      logstore: k8s-file
      # logtail config detail
      logtailConfig:
        # log file's input type is 'file'
        inputType: file
        # logtail config name, should be same with [metadata.name]
        configName: simple-file-example
        inputDetail:
          # 极简模式日志，logType设置为"common_reg_log"
          logType: common_reg_log
          # 日志文件夹
          logPath: /data/logs/app_1
          # 文件名, 支持通配符，例如log_*.log
          filePattern: simple.LOG
          # 采集容器内的文件，dockerFile flag设置为true
          dockerFile: true
          # only collect container with "ALIYUN_LOGTAIL_USER_DEFINED_ID" in docker env config
          dockerIncludeEnv:
            ALIYUN_LOGTAIL_USER_DEFINED_ID: ""
    ```

-   完整正则模式文件

    对于某Java程序日志样例为：

    ``` {#codeblock_giw_xe6_42v}
    [2018-05-11T20:10:16,000] [INFO] [SessionTracker] [SessionTrackerImpl.java:148] Expiring sessions
    java.sql.SQLException: Incorrect string value: '\xF0\x9F\x8E\x8F",...' for column 'data' at row 1
    at org.springframework.jdbc.support.AbstractFallbackSQLExceptionTranslator.translate(AbstractFallbackSQLExceptionTranslator.java:84)
    at org.springframework.jdbc.support.AbstractFallbackSQLException
    ```

    日志中由于包含错误堆栈信息，可能一条日志会被分解成多行，因此需要设置行首正则表达式；为了提取出各个字段，这里我们使用正则表达式进行提取，具体配置如下：

    ``` {#codeblock_91t_pja_v3s}
    apiVersion: log.alibabacloud.com/v1alpha1
    kind: AliyunLogConfig
    metadata:
      # your config name, must be unique in you k8s cluster
      name: regex-file-example
    spec:
      # logstore name to upload log
      logstore: k8s-file
      logtailConfig:
        # log file's input type is 'file'
        inputType: file
        # logtail config name, should be same with [metadata.name]
        configName: regex-file-example
        inputDetail:
          # 对于正则类型的日志，将logType设置为common_reg_log
          logType: common_reg_log
          # 日志文件夹
          logPath: /app/logs
          # 文件名, 支持通配符，例如log_*.log
          filePattern: error.LOG
          # 行首正则表达式
          logBeginRegex: '\[\d+-\d+-\w+:\d+:\d+,\d+]\s\[\w+]\s.*'
          # 解析正则
          regex: '\[([^]]+)]\s\[(\w+)]\s\[(\w+)]\s\[([^:]+):(\d+)]\s(.*)'
          # 提取出的key列表
          key : ["time", "level", "method", "file", "line", "message"]
          # 正则模式日志，时间解析默认从日志中的`time`提取，如果无需提取时间，可不设置该字段
          timeFormat: '%Y-%m-%dT%H:%M:%S'
          # 采集容器内的文件，dockerFile flag设置为true
          dockerFile: true
          # only collect container with "ALIYUN_LOGTAIL_USER_DEFINED_ID" in docker env config
          dockerIncludeEnv:
            ALIYUN_LOGTAIL_USER_DEFINED_ID: ""
    ```

    配置应用后，采集到日志服务的数据如下：

    ![](images/5237_zh-CN.png "采集到的日志数据")

-   分隔符模式文件

    Logtail同时支持分隔符模式的日志解析，示例如下：

    ``` {#codeblock_epr_32l_n9z}
    apiVersion: log.alibabacloud.com/v1alpha1
    kind: AliyunLogConfig
    metadata:
      # your config name, must be unique in you k8s cluster
      name: delimiter-file-example
    spec:
      # logstore name to upload log
      logstore: k8s-file
      logtailConfig:
        # log file's input type is 'file'
        inputType: file
        configName: delimiter-file-example
        # logtail config name, should be same with [metadata.name]
        inputDetail:
          # 对于分隔符类型的日志，logType设置为delimiter_log
          logType: delimiter_log
          # 日志文件夹
          logPath: /usr/local/ilogtail
          # 文件名, 支持通配符，例如log_*.log
          filePattern: delimiter_log.LOG
          # 使用多字符分隔符
          separator: '|&|'
          # 提取的key列表
          key : ['time', 'level', 'method', 'file', 'line', 'message']
          # 用作解析时间的key，如无需求可不填
          timeKey: 'time'
          # 时间解析方式，如无需求可不填
          timeFormat: '%Y-%m-%dT%H:%M:%S'
          # 采集容器内的文件，dockerFile flag设置为true
          dockerFile: true
          # only collect container with "ALIYUN_LOGTAIL_USER_DEFINED_ID" in docker env config
          dockerIncludeEnv:
            ALIYUN_LOGTAIL_USER_DEFINED_ID: ''
    ```

-   JSON模式文件

    若文件中每行数据为一个JSON object，可以使用JSON方式进行解析，示例如下：

    ``` {#codeblock_sw2_70h_sso}
    apiVersion: log.alibabacloud.com/v1alpha1
    kind: AliyunLogConfig
    metadata:
      # your config name, must be unique in you k8s cluster
      name: json-file-example
    spec:
      # logstore name to upload log
      logstore: k8s-file
      logtailConfig:
        # log file's input type is 'file'
        inputType: file
        # logtail config name, should be same with [metadata.name]
        configName: json-file-example
        inputDetail:
          # 对于分隔符类型的日志，logType设置为json_log
          logType: json_log
          # 日志文件夹
          logPath: /usr/local/ilogtail
          # 文件名, 支持通配符，例如log_*.log
          filePattern: json_log.LOG
          # 用作解析时间的key，如无需求可不填
          timeKey: 'time'
          # 时间解析方式，如无需求可不填
          timeFormat: '%Y-%m-%dT%H:%M:%S'
          # 采集容器内的文件，dockerFile flag设置为true
          dockerFile: true
          # only collect container with "ALIYUN_LOGTAIL_USER_DEFINED_ID" in docker env config
          dockerIncludeEnv:
            ALIYUN_LOGTAIL_USER_DEFINED_ID: ""
    ```


