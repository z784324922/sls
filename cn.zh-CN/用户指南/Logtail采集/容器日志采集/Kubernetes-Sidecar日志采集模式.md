# Kubernetes-Sidecar日志采集模式 {#concept_xhp_slg_2gb .concept}

日志服务Logtail支持在Kubernetes集群中通过Sidecar模式采集日志，为每个需要日志采集的业务容器创建一个Sidecar容器用于日志采集，实现多租户隔离和较高的采集性能。

目前日志服务在Kubernetes集群中默认安装的日志组件是DaemonSet模式，此种方式运维简单、资源占用较少、支持采集容器标准输出以及容器文件、配置方式灵活。

但DaemonSet模式下，一个Logtail需要采集该节点所有容器的日志，此种方式会存在一定的性能瓶颈，且各个业务日志之间的隔离性较弱。因此日志服务Logtail新增了对于Sidecar模式的支持，该模式为每个需要日志采集的业务容器创建一个Sidecar容器用于日志采集，多租户隔离性以及性能非常好，建议大型的Kubernetes集群或作为PAAS平台为多个业务方服务的集群使用该方式。

## 主要功能 {#section_sy5_wlg_2gb .section}

-   支持阿里云容器服务Kubernetes版本、ECS上的自建Kubernetes、线下IDC上的自建Kubernetes。
-   支持采集Pod对应的元数据信息，包括：Pod名、Pod IP、Pod Namespace、Pod所在node名、Pod所在node IP。
-   支持通过CRD自动创建日志服务相关资源，包括：Project、Logstore、索引、Logtail配置、机器组等。
-   支持动态扩容，副本数可随时变更，变更后立即生效。

## 技术原理 {#section_b2f_ylg_2gb .section}

Sidecar模式的日志采集依赖Logtail和业务容器共享日志目录，业务容器将日志写入到共享目录中，Logtail通过监控共享目录中日志文件变化并采集日志。详细技术原理请查看社区官方文档：

