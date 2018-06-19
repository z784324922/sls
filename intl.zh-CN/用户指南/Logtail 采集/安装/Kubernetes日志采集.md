# Kubernetes日志采集 {#concept_fy3_mrv_vdb .concept}

日志服务支持通过Logtail采集Kubernetes集群日志，并支持CRD（CustomResourceDefinition）进行采集配置管理。本文主要介绍如何安装并使用Logtail采集Kubernetes集群日志。

## 配置流程 {#section_npn_jgq_pdb .section}

 ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13059/2684_zh-CN.png "配置流程") 

1.  执行安装命令，安装alibaba-log-controller Helm包。
2.  根据您的需求选择使用CRD（CustomResourceDefinition）或控制台进行采集配置管理。

## 视频教程 {#section_dvn_rrv_vdb .section}

[https://cloud.video.taobao.com/play/u/3220778205/p/1/e/6/t/1/50076466637.mp4](https://cloud.video.taobao.com/play/u/3220778205/p/1/e/6/t/1/50076466637.mp4)

## 步骤1 安装 {#section_ixs_qgq_pdb .section}

## 阿里云容器服务Kubernetes安装方式 {#section_lnl_jpr_zdb .section}

**安装步骤**

1.  登录您的阿里云容器服务Kubernetes的Master节点。登录方式请参考[SSH访问Kubernetes集群](../../../../intl.zh-CN/用户指南/Kubernetes 集群/集群管理/SSH访问Kubernetes集群.md)。
2.  将下述命令中的`${your_k8s_cluster_id}`替换为您的Kubernetes集群id，并执行此命令。

    ```
    wget http://logtail-release.oss-cn-hangzhou.aliyuncs.com/linux64/alicloud-log-k8s-install.sh -O alicloud-log-k8s-install.sh; chmod 744 ./alicloud-log-k8s-install.sh; sh ./alicloud-log-k8s-install.sh ${your_k8s_cluster_id}
    ```

    安装后，日志服务会自动创建和您的Kubernetes集群处于同一region的日志服务Project，Project名称为`k8s-log-${your_k8s_cluster_id}`；同时会在该Project下创建机器组，机器组名为`k8s-group-${your_k8s_cluster_id}`。

    **说明：** Project `k8s-log-${your_k8s_cluster_id}`下会自动创建名为`config-operation-log`的Logstore，请不要删除此Logstore。


**安装示例**

示例如下，执行成功后将会输出以下内容：

```
[root@iZbp******biaZ ~]# wget http://logtail-release.oss-cn-hangzhou.aliyuncs.com/linux64/alicloud-log-k8s-install.sh -O alicloud-log-k8s-install.sh; chmod 744 ./alicloud-log-k8s-install.sh; sh ./alicloud-log-k8s-install.sh c12ba20**************86939f0b
....
....
....
alibaba-cloud-log/Chart.yaml
alibaba-cloud-log/templates/
alibaba-cloud-log/templates/_helpers.tpl
alibaba-cloud-log/templates/alicloud-log-crd.yaml
alibaba-cloud-log/templates/logtail-daemonset.yaml
alibaba-cloud-log/templates/NOTES.txt
alibaba-cloud-log/values.yaml
NAME:   alibaba-log-controller
LAST DEPLOYED: Wed May 16 18:43:06 2018
NAMESPACE: default
STATUS: DEPLOYED
RESOURCES:
==> v1beta1/ClusterRoleBinding
NAME                    AGE
alibaba-log-controller  0s
==> v1beta1/DaemonSet
NAME     DESIRED  CURRENT  READY  UP-TO-DATE  AVAILABLE  NODE SELECTOR  AGE
logtail  2        2        0      2           0          <none>         0s
==> v1beta1/Deployment
NAME                    DESIRED  CURRENT  UP-TO-DATE  AVAILABLE  AGE
alibaba-log-controller  1        1        1           0          0s
==> v1/Pod(related)
NAME                                     READY  STATUS             RESTARTS  AGE
logtail-ff6rf                            0/1    ContainerCreating  0         0s
logtail-q5s87                            0/1    ContainerCreating  0         0s
alibaba-log-controller-7cf6d7dbb5-qvn6w  0/1    ContainerCreating  0         0s
==> v1/ServiceAccount
NAME                    SECRETS  AGE
alibaba-log-controller  1        0s
==> v1beta1/CustomResourceDefinition
NAME                                   AGE
aliyunlogconfigs.log.alibabacloud.com  0s
==> v1beta1/ClusterRole
alibaba-log-controller  0s
[SUCCESS] install helm package : alibaba-log-controller success.
```

您可以使用`helm status alibaba-log-controller`查看Pod当前状态，若状态全部成功后，表示安装成功。

安装成功后登陆日志服务控制台，即可看到已经自动创建出的日志服务Project（若您的Project数过多，可以搜索`k8s-log`关键字）。

## 自建Kubernetes安装方式 {#section_kdx_bqr_zdb .section}

**限制说明**

1.  Kubernetes集群版本1.8及以上。
2.  已经安装Helm命令，版本2.6.4及以上。

**安装步骤**

1.  在日志服务控制台创建一个Project，Project名称以`k8s-log-custom-`开头。
2.  将下述命令中的参数替换，并执行此命令。

    ```
    wget http://logtail-release.oss-cn-hangzhou.aliyuncs.com/linux64/alicloud-log-k8s-custom-install.sh -O alicloud-log-k8s-custom-install.sh; chmod 744 ./alicloud-log-k8s-custom-install.sh; sh ./alicloud-log-k8s-custom-install.sh {your-project-suffix} {region-id} {aliuid} {access-key-id} {access-key-secret}
    ```

    各参数及其说明如下：

    |参数|说明|
    |--|--|
    |\{your-project-suffix\}|您在第二步创建的Project名称的`k8s-log-custom-`之后部分。例如创建的Project为`k8s-log-custom-xxxx`，这边填写`xxxx`。|
    |\{regionId\}|您的Project所在区域的Region Id，请在[服务入口](../../../../intl.zh-CN/API 参考/服务入口.md)中查找，例如`华东 1 (杭州)`的Region Id为`cn-hangzhou`。|
    |\{aliuid\}|用户标识，请替换为您的阿里云主账号用户ID。主账号用户ID为字符串形式，如何查看ID请参考[用户标识](intl.zh-CN/用户指南/Logtail 采集/机器组/非本人ECS（或线下机器）.md)配置中的2.1节。|
    |\{access-key-id\}|您的账号access key id。推荐使用子账号access key，并授予AliyunLogFullAccess权限，具体设置参考[授权-简介](intl.zh-CN/用户指南/访问控制 RAM/授权-简介.md)。|
    |\{access-key-secret\}|您的账号access key secret。推荐使用子账号access key，并授予AliyunLogFullAccess权限，具体设置参考[授权-简介](intl.zh-CN/用户指南/访问控制 RAM/授权-简介.md)。|

    安装好之后，日志服务会自动在该Project下创建机器组，机器组名为`k8s-group-${your_k8s_cluster_id}`。

    **说明：** 

    -   Project下会自动创建名为`config-operation-log`的Logstore，请不要删除此Logstore。
    -   自建Kubernetes安装时，默认为Logtail授予`privileged`权限，主要为避免删除其他POD时可能出现错误`container text file busy`。相关说明请参考：[Bug 1468249](https://bugzilla.redhat.com/show_bug.cgi?spm=a2c4g.11186623.2.10.QhaVGc&id=1468249)、[Bug 1441737](https://bugzilla.redhat.com/show_bug.cgi?spm=a2c4g.11186623.2.11.QhaVGc&id=1441737)和 [issue 34538](https://github.com/moby/moby/issues/34538?spm=a2c4g.11186623.2.12.QhaVGc)。

**安装示例**

示例如下，执行成功后将会输出以下内容：

```
[root@iZbp1dsxxxxxqfbiaZ ~]#  wget http://logtail-release.oss-cn-hangzhou.aliyuncs.com/linux64/alicloud-log-k8s-custom-install.sh -O alicloud-log-k8s-custom-install.sh; chmod 744 ./alicloud-log-k8s-custom-install.sh; sh ./alicloud-log-k8s-custom-install.sh xxxx cn-hangzhou 165xxxxxxxx050 LTAxxxxxxxxxxx AIxxxxxxxxxxxxxxxxxxxxxxxxxxxxxe
....
....
....
NAME:   alibaba-log-controller
LAST DEPLOYED: Fri May 18 16:52:38 2018
NAMESPACE: default
STATUS: DEPLOYED
RESOURCES:
==> v1beta1/ClusterRoleBinding
NAME                    AGE
alibaba-log-controller  0s
==> v1beta1/DaemonSet
NAME        DESIRED  CURRENT  READY  UP-TO-DATE  AVAILABLE  NODE SELECTOR  AGE
logtail-ds  2        2        0      2           0          <none>         0s
==> v1beta1/Deployment
NAME                    DESIRED  CURRENT  UP-TO-DATE  AVAILABLE  AGE
alibaba-log-controller  1        1        1           0          0s
==> v1/Pod(related)
NAME                                     READY  STATUS             RESTARTS  AGE
logtail-ds-7xf2d                         0/1    ContainerCreating  0         0s
logtail-ds-9j4bx                         0/1    ContainerCreating  0         0s
alibaba-log-controller-796f8496b6-6jxb2  0/1    ContainerCreating  0         0s
==> v1/ServiceAccount
NAME                    SECRETS  AGE
alibaba-log-controller  1        0s
==> v1beta1/CustomResourceDefinition
NAME                                   AGE
aliyunlogconfigs.log.alibabacloud.com  0s
==> v1beta1/ClusterRole
alibaba-log-controller  0s
[INFO] your k8s is using project : k8s-log-custom-xxx, region : cn-hangzhou, aliuid : 1654218965343050, accessKeyId : LTAxxxxxxxxxxx
[SUCCESS] install helm package : alibaba-log-controller success.
```

您可以使用`helm status alibaba-log-controller`查看Pod当前状态，若状态全部成功后，表示安装成功。

安装成功后登陆日志服务控制台，即可看到已经自动创建出的日志服务Project（若您的Project数过多，可以搜索`k8s-log`关键字）。

## 步骤二 配置 {#section_axv_ghq_pdb .section}

日志采集配置默认支持控制台配置方式，同时针对Kubernetes微服务开发模式，我们还提供CRD的配置方式，您可以直接使用kubectl对配置进行管理。以下是对两种配置方式进行的比较：

|-|CRD方式|控制台方式|
|:-|:----|:----|
|操作复杂度|低|一般|
|功能项|支持除控制台方式外的高级配置|一般|
|上手难度|一般|低|
|网络连接|连接Kubernetes集群|连接互联网|
|与组件部署集成|支持|不支持|
|鉴权方式|Kubernetes鉴权|云账号鉴权|

推荐您使用CRD方式进行采集配置管理，该方式与Kubernetes部署、发布流程的集成更加完善。

## 通过控制台管理采集配置 {#section_mnv_fsv_vdb .section}

请根据您的需求在控制台创建Logtail采集配置，采集配置步骤请参考：

-   [容器内文本文件（推荐）](intl.zh-CN/用户指南/Logtail 采集/数据源/容器文本日志.md)
-   [容器标准输出（推荐）](intl.zh-CN/用户指南/Logtail 采集/数据源/容器标准输出.md)

-   [宿主机文本文件](intl.zh-CN/用户指南/Logtail 采集/数据源/文本日志.md)

    默认宿主机根目录挂载到Logtail容器的`/logtail_host`目录，配置路径时，您需要加上此前缀。例如需要采集宿主机上`/home/logs/app_log/`目录下的数据，配置页面中日志路径设置为`/logtail_host/home/logs/app_log/`。

-   [Syslog](intl.zh-CN/用户指南/Logtail 采集/数据源/Syslog.md)

## 通过CRD管理采集配置 {#section_ff4_drr_zdb .section}

**实现原理**

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13059/2686_zh-CN.png "实现原理")

执行安装命令时会自动安装`alibaba-log-controller`的Helm包。Helm包中主要执行了以下操作：

1.  创建aliyunlogconfigs CRD（CustomResourceDefinition）。
2.  部署alibaba-log-controller的Deployment。
3.  部署Logtail的DaemonSet。

具体配置的内部工作流程如下：

1.  用户使用`kubectl`或其他工具应用aliyunlogconfigs CRD配置。
2.  alibaba-log-controller监听到配置更新。
3.  alibaba-log-controller根据CRD内容以及服务端状态，自动向日志服务提交logstore创建、配置创建以及应用机器组的请求。
4.  以DaemonSet模式运行的Logtail会定期请求配置服务器，获取新的或已更新的配置并进行热加载。
5.  Logtail根据配置信息采集各个容器\(POD\)上的标准输出或文件。
6.  最终Logtail将处理、聚合好的数据发送到日志服务。

**使用方式**

**说明：** 如果您之前使用的DaemonSet方式部署的日志服务Logtail，将无法使用CRD的方式进行配置管理。升级方式参见本文档中**DaemonSet部署方式迁移步骤**。

您只需要定义AliyunLogConfig的CRD即可实现配置的创建，删除对应的CRD资源即可删除对应配置。CRD配置格式如下：

```
apiVersion: log.alibabacloud.com/v1alpha1      ## 默认值，无需改变
kind: AliyunLogConfig                          ## 默认值，无需改变
metadata:
  name: simple-stdout-example                  ## 资源名，必须在集群内唯一
spec:
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

**查看配置**

您可以通过Kubernetes CRD或控制台查看配置。

控制台查看配置参见[管理采集配置](intl.zh-CN/用户指南/Logtail 采集/机器组/创建 Logtail 配置.md)。

**说明：** 使用CRD管理方式，若您在控制台更改配置，下一次执行CRD更新配置时，会覆盖控制台的更改内容。

-   使用`kubectl get aliyunlogconfigs`查看当前所有配置。

    ```
    [root@iZbp1dsbiaZ ~]# kubectl get aliyunlogconfigs
    NAME                   AGE
    regex-file-example     10s
    regex-stdout-example   4h
    simple-file-example    5s
    ```

-   使用`kubectl get aliyunlogconfigs ${config_name} -o yaml`查看详细配置和状态。

    配置中`status`字段显示配置执行的结果，若配置应用成功，`status`字段中的`statusCode`为200。若`statusCode`非200，则说明配置应用失败。

    ```
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


**配置示例**

**容器标准输出**

容器标准输出中，需要将`inputType`设置为`plugin`，并将具体信息填写到`inputDetail`下的`plugin`字段，详细配置字段及其含义请参考[容器标准输出](intl.zh-CN/用户指南/Logtail 采集/数据源/容器标准输出.md)。

**极简采集方式**

采集除了环境变量中配置`COLLECT_STDOUT_FLAG=false`之外所有容器的标准输出\(stdout和stderr\)。

```
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

**自定义处理采集方式**

采集grafana的access log，并将access log解析成结构化数据。

grafana的容器配置中包含环境变量为：`GF_INSTALL_PLUGINS=grafana-piechart-....`，通过配置`IncludeEnv`为`GF_INSTALL_PLUGINS: ''`指定Logtail只采集该容器的标准输出。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13059/2687_zh-CN.png "自定义处理采集方式")

grafana的access log格式如下：

```
t=2018-03-09T07:14:03+0000 lvl=info msg="Request Completed" logger=context userId=0 orgId=0 uname= method=GET path=/ status=302 remote_addr=172.16.64.154 time_ms=0 size=29 referer=
```

使用正则表达式解析access log，具体配置如下：

```
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
              InluceEnv:
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

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13059/2689_zh-CN.png "采集到的日志数据")

