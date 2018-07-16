# Containers-standard output {#concept_yk2_t2d_wdb .concept}

Logtail supports using the standard output stream of the container as the input source, and uploading the standard output stream together with the container metadata to Log Service.

## Features {#section_fdb_pvx_pdb .section}

-   Supports collection stdout and stderr
-   Supports label specified collection containers
-   Supports using labels to exclude specific containers
-   Supports environments to to specify containers to be collected.
-   Supports environments to exclude specific containers.
-   Supports multi-line logs \(like java stack logs\)
-   Supports automatic tagging of container data
-   Supports automatic tagging for Kubernetes containers

## Implementation principle {#section_vcf_qvx_pdb .section}

As shown in the following figure, Logtail communicates with the Domain Socket of Docker to query all of the containers running on Docker and then locate the containers to be collected according to label information.  Then, Logtail uses the Docker log command to retrieve the specified container log.

When Logtail collects the standard output of the container, it periodically saves the collected point information in the checkpoint file. If Logtail is restarted after being stopped, the log will be collected from the last saved point.

![](images/2950_en-US.png "Implementation principle")

## Limits {#section_ftp_vvx_pdb .section}

-   Currently, this feature only supports Linux and depends on Logtail 0.16.0 and later versions. For version check and upgrade, see [Linux ](intl.en-US/User Guide/Logtail collection/Install/Linux .md).
-   By default, Logtail uses `/var/run/docker.sock` to access Docker Engine. Make sure that Domain Socket exists and has access permissions.
-   **Multiline log limit**. To ensure that a logmade up of multiple lines is not split up due to output delay, multi-line logs will be cached for a short time by default. The default cache time is three seconds, but can be changed by using the `BeginLineTimeoutMs` parameter. However, this value cannot be less than 1000. Otherwise, the operation may be prone to error.
-     **Policy of stopping collection**. When the container is stopped, Logtail stops collecting the standard output from the container after listening to the container to `die` event. If a collection delay occurs during this time, it is possible to lose parts of the output before the stop.
-   **Context limit**. Each collection is deployed to the same context by default. If you need a different context for each type of container, they must be set individually.
-   **Data processing**.  The default field of collected data is `content`, which supports common processing configurations.
-   **Label**. The label is the label information in the Docker inspect, not the label in the Kubernetes configuration.
-     **Environment** The environment is the environment information configured in the container startup.

## Configuration process {#section_jct_bwx_pdb .section}

1.  Deploy and configure the Logtail container.
2.  Set the collection configuration in Log Service.

## 1. Logtail deployment and configuration {#section_kpj_cwx_pdb .section}

-   **Kubernetes**
-   **Other container management methods**

## 2. Collection configuration in Log Service {#section_a13_fwx_pdb .section}

1.  On the Logstore List page, click the **Data Import Wizard** icon to enter the configuration process.
2.  Select the data source. 

    Select **Docker Stdout** under **Third-Party Software** and then click **Next**.

3.  Configure the data source.

    On the Configure Data Source page, complete your collection configuration. See the following example.

    ```
    {
     "inputs": [
         {
             "type": "service_docker_stdout",
             "detail": {
                 "Stdout": true,
                 "Stderr": true,
                 "IncludeLabel": {
                     "io.kubernetes.container.name": "nginx"
                 },
                 "ExcludeLabel": {
                     "io.kubernetes.container.name": "nginx-ingress-controller"
                 },
                 "IncludeEnv": {
                     "NGINX_SERVICE_PORT": "80"
                 },
                 "ExcludeEnv": {
                     "POD_NAMESPACE": "kube-system"
                 }
             }
         }
     ]
    }
    ```

4.  Apply the configuration to the machine group.

    Enter the apply to machine group page. select the Logtail machine group to be collected and click Apply to Machine Group to apply the configuration to the selected machine group.  If you have not created a machine group, click **Create Machine Group** to create one.


## Description {#section_rdm_lwx_pdb .section}

The input source type is `service_docker_stdout`

|Configuration items|Type|Required or not|Description|
|:------------------|:---|:--------------|:----------|
|IncludeLabel|The mapping type, where key and value are both strings.|Required.|Empty by default. When this is empty, all Container data will be collected. When key is not empty and value is empty, all containers with a label containing this key will be collected.**Note:** 

