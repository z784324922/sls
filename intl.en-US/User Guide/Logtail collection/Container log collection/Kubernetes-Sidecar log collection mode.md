# Kubernetes-Sidecar log collection mode {#concept_xhp_slg_2gb .concept}

Logtail can collect logs in Sidecar mode from Kubernetes and create a Sidecar container for each service container requiring log collection, thereby facilitating multi-tenant isolation and improving collection performance.

Currently, the default log component installed in Kubernetes clusters is DaemonSet, which simplifies O&M operations, occupies a few resources, supports collection of container stdout and container files, and can be flexibly configured.

However, in DaemonSet mode, Logtail needs to collect logs from all container on a node. This leads to a bottleneck in performance and does not allow for total isolation among service logs. To resolve the preceding issue, Logtail now provides the Sidecar mode, which enables Logtail to create a Sidecar container for each service container requiring log collection. This mode greatly strengthens multi-tenant isolation and improves the collection performance. We recommend that you use the Sidecar mode for large-scale Kubernetes clusters and for clusters that function as a PaaS platform to serve multiple services.

## Features {#section_sy5_wlg_2gb .section}

-   The Sidecar mode can be applied to Container Service for Kubernetes, on-premises ECS Kubernetes, and on-premises Kubernetes in IDCs.
-   In Sidecar mode, Logtail can collect Pod metadata, including the Pod name, Pod IP address, Pod namespace, and name and IP address of the node to which the Pod belongs.
-   In Sidecar mode, Logtail can automatically create Log Service resources through CustomResourceDefinition \(CRD\), including projects, Logstores, indexes, Logtail Configs, and machine groups.
-   The Sidecar mode supports dynamic scaling. You can adjust the number of replicas at any time, and the changes take effect immediately.

## Concepts {#section_b2f_ylg_2gb .section}

In Sidecar mode, log collection requires that Logtail share the log directory with the service container. Briefly, the service container writes logs into the log directory, and Logtail monitors changes on log files in the log directory and collects logs. For more information, see the following official documents:

1.  [Introduction to the Sidecar log collection mode](https://kubernetes.io/docs/concepts/cluster-administration/logging/#sidecar-container-with-a-logging-agent)
2.  [Example of the Sidecar mode](https://kubernetes.io/docs/concepts/workloads/pods/pod-overview/#how-pods-manage-multiple-containers)

## Prerequisites {#section_j2j_vlg_2gb .section}

1.  You have activated Log Service.

    If you have not activated Log Service, [activate it](https://sls.console.aliyun.com/).

2.  You have installed [Kubernetes log collection](reseller.en-US/User Guide/Logtail collection/Container log collection/Kubernetes log collection.md) for CRD-based settings.

## Limits {#section_qtc_zlg_2gb .section}

1.  Logtail must share the log directory with the service container.
2.  Sidecar mode does not support collection of container stdout.

## Sidecar configuration {#section_ay5_1mg_2gb .section}

The Sidecar configuration involves:

1.  [Setting basic operation parameters](#)
2.  [Setting the mount path](#)

The following is an example:

```
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

## Configuration 1: Set basic operation parameters. {#section_anx_cmg_2gb .section}

The following shows major parameters and their settings:

```
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

|Parameter|Description|
|:--------|:----------|
|`${your_region_config}`|This parameter is determined by the region and network type of the project. Set the parameter to an appropriate value according to the network type. Valid values:-   For the Internet: `region-internet`. For example, the value for the China \(Hangzhou\) region is `cn-hangzhou-internet`.
-   For Alibaba Cloud intranet: `region`. For example, the value for the China \(Hangzhou\) region is `cn-hangzhou`.

In this parameter, region is a [EN-US\_TP\_13056.md\#table\_eyz\_pmv\_vdb](reseller.en-US/User Guide/Logtail collection/Install/Linux.md#table_eyz_pmv_vdb). Set it to the region to which the project belongs.|
|`${your_aliyun_user_id}`|This parameter specifies the user ID, which must be replaced with your **Alibaba Cloud account** ID in string format. For more information about how to query your ID, see section 2.1 in [user ID configuration](reseller.en-US/User Guide/Logtail collection/Machine Group/Collect logs from non-Alibaba Cloud ECS instances or ECS instances not in your account.md).**Note:** This parameter value must be your **Alibaba Cloud account** ID. RAM user IDs do not take effect.

|
|`${your_machine_group_user_defined_id}`|This parameter specifies the custom ID of a machine group in your cluster. The ID must be unique within the region where Log Service is deployed. For more information, see [Configure a user-defined identity for a machine group](reseller.en-US/User Guide/Logtail collection/Machine Group/Configure a user-defined identity for a machine group.md).|

## Configuration 2: Set the mount path. {#section_ibr_lmg_2gb .section}

1.  Logtail and the service container must be mounted to the same directory.
2.  The `emptyDir` mount method is recommended.

The mount path example is shown in the preceding configuration example.

## Log collection settings {#section_jcn_fmg_2gb .section}

Log collection can be set through CRD or the Log Service console. CRD-based settings support automatic creation of projects, Logstores, indexes, machine groups, and Logtail Configs, and can be easily integrated with Kubernetes. Therefore, CRD-based settings are recommended. Console-based settings are easier for users who debug or use Kubernetes log collection for the first time.

## CRD-based settings {#section_yyf_nmg_2gb .section}

For more information, see [Configure Kubernetes log collection on CRD](reseller.en-US/User Guide/Logtail collection/Container log collection/Configure Kubernetes log collection on CRD.md). Compared with the DaemonSet collection mode, CRD-based settings are subjected to the following limits:

1.  You must specify the name of the project requiring log collection. Otherwise, logs are collected and sent to the project where the log component is installed by default.
2.  You must specify the machine group for the settings to take effect. Otherwise, the settings are applied to the machine group to which the DaemonSet belongs by default.
3.  The Sidecar mode supports only file collection, during which, `dockerFile` must be set to false.

For more information, see the corresponding example.

## Console-based settings {#section_bdb_4mg_2gb .section}

1.  Configure the machine group.

    In the Log Service console, create a Logtail machine group with the identification set to a custom ID to dynamically adapt to changes of the Pod IP address. To do so, perform the following steps:

    1.  Activate Log Service and create a project and a Logstore as needed. For more information, see [Preparation](reseller.en-US/User Guide/Preparation/Preparation.md).
    2.  On the machine group list page, click **Create Machine Group**.
    3.  Set the identification to the custom ID `ALIYUN_LOGTAIL_USER_DEFINED_ID`.![](http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/c9f1dee297fe02c61ac3936391887aa1.png)
2.  Set the collection mode.

    Set collection details for the target file. Currently, various modes are supported, such as the simple mode, Nginx access mode, delimiter mode, JSON mode, and regular mode. For more information, see [Collect text logs](reseller.en-US/User Guide/Logtail collection/Text logs/Collect text logs.md).

    The settings of this example is shown in the following figure.

    **Note:** **Docker File** must be disabled.

    ![](https://cdn.nlark.com/lark/0/2018/png/33393/1539177113823-11daa00d-685e-44c2-a460-2a3880e72361.png)


## Examples {#section_fn5_pmg_2gb .section}

Scenario:

1.  The Kubernetes cluster is an on-premises cluster in an IDC, and the region where Log Service is deployed is China \(Hangzhou\). Logs are collected from the Internet.
2.  In the following examples, the mount object is `nginx-log`, and the mount type is `emptyDir`. They are mounted to the /var/log/nginx directory in the nginx-log-demo and logtail containers, respectively.
3.  The access log is /var/log/nginx/access.log, and the destination Logstore is `nginx-access`.
4.  The error log is /var/log/nginx/error.log, and the destination Logstore is `nginx-error`.

-   **Sidecar settings:**

```
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

-   **CRD settings:**

    ```
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
          # Simple logs with logType set to common_reg_log
          logType: common_reg_log
          # Log folder
          logPath: /var/log/nginx
          # File name with wildcards supported, for example, log_*.log
          filePattern: access.log
          # Sidecar mode with dockerFile set to false
          dockerFile: false
          # Line start regular expression, which is set to .* is the log contains only a line
          logBeginRegex: '. *'
          # Regular expression for parsing
          regex: '(\S+)\s(\S+)\s\S+\s\S+\s"(\S+)\s(\S+)\s+([^"]+)"\s+(\S+)\s(\S+)\s(\d+)\s(\d+)\s(\S+)\s"([^"]+)"\s. *'
          # List of the extracted keys
          key : ["time", "ip", "method", "url", "protocol", "latency", "payload", "status", "response-size",ser-agent"]
    # config for error log
    ```

    ```
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
          # Simple logs with logType set to common_reg_log
          logType: common_reg_log
          # Log folder
          logPath: /var/log/nginx
          # File name with wildcards supported, for example, log_*.log
          filePattern: error.log
          # Sidecar mode with dockerFile set to false
          dockerFile: false
    ```

-   **View log collection results**

    After the preceding settings are applied to the Kubernetes cluster, the Logtail container automatically creates the corresponding project, Logstore, machine group, and Logtail Config, and automatically sends the collected logs to Log Service. You can log on to the Log Service console to view details.