**容器文件**

-   **极简文件**

    采集环境变量配置中含有key为`ALIYUN_LOGTAIL_USER_DEFINED_ID`的容器内日志文件，文件所处的路径为`/data/logs/app_1`，文件名为`simple.LOG`。

    ```
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

-   **完整正则模式文件**

    对于某Java程序日志样例为：

    ```
    [2018-05-11T20:10:16,000] [INFO] [SessionTracker] [SessionTrackerImpl.java:148] Expiring sessions
    java.sql.SQLException: Incorrect string value: '\xF0\x9F\x8E\x8F",...' for column 'data' at row 1
    at org.springframework.jdbc.support.AbstractFallbackSQLExceptionTranslator.translate(AbstractFallbackSQLExceptionTranslator.java:84)
    at org.springframework.jdbc.support.AbstractFallbackSQLException
    ```

    日志中由于包含错误堆栈信息，可能一条日志会被分解成多行，因此需要设置行首正则表达式；为了提取出各个字段，这里我们使用正则表达式进行提取，具体配置如下：

    ```
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

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13059/5237_zh-CN.png "采集到的日志数据")

-   **分隔符模式文件**

    Logtail同时支持分隔符模式的日志解析，示例如下：

    ```
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

-   **JSON模式文件**

    若文件中每行数据为一个JSON object，可以使用JSON方式进行解析，示例如下：

    ```
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