1.  [Sidecar日志采集介绍](https://kubernetes.io/docs/concepts/cluster-administration/logging/#sidecar-container-with-a-logging-agent)
2.  [Sidecar模式示例](https://kubernetes.io/docs/concepts/workloads/pods/pod-overview/#how-pods-manage-multiple-containers)

## 前提条件 {#section_j2j_vlg_2gb .section}

1.  已经开通日志服务。

    若您未开通日志服务，请单击[开通日志服务](https://sls.console.aliyun.com/)。

2.  若您需要使用CRD（CustomResourceDefinition）进行配置，请提前安装[Kubernetes日志采集流程](cn.zh-CN/用户指南/Logtail采集/容器日志采集/Kubernetes日志采集流程.md)。

## 限制说明 {#section_qtc_zlg_2gb .section}

1.  Logtail必须和业务容器共享日志目录。
2.  Sidecar模式不支持采集容器标准输出。

## Sidecar部署配置 {#section_ay5_1mg_2gb .section}

Sidecar部署配置包括：

1.  [基础运行参数](#)
2.  [挂载路径](#)

Sidecar模式的配置样例如下：

``` {#codeblock_01z_xzw_b5z}
apiVersion: batch/v1
kind: Job
metadata:
  name: nginx-log-sidecar-demo
  namespace: default
spec:
  template:
    metadata:
      name: nginx-log-sidecar-demo
    spec:
      restartPolicy: Never
      containers:
      - name: nginx-log-demo
        image: registry.cn-hangzhou.aliyuncs.com/log-service/docker-log-test:latest
        command: ["/bin/mock_log"]
        args: ["--log-type=nginx", "--stdout=false", "--stderr=true", "--path=/var/log/nginx/access.log", "--total-count=1000000000", "--logs-per-sec=100"]
        volumeMounts:
        - name: nginx-log
          mountPath: /var/log/nginx
      ##### logtail sidecar container
      - name: logtail
        # more info: https://cr.console.aliyun.com/repository/cn-hangzhou/log-service/logtail/detail
        # this images is released for every region 
        image: registry.cn-hangzhou.aliyuncs.com/log-service/logtail:latest
        livenessProbe:
          exec:
            command:
            - /etc/init.d/ilogtaild
            - status
          initialDelaySeconds: 30
          periodSeconds: 30
        resources:
          limits:
            memory: 512Mi
          requests:
            cpu: 10m
            memory: 30Mi
        env:
          ##### base config
          # user id
          - name: "ALIYUN_LOGTAIL_USER_ID"
            value: "${your_aliyun_user_id}"
          # user defined id
          - name: "ALIYUN_LOGTAIL_USER_DEFINED_ID"
            value: "${your_machine_group_user_defined_id}"
          # config file path in logtail's container
          - name: "ALIYUN_LOGTAIL_CONFIG"
            value: "/etc/ilogtail/conf/${your_region_config}/ilogtail_config.json"
          ##### env tags config
          - name: "ALIYUN_LOG_ENV_TAGS"
            value: "_pod_name_|_pod_ip_|_namespace_|_node_name_|_node_ip_"
          - name: "_pod_name_"
            valueFrom:
              fieldRef:
                fieldPath: metadata.name
          - name: "_pod_ip_"
            valueFrom:
              fieldRef:
                fieldPath: status.podIP
          - name: "_namespace_"
            valueFrom:
              fieldRef:
                fieldPath: metadata.namespace
          - name: "_node_name_"
            valueFrom:
              fieldRef:
                fieldPath: spec.nodeName
          - name: "_node_ip_"
            valueFrom:
              fieldRef:
                fieldPath: status.hostIP
        volumeMounts:
        - name: nginx-log
          mountPath: /var/log/nginx
      ##### share this volume
      volumes:
      - name: nginx-log
        emptyDir: {}
```

## 配置1：基础运行参数 {#section_anx_cmg_2gb .section}

该部分主要包括的配置有：

``` {#codeblock_rp6_3p5_lrs}
##### base config
          # user id
          - name: "ALIYUN_LOGTAIL_USER_ID"
            value: "${your_aliyun_user_id}"
          # user defined id
          - name: "ALIYUN_LOGTAIL_USER_DEFINED_ID"
            value: "${your_machine_group_user_defined_id}"
          # config file path in logtail's container
          - name: "ALIYUN_LOGTAIL_CONFIG"
            value: "/etc/ilogtail/conf/${your_region_config}/ilogtail_config.json"
```

|参数|说明|
|:-|:-|
|`${your_region_config}`|该参数由日志服务Project所在Region以及网络类型决定，请根据网络类型输入正确的格式。包括： -   公网：`region-internet`。例如，华东一地域为`cn-hangzhou-internet`。
-   阿里云内网：`region`。例如，华东一地域为`cn-hangzhou`。

 其中，region为[表 1](cn.zh-CN/用户指南/Logtail采集/安装/安装Logtail（Linux系统）.md#table_eyz_pmv_vdb)，请根据Project地域选择正确的参数。|
|`${your_aliyun_user_id}`|用户标识，请替换为您的阿里云**主账号**用户ID。主账号用户ID为字符串形式，如何查看ID请参考[用户标识](cn.zh-CN/用户指南/Logtail采集/机器组/配置主账号AliUid.md)配置中的2.1节。 **说明：** 用户标识一定是**主账号**用户ID，子账号ID没有任何意义。

 |
|`${your_machine_group_user_defined_id}`|您集群的机器组自定义标识。需确保该标识在您的日志服务所在Region内唯一。详细内容可参考[创建用户自定义标识机器组](cn.zh-CN/用户指南/Logtail采集/机器组/创建用户自定义标识机器组.md)。|

## 配置2：挂载路径 {#section_ibr_lmg_2gb .section}

1.  需确保Logtail容器和业务容器挂载相同的目录。
2.  建议使用 `emptyDir` 的挂载方式。

挂载路径的示例请参考上述配置样例。

## 日志采集配置 {#section_jcn_fmg_2gb .section}

日志采集可使用CRD（CustomResourceDefinition）方式或者控制台手工配置。CRD配置可自动创建Project、Logstore、索引、机器组、采集配置等资源，且和Kubernetes集成性较好，推荐使用该方式；控制台配置操作更加简单，适合首次调试和新接触Kubernetes日志采集的用户。

## CRD配置 {#section_yyf_nmg_2gb .section}

CRD详细配置请参考[Kubernetes-CRD配置日志采集](cn.zh-CN/用户指南/Logtail采集/容器日志采集/Kubernetes-CRD配置日志采集.md)，相比DaemonSet采集方式，增加以下限制：

1.  需要设置采集配置的Project名，否则将默认采集到日志组件安装时的project。
2.  需要设置配置应用的机器组，否则将默认应用到DaemonSet所在的机器组。
3.  Sidecar只支持文件采集，文件采集模式中，需把 `dockerFile` 选项设置为false。

具体请参考示例。

## CRD控制台配置 {#section_bdb_4mg_2gb .section}

1.  配置机器组

    如下图所示，在日志服务控制台创建一个Logtail的机器组，机器组选择自定义标识，可以动态适应POD ip地址的改变。具体操作步骤如下：

    1.  开通日志服务并创建Project、Logstore，详细步骤请参考[准备流程](cn.zh-CN/用户指南/准备工作/准备流程.md)。
    2.  在日志服务控制台的机器组列表页面，单击**机器组**后的![机器组](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13080/156410351152484_zh-CN.png)图标，选择**创建机器组**。
    3.  选择用户自定义标识，将您上一步配置的 `ALIYUN_LOGTAIL_USER_DEFINED_ID`填入用户自定义标识内容框中。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/80744/156410351153026_zh-CN.png)

2.  配置采集方式

    机器组创建完成后，即可配置对应文件的采集配置，目前支持极简、Nginx访问日志、分隔符日志、JSON日志、正则日志等格式，具体可参考：[采集文本日志](cn.zh-CN/用户指南/Logtail采集/文本日志/采集文本日志.md)。

    本示例中配置如下：

    **说明：** **是否为Docker文件**选项需要保持关闭。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/80744/156410351153029_zh-CN.png)


## 示例 {#section_fn5_pmg_2gb .section}

示例场景：

1.  Kubernetes为线下IDC上自建集群，日志服务所在Region为华东1（杭州），使用的是公网方式采集。
2.  下述示例中，挂载为 `nginx-log` ，类型为`emptyDir`，分别挂载到nginx-log-demo容器和logtail容器的/var/log/nginx目录下。
3.  配置采集访问日志为/var/log/nginx/access.log ，采集的目的logstore为 `nginx-access`。
4.  配置采集的错误日志为/var/log/nginx/error.log ，采集的目的logstore为 `nginx-error`。

-   **Sidecar配置** 

``` {#codeblock_b9h_ytw_yeh}
apiVersion: batch/v1
kind: Job
metadata:
  name: nginx-log-sidecar-demo
  namespace: default
spec:
  template:
    metadata:
      name: nginx-log-sidecar-demo
    spec:
      restartPolicy: Never
      containers:
      - name: nginx-log-demo
        image: registry.cn-hangzhou.aliyuncs.com/log-service/docker-log-test:latest
        command: ["/bin/mock_log"]
        args: ["--log-type=nginx", "--stdout=false", "--stderr=true", "--path=/var/log/nginx/access.log", "--total-count=1000000000", "--logs-per-sec=100"]
        volumeMounts:
        - name: nginx-log
          mountPath: /var/log/nginx
      ##### logtail sidecar container
      - name: logtail
        # more info: https://cr.console.aliyun.com/repository/cn-hangzhou/log-service/logtail/detail
        # this images is released for every region 
        image: registry.cn-hangzhou.aliyuncs.com/log-service/logtail:latest
        livenessProbe:
          exec:
            command:
            - /etc/init.d/ilogtaild
            - status
          initialDelaySeconds: 30
          periodSeconds: 30
        env:
          ##### base config
          # user id
          - name: "ALIYUN_LOGTAIL_USER_ID"
            value: "xxxxxxxxxx"
          # user defined id
          - name: "ALIYUN_LOGTAIL_USER_DEFINED_ID"
            value: "nginx-log-sidecar"
          # config file path in logtail's container
          - name: "ALIYUN_LOGTAIL_CONFIG"
            value: "/etc/ilogtail/conf/cn-hangzhou-internet/ilogtail_config.json"
          ##### env tags config
          - name: "ALIYUN_LOG_ENV_TAGS"
            value: "_pod_name_|_pod_ip_|_namespace_|_node_name_|_node_ip_"
          - name: "_pod_name_"
            valueFrom:
              fieldRef:
                fieldPath: metadata.name
          - name: "_pod_ip_"
            valueFrom:
              fieldRef:
                fieldPath: status.podIP
          - name: "_namespace_"
            valueFrom:
              fieldRef:
                fieldPath: metadata.namespace
          - name: "_node_name_"
            valueFrom:
              fieldRef:
                fieldPath: spec.nodeName
          - name: "_node_ip_"
            valueFrom:
              fieldRef:
                fieldPath: status.hostIP
        volumeMounts:
        - name: nginx-log
          mountPath: /var/log/nginx
      ##### share this volume
      volumes:
      - name: nginx-log
        emptyDir: {}
```

-   **CRD配置** 

    ``` {#codeblock_hgb_r6c_z5x}
    # config for access log
    apiVersion: log.alibabacloud.com/v1alpha1
    kind: AliyunLogConfig
    metadata:
      # your config name, must be unique in you k8s cluster
      name: nginx-log-access-example
    spec:
      # project name to upload log
      project: k8s-nginx-sidecar-demo
      # logstore name to upload log
      logstore: nginx-access
      # machine group list to apply config, should be same with your sidecar' [ALIYUN_LOGTAIL_USER_DEFINED_ID]
      machineGroups:
      - nginx-log-sidecar
      # logtail config detail
      logtailConfig:
        # log file's input type is 'file'
        inputType: file
        # logtail config name, should be same with [metadata.name]
        configName: nginx-log-access-example
        inputDetail:
          # 极简模式日志，logType设置为"common_reg_log"
          logType: common_reg_log
          # 日志文件夹
          logPath: /var/log/nginx
          # 文件名, 支持通配符，例如log_*.log
          filePattern: access.log
          # sidecar模式，dockerFile设置为false
          dockerFile: false
          # 行首正则表达式，如果为单行模式，设置成 .*
          logBeginRegex: '.*'
          # 解析正则
          regex: '(\S+)\s(\S+)\s\S+\s\S+\s"(\S+)\s(\S+)\s+([^"]+)"\s+(\S+)\s(\S+)\s(\d+)\s(\d+)\s(\S+)\s"([^"]+)"\s.*'
          # 提取出的key列表
          key : ["time", "ip", "method", "url", "protocol", "latency", "payload", "status", "response-size",ser-agent"]
    # config for error log
    ```

    ``` {#codeblock_zzd_6lq_adi}
    # config for error log
    apiVersion: log.alibabacloud.com/v1alpha1
    kind: AliyunLogConfig
    metadata:
      # your config name, must be unique in you k8s cluster
      name: nginx-log-error-example
    spec:
      # project name to upload log
      project: k8s-nginx-sidecar-demo
      # logstore name to upload log
      logstore: nginx-error
      # machine group list to apply config, should be same with your sidecar' [ALIYUN_LOGTAIL_USER_DEFINED_ID]
      machineGroups:
      - nginx-log-sidecar
      # logtail config detail
      logtailConfig:
        # log file's input type is 'file'
        inputType: file
        # logtail config name, should be same with [metadata.name]
        configName: nginx-log-error-example
        inputDetail:
          # 极简模式日志，logType设置为"common_reg_log"
          logType: common_reg_log
          # 日志文件夹
          logPath: /var/log/nginx
          # 文件名, 支持通配符，例如log_*.log
          filePattern: error.log
          # sidecar模式，dockerFile设置为false
          dockerFile: false
    ```

-   **查看日志采集结果** 

    上述示例中配置应用到Kubernetes集群后，Logtail容器将自动创建出对应的Project、Logstore、机器组、配置等资源，并自动将产生的日志采集到日志服务。您可登录日志服务控制台查看。