1.  Key-value pairs have an OR relationship between each other, that is, a container is collected if its label includes any of the key-value pairs. 
2.  Here the label is the label information in Docker inspect.

|
|ExcludeLabel|The mapping type, where key and value are both strings.|Optional|Empty by default. When empty, no Containers will be excluded. When key is not empty and value is empty, all containers with a label that contains this key will be excluded.**Note:** 

1.  All key-value pairs have an OR relationship. As long as the label for a container includes one of the key-value pairs, it will be excluded.
2.  Here the label is the label information in Docker inspect.

|
|IncludeEnv|The mapping type, where key and value are both strings.|Optional|Empty by default. If empty, all containers are collected. If the key is not empty but the value is empty, all the containers whose environment includes this key are collected.**Note:** 

1.  Key-value pairs have an OR relationship between each other, that is, a container is collected if its environment includes any of the key-value pairs.
2.  The environment is the environment information configured in the container startup.

|
|ExcludeEnv| The mapping type, where key and value are both strings.|Optional|Empty by default. If empty, no containers are excluded. If the key is not empty but the value is empty, all the containers whose environment includes this key are excluded.**Note:** 

1.  Key-value pairs have an OR relationship between each other, that is, a container is excluded if its environment includes any of the key-value pairs.
2.  The environment is the environment information configured in the container startup.

|
|Stdout|bool|Optional|True by default. When false, stdout data will not be collected.|
|Stderr|bool|Optional|True by default. When false, stderr data will not be collected.|
|BeginLineRegex|String|Optional|Empty by default. When not empty it is the first match to the regular expression in the line. If a line matches the regular expression, then that line will be treated as a new log. Otherwise the line of data will be connected to the previous log.|
|BeginLineTimeoutMs|int|Optional|Timeout time for matchin a lin, measured in miliseconds, 3000 by default. Every 3 seconds, if a new log has not appeared, the last log will be output.|
|BeginLineCheckLength|int|Optional|The length \(in bytes\) of the beginning of a row used to match with the regular expression. The default value is 10\*1024. If the regular expression can match with the row within the first N bytes, configure this parameter to increase the matching efficiency. |
|MaxLogSize|int|Optional|The maximum length \(in bytes\) of a log. The default value is 512\*1024.  If the log exceeds this setting, it will not continue to be searched, rather it will be directly uploaded.|

## Default fields {#section_k1z_ywx_pdb .section}

**Normal Docker**

The following fields are uploaded by each log by default.

|Field name|Description:|
|:---------|:-----------|
| `_time_` |The data time. For example, `2018-02-02T02:18:41.979147844Z`.|
| `_source_` |The input source type, either stdout or stderr.|
| `_image_name_` |The image name.|
| `_container_name_` |The container name.|

**Kubernetes**

If the cluster is a Kubernetes cluster, the following fields are uploaded by each log by default.

|Field name|Description:|
|:---------|:-----------|
| `_time_` |The data time. For example, `2018-02-02T02:18:41.979147844Z`.|
| `_source_` |The input source type, either stdout or stderr.|
| `_image_name_` |The image name.|
| `_container_name_` |The container name.|
| `_pod_name_` |The pod name.|
| `_namespace_` |The namespace where the pod resides.|
| `_pod_uid_` |The unique identifier for the pod.|

## Configuration example {#section_h3v_zwx_pdb .section}

## General configuration {#section_bln_jyx_pdb .section}

-   **Environment configuration configuration**

    Collect the stdout logs and stderr logs of containers whose environment is `NGINX_PORT_80_TCP_PORT=80`, and is not `POD_NAMESPACE=kube-system`:

    **Note:** The environment is the environment information configured in the container startup.

    ![](images/2951_en-US.png "Example of Environment Configuration")

    **Collection configuration**

    ```
    {
        "inputs": [
            {
                "type": "service_docker_stdout",
                "detail": {
                    "Stdout": true,
                    "Stderr": true,
                    "IncludeEnv": {
                        "NGINX_PORT_80_TCP_PORT": "80"
                    },
                    "ExcludeEnv": {
                        "POD_NAMESPACE": "kube-system"
                    }
                }
            }
        ]
    }
    ```