## 其他操作 {#section_ulb_3sv_vdb .section}

## DaemonSet部署方式迁移步骤 {#section_yk4_htr_zdb .section}

如果您之前使用的DaemonSet方式部署的日志服务Logtail，将无法使用CRD的方式进行配置管理。您可以通过以下方式迁移到新的版本：

**说明：** 升级期间会有部分日志重复；CRD配置管理方式只对使用CRD创建的配置生效（由于历史配置使用非CRD方式创建，因此历史配置不支持CRD管理方式）。

1.  按照新版本的方式安装，安装命令最后新增一个参数为您之前Kubernetes集群使用的日志服务Project名。

    例如Project名为`k8s-log-demo`，集群id为`c12ba2028cxxxxxxxxxx6939f0b`，安装命令为：

    ```
    wget
              http://logtail-release.oss-cn-hangzhou.aliyuncs.com/linux64/alicloud-log-k8s-install.sh -O
              alicloud-log-k8s-install.sh; chmod 744 ./alicloud-log-k8s-install.sh; sh
              ./alicloud-log-k8s-install.sh c12ba2028cxxxxxxxxxx6939f0b k8s-log-demo
    ```

2.  安装成功后，进入日志服务控制台，将历史采集配置应用到新的机器组`k8s-group-${your_k8s_cluster_id}`。
3.  一分后，将历史采集配置从历史的机器组中解绑。
4.  观察日志采集正常后，可以选择删除之前安装的logtail daemonset。

