# Configure Kubernetes log collection on CRD {#concept_tfg_pl1_f2b .concept}

Log collection is configured on the console by default. Log Service also provides CRD configuration for log collection for Kubernetes microservice development. This allows you to use kubectl to manage configurations.

We recommend you use the CRD method for collection configuration management, as this method is better integrated with the Kubernetes deployment and publishing process.

## Implementation principles {#section_ff4_drr_zdb .section}

![](images/2686_en-US.png "Implementation principles")

Run the installation command to install the `alibaba-log-controller` Helm package.  The Helm package mainly run the following operations:

1.  Create aliyunlogconfigs CRD \(Custom Resource Definition\).
2.  Deploy alibaba-log-controller.
3.  Deploy Logtail DaemonSet.

The internal workflow of configuration is as follows:

1.  Use `kubectl` or other tools to apply the aliyunlogconfigs CRD configuration.
2.  alibaba-log-controller detects configuration update.
3.  alibaba-log-controller automatically submits requests for Logstore creation, configuration creation, and configuration application to machine groups based on the CRD content and server status.
4.  Logtail running in DaemonSet mode periodically sends requests for server configuration, obtains the new or updated configuration, and performs the rapid loading.
5.  Logtail collects standard outputs or files from each container \(pod\) based on the configuration information.
6.  Logtail sends processed and aggregated data to the Log Service.

## Configuration method {#section_ir5_mm1_f2b .section}

**Note:** If you have used the Logtail deployed in DaemonSet mode, you cannot manage configurations in CRD mode.  For more information, see **Migration process for the DaemonSet deployment mode** in this document.

You must define the CRD of AliyunLogConfig to create configurations, and delete the corresponding CRD resource to delete the configuration.  The CRD is configured as follows:

```
apiVersion: log.alibabacloud.com/v1alpha1 ## Default value, no need for change
kind: AliyunLogConfig ## Default value, no need for change
metadata:
  name: simple-stdout-example ## Resource name, which must be unique in the cluster
spec:
  logstore: k8s-stdout ## Logstore name, automatically created if no name exists
  shardCount: 2 ## [Optional] Number of Logstore shards. The default value is 2. The value range is 1 to 10.
  lifeCycle: 90 ## [Optional] Storage period of the Logstore. The default value is 90. The value range is 1 to 7300. The value 7300 indicates permanent storage.
  logtailConfig: ## Detailed configuration
    inputType: plugin ## Input type of collection. Generally, the value is file or plugin.
    configName: simple-stdout-example ## Collection configuration name. The value must the same as the resource name (metadata.name).
    inputDetail: ## Detailed configuration information, see the example
      ...
```

After the configuration is completed and applied, alibaba-log-controller is created automatically.

## View configuration {#section_zyq_4m1_f2b .section}

You can check the configuration on the Kubernetes CRD or console.

For how to view configuration on the console, see [Create a Logtail configuration](reseller.en-US/User Guide/Logtail collection/Machine Group/Create a Logtail configuration.md).

**Note:** If you use the CRD method to manage configuration, the configuration changes you have made on the console will be overwritten when you update configuration on the CRD.

-   Run `kubectl get aliyunlogconfigs` to view all the configurations.

    ```
    [root@iZbp1dsbiaZ ~]# kubectl get aliyunlogconfigs
    NAME AGE
    regex-file-example 10s
    regex-stdout-example 4h
    simple-file-example 5s
    ```

-   Run `kubectl get aliyunlogconfigs ${config_name} -o yaml` to view the detailed configuration and status.

    The `status` field in the configuration shows the configuration execution result. If the configuration is successfully applied, the value of `statusCode` is 200 in the `status` field.  If the value of `statusCode` is not 200, applying the configuration failed.

    ```
    [root@iZbp1dsbiaZ ~]# kubectl get aliyunlogconfigs simple-file-example -o yaml
    apiVersion: log.alibabacloud.com/v1alpha1
    kind: AliyunLogConfig
    metadata:
    annotations:
      kubectl.kubernetes.io/last-applied-configuration: |
        
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


## Configuration example {#section_vrk_rm1_f2b .section}

## Container standard output {#section_c3g_wm1_f2b .section}

In the container standard output, set `inputType` to `plugin` and fill the detailed information in the `plugin` field under `inputDetail`. For more information on the configuration fields, see [Containers-standard output](reseller.en-US/User Guide/Logtail collection/Data Source/Containers-standard output.md).

-   **Simple collection mode**

    Collect standard outputs \(stdout and stdeer\) of all containers except for those who has environment variable configuration `COLLECT_STDOUT_FLAG=false`.

    ```
    apiVersion: log.alibabacloud.com/v1alpha1
    kind: AliyunLogConfig
    metadata:
      # your config name, must be unique in your k8s cluster
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