-   **Label  configuration**

    Collect the stdout logs and stderr logs of containers whose label is `io.kubernetes.container.name=nginx`, and is not `type=pre`:

    **Note:** The label here is Docker not the label in the Kubernetes configuration.

    ![](images/2952_en-US.png "Label configuration example")

    ```
    {
        "inputs": [
            {
                "type": "service_docker_stdout",
                "detail": {
                    "Stdout": true,
                    "Stderr": true,
                    "IncludeLabel": {
                        "io.kubernetes.container.name": "nginx"
                    },
                    "ExcludeLabel": {
                        "type": "pre"
                    }
                }
            }
        ]
    }
    ```


## Collection configuration of multiline logs {#section_mhz_xyx_pdb .section}

Multi-line log collection is particularly important for the collection of Java exception stack output. Here we introduce a standard Java standard output log collection configuration.

-   **Log sample**

    ```
    2018-02-03 14:18:41.968 INFO [spring-cloud-monitor] [nio-8080-exec-4] c.g.s.web.controller.DemoController : service start
    2018-02-03 14:18:41.969 ERROR [spring-cloud-monitor] [nio-8080-exec-4] c.g.s.web.controller.DemoController : java.lang.NullPointerException
    at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193)
    at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166)
    at org.apache.catalina.core.StandardWrapperValve.invoke(StandardWrapperValve.java:199)
    at org.apache.catalina.core.StandardContextValve.invoke(StandardContextValve.java:96)
    ...
    2018-02-03 14:18:41.968 INFO [spring-cloud-monitor] [nio-8080-exec-4] c.g.s.web.controller.DemoController : service start done
    ```

-   **Collection configuration**

    Collect input logs of containers whose label is `app=monitor` and the beginning of a row is of the date type \(to increase matching efficiency, only the first 10 bytes of the row is used to check for a match with the regular expression\).

    ```
    {
    "inputs": [
      {
        "detail": {
          "BeginLineCheckLength": 10,
          "BeginLineRegex": "\\d+-\\d+-\\d+. *",
          "IncludeLabel": {
            "app": "monitor"
          }
        },
        "type": "service_docker_stdout"
      }
    ]
    }
    ```


## Process collected data {#section_mkf_bzx_pdb .section}

Logtail supports common data processing methods for collected Docker standard output. We recommend that you use a regular expression to parse logs into time, module, thread, class, and info based on the multiline log format in the previous section.

-   **Collection configuration**:

    Collect input logs of containers whose label is `app=monitor` and the beginning of a row is of the date type \(to increase matching efficiency, only the first 10 bytes of the row is used to check for a match with the regular expression\).

    ```
    {
    "inputs": [
      {
        "detail": {
          "BeginLineCheckLength": 10,
          "BeginLineRegex": "\\d+-\\d+-\\d+. *",
          "IncludeLabel": {
            "app": "monitor"
          }
        },
        "type": "service_docker_stdout"
      }
    ],
    "Processors ":[
        {
            "type": "processor_regex",
            "detail": {
                "SourceKey": "content",
                "Regex": "(\\d+-\\d+-\\d+ \\d+:\\d+:\\d+\\.\\d+)\\s+(\\w+)\\s+\\[([^]]+)]\\s+\\[([^]]+)]\\s+:\\s+(. *)",
                "Keys": [
                    "time",
                    "module",
                    "thread",
                    "class",
                    "info"
                ],
                "NoKeyError": true,
                "NoMatchError": true,
                "KeepSource": false
            }
        }
    ]
    }
    ```

-   Sample output:

    The output after processing the log `2018-02-03 14:18:41.968 INFO [spring-cloud-monitor] [nio-8080-exec-4] c.g.s.web.controller.DemoController : service start done` is as follows:

    ```
    __tag__:__hostname__:logtail-dfgef
    _container_name_:monitor
    _image_name_:registry.cn-hangzhou.aliyuncs.xxxxxxxxxxxxxxx
    _namespace_:default
    _pod_name_:monitor-6f54bd5d74-rtzc7
    _pod_uid_:7f012b72-04c7-11e8-84aa-00163f00c369
    _source_:stdout
    _time_:2018-02-02T14:18:41.979147844Z
    Time: 2018-02-02 02:18:41. 968
    level:INFO
    module:spring-cloud-monitor
    Thread: fig
    Class: c.g.s. web. Controller. demcontroller
    message:service start done
    ```