## 多集群使用同一个日志服务Project {#section_ewp_ltr_zdb .section}

如果您希望多个集群将日志采集到同一个日志服务Project，您可以在安装其他集群日志服务组件时，将上述安装参数中的`${your_k8s_cluster_id}`替换为您第一次安装的集群ID。

例如您现在有3个集群，ID分别为 abc001、abc002、abc003，三个集群安装组件的参数中`${your_k8s_cluster_id}`都填写为`abc001`。

**说明：** 此方式不支持跨region的Kubernetes多集群共享。

## Logtail容器日志 {#section_olb_4tr_zdb .section}

Logtail日志存储在Logtail容器中的`/usr/local/ilogtail/`目录中，文件名为`ilogtail.LOG`以及`logtail_plugin.LOG`，容器stdout并不具备参考意义，请忽略以下stdout输出：

```
start umount useless mount points, /shm$|/merged$|/mqueue$
umount: /logtail_host/var/lib/docker/overlay2/3fd0043af174cb0273c3c7869500fbe2bdb95d13b1e110172ef57fe840c82155/merged: must be superuser to unmount
umount: /logtail_host/var/lib/docker/overlay2/d5b10aa19399992755de1f85d25009528daa749c1bf8c16edff44beab6e69718/merged: must be superuser to unmount
umount: /logtail_host/var/lib/docker/overlay2/5c3125daddacedec29df72ad0c52fac800cd56c6e880dc4e8a640b1e16c22dbe/merged: must be superuser to unmount
......
xargs: umount: exited with status 255; aborting
umount done
start logtail
ilogtail is running
logtail status:
ilogtail is running
```