-   **Custom collection mode**

    Collect the access log of Grafana and parse the access log into structured data.

    Grafana container has environment variable configuration `GF_INSTALL_PLUGINS=grafana-piechart-....`. You can set `IncludeEnv` to `GF_INSTALL_PLUGINS: ''` to enable the Logtail to collect standard outputs from this container only.

    ![](images/2687_en-US.png "Custom collection mode")

    The access log of Grafana is in the following format:

    ```
    t=2018-03-09T07:14:03+0000 lvl=info msg="Request Completed" logger=context userId=0 orgId=0 uname= method=GET path=/ status=302 remote_addr=172.16.64.154 time_ms=0 size=29 referer=
    ```

    Parse the access log using a regular expression. The detailed configuration is as follows:

    ```
    apiVersion: log.alibabacloud.com/v1alpha1
    kind: AliyunLogConfig
    metadata:
      # your config name, must be unique in your k8s cluster
      name: regex-stdout-example
    spec:
      # logstore name to upload log
      logstore: k8s-stdout-regex
      # logtail config detail
      logtailConfig:
        # docker stdouts input type is plugin
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
                  # Collect stdout outputs only and do not collect stdeer outputs.
                  Stdout: true
                  Stderr: false
                  # Collect only stdout outputs whose key is "GF_INSTALL_PLUGINS" in the environment variable configuration from the container. 
                  IncludeEnv:
                    GF_INSTALL_PLUGINS: ''
            processors:
              -
                # Use a regular expression
                type: processor_regex
                detail:
                  # The data collected by the docker has key "content" by default.
                  SourceKey: content
                  # Regular expression for extraction
                  Regex: 't=(\d+-\d+-\w+:\d+:\d+\+\d+) lvl=(\w+) msg="([^"]+)" logger=(\w+) userId=(\w+) orgId=(\w+) uname=(\S*) method=(\w+) path=(\S+) status=(\d+) remote_addr=(\S+) time_ms=(\d+) size=(\d+) referer=(\S*). *'
                  # Extracted keys
                  Keys: ['time', 'level', 'message', 'logger', 'userId', 'orgId', 'uname', 'method', 'path', 'status', 'remote_addr', 'time_ms', 'size', 'referer']
                  # Retain the original fields
                  KeepSource: true
                  NoKeyError: true
                  NoMatchError: true
    ```

    After the configuration is applied, the data collected by Log Service is as follows:

    ![](images/2689_en-US.png "Collected log data")


## Container file {#section_qlv_zm1_f2b .section}

-   **Simple file**

    Collect log files from containers whose environment variable configuration contains key `ALIYUN_LOGTAIL_USER_DEFINED_ID`. The log file path is `/data/logs/app_1` and the file name is `simple.LOG`.

    ```
    apiVersion: log.alibabacloud.com/v1alpha1
    kind: AliyunLogConfig
    metadata:
      # your config name, must be unique in your k8s cluster
      name: simple-file-example
    spec:
      # logstore name to upload log
      logstore: k8s-file
      # logtail config detail
      logtailConfig:
        # log file's input type is 'file'
        inputType: file
        # logtail config name, must same with [metadata.name]
        configName: simple-file-example
        inputDetail:
          # Set logType to "common_reg_log" for simple mode logs
          logType: common_reg_log
          # Log file folder
          logPath: /data/logs/app_1
          # File name, which supports wildcards, for example, log_*.log
          filePattern: simple.LOG
          # Collect files from the container. dockerFile flag is set to true
          dockerFile: true
          # Only collect container with "ALIYUN_LOGTAIL_USER_DEFINED_ID" in docker env config
          dockerIncludeEnv:
            ALIYUN_LOGTAIL_USER_DEFINED_ID: ""
    ```