## 查看Kubernetes集群中日志相关组件状态 {#section_cx4_ttr_zdb .section}

```
helm status alibaba-log-controller
```

## alibaba-log-controller启动失败 {#section_dx4_ttr_zdb .section}

请确认您是按照以下方式执行的安装：

1.  安装命令再Kubernetes集群的master节点执行
2.  安装命令参数输入的是您的集群ID

若由于以上问题安装失败，请使用`helm del --purge alibaba-log-controller`删除安装包并重新执行安装。

若以上操作依然安装失败，请提交工单联系我们。

## 查看Kubernetes集群中Logtail DaemonSet状态。 {#section_ej5_ytr_zdb .section}

您可以执行命令`kubectl get ds -n kube-system`查看Logtail运行状态。

**说明：** Logtail 默认的namespace为`kube-system`。

## 如何调整Logtail资源限制 {#section_aj5_ytr_zdb .section}

默认Logtail最多占用单核40%CPU和200M的内存，如需提升处理速度，需调整两部分参数：

-   yaml模板中`resources`下的`limits`和`requests`。
-   Logtail启动配置文件，文件路径为yaml模板中的`ALIYUN_LOGTAIL_CONFIG`环境变量，修改方式参见[配置启动参数](intl.zh-CN/用户指南/Logtail 采集/安装/配置启动参数.md)。