-   **Complete regular expression files**

    The following is an example of a Java program log:

    ```
    [2018-05-11T20:10:16,000] [INFO] [SessionTracker] [SessionTrackerImpl.java:148] Expiring sessions
    java.sql.SQLException: Incorrect string value: '\xF0\x9F\x8E\x8F",...' for column 'data' at row 1
    at org.springframework.jdbc.support.AbstractFallbackSQLExceptionTranslator.translate(AbstractFallbackSQLExceptionTranslator.java:84)
    at org.springframework.jdbc.support.AbstractFallbackSQLException
    ```

    A log entry may be divided into multiple lines because the log contains error stacking information. Therefore, you must set a regular expression for the beginning of a line. To extract each field, use a regular expression. The detailed configuration is as follows:

    ```
    apiVersion: log.alibabacloud.com/v1alpha1
    kind: AliyunLogConfig
    metadata:
      # your config name, must be unique in your k8s cluster
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
          # Set logType to "common_reg_log" for logs of the regular expression type.
          logType: common_reg_log
          # Log file folder
          logPath: /app/logs
          # File name, which supports wildcards, for example, log_*.log
          filePattern: error.LOG
          # Regular expression for first line 
          logBeginRegex: '\[\d+-\d+-\w+:\d+:\d+,\d+]\s\[\w+]\s. *'
          # Parse the regular expression
          regex: '\[([^]]+)]\s\[(\w+)]\s\[(\w+)]\s\[([^:]+):(\d+)]\s(. *)'
          # List of extracted keys
          key : ["time", "level", "method", "file", "line", "message"]
          # Logs in regular expression. `time` in the logs are extracted for time parsing by default. If time is not required, ignore the field.
          timeFormat: '%Y-%m-%dT%H:%M:%S'
          # Collect files from the container. dockerFile flag is set to true
          dockerFile: true
          # Only collect container with "ALIYUN_LOGTAIL_USER_DEFINED_ID" in docker env config
          dockerIncludeEnv:
            ALIYUN_LOGTAIL_USER_DEFINED_ID: ""
    ```

    After the configuration is applied, the data collected by Log Service is as follows:

    ![](images/5237_en-US.png "Collected log data")

-   **Delimiter pattern file**

    Logtail supports log parsing in delimiter mode, an example is as follows:

    ```
    apiVersion: log.alibabacloud.com/v1alpha1
    kind: AliyunLogConfig
    metadata:
      # your config name, must be unique in your k8s cluster
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
          # Set logType to delimiter_log for logs of the delimiter type
          logType: delimiter_log
          # Log file folder
          logPath: /usr/local/ilogtail
          # File name, which supports wildcards, for example, log_*.log
          filePattern: delimiter_log.LOG
          # Use a multi-character delimiter
          separator: '|&|'
          # List of extracted keys
          key : ['time', 'level', 'method', 'file', 'line', 'message']
          # Keys for parsing time. Ignore the field if time parsing is not required
          timeKey: 'time'
          # Time parsing method. Ignore the field if time parsing is not required
          timeFormat: '%Y-%m-%dT%H:%M:%S'
          # Collect files from the container. dockerFile flag is set to true
          dockerFile: true
          # Only collect container with "ALIYUN_LOGTAIL_USER_DEFINED_ID" in docker env config
          dockerIncludeEnv:
            ALIYUN_LOGTAIL_USER_DEFINED_ID: ''
    ```

-   **JSON mode file**

    If each data line in a file is a JSON object, you can use the JSON method for parsing, an example is as follows:

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
          # Set logType to json_log for logs of the delimiter type
          logType: json_log
          # Log file folder
          logPath: /usr/local/ilogtail
          # File name, which supports wildcards, for example, log_*.log
          filePattern: json_log.LOG
          # Keys for parsing time. Ignore the field if time parsing is not required
          timeKey: 'time'
          # Time parsing method. Ignore the field if time parsing is not required
          timeFormat: '%Y-%m-%dT%H:%M:%S'
          # Collect files from the container. dockerFile flag is set to true
          dockerFile: true
          # Only collect container with "ALIYUN_LOGTAIL_USER_DEFINED_ID" in docker env config
          dockerIncludeEnv:
            ALIYUN_LOGTAIL_USER_DEFINED_ID: ""
    ```