## 强制更新Logtail DaemonSet {#section_e2b_d5r_zdb .section}

修改`logtail-daemonset.yaml`文件后，执行如下命令进行更新：

```
kubectl --namespace=kube-system delete ds logtail
kubectl apply -f ./logtail-daemonset.yaml
```

**说明：** 强制更新期间可能会出现数据重复。

## 查看Logtail DaemonSet的配置信息 {#section_b2b_d5r_zdb .section}

```
kubectl describe ds logtail -n kube-system
```

## 查看Logtail的版本号信息、IP、启动时间 {#section_glj_f5r_zdb .section}

示例如下：

```
[root@iZbp1dsu6v77zfb40qfbiaZ ~]# kubectl get po -n kube-system -l k8s-app=logtail
NAME            READY     STATUS    RESTARTS   AGE
logtail-gb92k   1/1       Running   0          2h
logtail-wm7lw   1/1       Running   0          4d
[root@iZbp1dsu6v77zfb40qfbiaZ ~]# kubectl exec logtail-gb92k -n kube-system cat /usr/local/ilogtail/app_info.json
{
   "UUID" : "",
   "hostname" : "logtail-gb92k",
   "instance_id" : "0EBB2B0E-0A3B-11E8-B0CE-0A58AC140402_172.20.4.2_1517810940",
   "ip" : "172.20.4.2",
   "logtail_version" : "0.16.2",
   "os" : "Linux; 3.10.0-693.2.2.el7.x86_64; #1 SMP Tue Sep 12 22:26:13 UTC 2017; x86_64",
   "update_time" : "2018-02-05 06:09:01"
}
```

## 查看Logtail的运行日志 {#section_dlj_f5r_zdb .section}

Logtail运行日志保存在`/usr/local/ilogtail/`目录下，文件名为`ilogtail.LOG`，轮转文件会压缩存储为`ilogtail.LOG.x.gz`。

示例如下：

```
[root@iZbp1dsu6v77zfb40qfbiaZ ~]# kubectl exec logtail-gb92k -n kube-system tail /usr/local/ilogtail/ilogtail.LOG
[2018-02-05 06:09:02.168693] [INFO] [9] [build/release64/sls/ilogtail/LogtailPlugin.cpp:104] logtail plugin Resume:start
[2018-02-05 06:09:02.168807] [INFO] [9] [build/release64/sls/ilogtail/LogtailPlugin.cpp:106] logtail plugin Resume:success
[2018-02-05 06:09:02.168822] [INFO] [9] [build/release64/sls/ilogtail/EventDispatcher.cpp:369] start add existed check point events, size:0
[2018-02-05 06:09:02.168827] [INFO] [9] [build/release64/sls/ilogtail/EventDispatcher.cpp:511] add existed check point events, size:0 cache size:0 event size:0 success count:0

```

## 重启某个pod的Logtail {#section_n4q_h5r_zdb .section}

示例如下：

```
[root@iZbp1dsu6v77zfb40qfbiaZ ~]# kubectl exec logtail-gb92k -n kube-system /etc/init.d/ilogtaild stop
kill process Name: ilogtail pid: 7
kill process Name: ilogtail pid: 9
stop success
[root@iZbp1dsu6v77zfb40qfbiaZ ~]# kubectl exec logtail-gb92k -n kube-system /etc/init.d/ilogtaild start
ilogtail is running

```

