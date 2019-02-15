# Container text logs {#concept_mgv_21d_wdb .concept}

Logtail supports collecting text logs generated in containers and uploading the collected logs together with the container metadata to Log Service.

## Function features {#section_ccs_hrx_pdb .section}

Compared with the basic log file collection, Docker file collection has the following features:

-   Set the log path within the container, no need to care about the mapping from this path to the host.
-   Supports using labels to specify containers to be collected.
-   Supports using labels to exclude specific containers.
-   Supports environments to specify containers to be collected.
-   Supports environments to exclude specific containers.
-   Supports multiline logs \(such as Java stack logs\).
-   Supports automatic tagging for container data.
-   Supports automatic tagging for Kubernetes containers.

## Limits {#section_iyv_3rx_pdb .section}

-    **Policy of stopping collection**: When the container is stopped, Logtail stops collecting logs from the container after listening to the `die` event of the container \(with a delay of 1–3 seconds\). If a collection delay occurs during this time, it is possible to lose part of the logs before the stop.
-   **Docker storage drives**: Currently, only overlay and overlay2 drives are supported. For other drive types, you must mount the log directory to your local PC.
-    **Logtail running methods**: Logtail must be run as a container and follow the Logtail deployment method.
-   **Label**: The label is the label information in the Docker inspect, not the label in the Kubernetes configuration.
-    **Environment**: The environment is the environment information configured in the container startup.

## Procedure {#section_hmq_krx_pdb .section}

1.  Deploy and configure the Logtail container.
2.  Set the collection configuration in Log Service.

## 1. Logtail deployment and configuration {#section_hmj_lrx_pdb .section}

-   **Kubernetes**

    For more information on Kubernetes log collection, see [Collect Kubernetes logs](reseller.en-US/User Guide/Logtail collection/Container log collection/Kubernetes log collection process.md).

-   **Other container management methods**

    For more information on other container management methods such as Swarm and Mesos, see [Collect standard Docker logs](reseller.en-US/User Guide/Logtail collection/Container log collection/Collect standard Docker logs.md).


## 2. Collection configuration in Log Service {#section_kzk_rrx_pdb .section}

1.  On the Logstore List page, click the **Data Import Wizard** icon to enter the configuration process.
2.  Select a data source. 

    Select **Docker File** under **Third-Party Software** and then click Next.

3.  Configure the data source.

    |Configuration item| Required|Description|
    |:-----------------|:--------|:----------|
    |Docker file|Yes|Confirm if the target file being collected is a Docker file.|
    |Label whitelist|Optional|LabelKey is required. If LabelValue is not empty, only containers whose label includes LabelKey = LabelValue are collected. If LabelValue is empty, all the containers whose label includes the LabelKey are collected.**Note:** 

    1.  Key-value pairs have an OR relationship between each other, that is, a container is collected if its label includes any of the key-value pairs. 
    2.  Here the label is the label information in Docker inspect.
|
    |Label blacklist|Optional|LabelKey is required. If LabelValue is not empty, only containers whose label includes LabelKey = LabelValue are excluded. If LabelValue is empty, all the containers whose label includes the LabelKey are excluded. **Note:** 

    1.  Key-value pairs have an OR relationship between each other, that is, a container is excluded if its label includes any of the key-value pairs.
    2.  Here the label is the label information in Docker inspect.
|
    |Environment whitelist|Optional|EnvKey is required. If EnvValue is not empty, only containers whose environment includes EnvKey=EnvValue are collected. If EnvValueis empty, all the containers whose environment includes the EnvKey are collected. **Note:** 

    -   Key-value pairs have an OR relationship between each other, that is, a container is collected if its environment includes any of the key-value pairs.
    -   Here the environment is the environment information configured in the container startup.
|
    |Environment blacklist|Optional|EnvKeyis required. If EnvValue is not empty, only containers whose environment includes EnvKey=EnvValue are excluded. If EnvValue is empty, all the containers whose environment includes EnvKey are excluded.**Note:** 

    1.  Key-value pairs have an OR relationship between each other, that is, a container is collected if its environment includes any of the key-value pairs. 
    2.  Here the environment is the environment information configured in the container startup.
|
    |Other configurations|-|For other collection configurations and parameter descriptions, see [Collect text logs](reseller.en-US/User Guide/Logtail collection/Text logs/Collect text logs.md).|

4.  Description
    -   In this topic, labels refer to label information contained in Docker inspect.
    -   The namespace and container name in Kubernetes are mapped to labels `io.kubernetes.pod.namespace` and `io.kubernetes.container.name` in a docker. For example, the Pod you have created belongs to the backend-prod namespace, and the container name is worker-server. In this case, you can configure two whitelist labels `io.kubernetes.pod.namespace : backend-prod` and `io.kubernetes.container.name : worker-server` to specify that only logs in the worker-server container can be collected.
    -   We recommend that you use only the `io.kubernetes.pod.namespace` and `io.kubernetes.container.name` labels in Kubernetes. For other scenarios, you can use an environment whitelist or blacklist.
5.  Apply to the machine group.

    On the Apply to Machine Group page, select the Logtail machine group to be collected and click **Apply to Machine Group** to apply the configuration to the selected machine group.  If you have not created a machine group, click **Create Machine Group** to create one.

6.  Complete the process of accessing container text logs.

    To configure the Search, Analysis, and Visualization function and the Shipper & ETL function, complete the settings as instructed on the page.


## Configuration example {#section_xyy_srx_pdb .section}

-   **Environment configuration**

    Collect the logs of containers whose environment is `NGINX_PORT_80_TCP_PORT=80` , and is not `POD_NAMESPACE=kube-system` . The log file path is `/var/log/nginx/access.log` , and the logs are parsed in the simple mode.

    **Note:** The environment is the environment information configured in the container startup.

    ![](images/2942_en-US.png "Example of Environment Configuration")

    The data source configuration in this example is as follows.  For other collection configurations and parameter descriptions, see [Collect text logs](reseller.en-US/User Guide/Logtail collection/Text logs/Collect text logs.md).

-   **Label configuration**

    Collect the logs of containers whose label is `io.kubernetes.container.name=nginx` , and is not type=pre . The log file path is  `/var/log/nginx/access.log` , and the logs are parsed in the simple mode.

    **Note:** The label is the label information in the Docker inspect, not the label in the Kubernetes configuration.

    ![](images/2944_en-US.png "Example label Mode")

    The data source configuration in this example is as follows. For other collection configurations and parameter descriptions, see [Collect text logs](reseller.en-US/User Guide/Logtail collection/Text logs/Collect text logs.md).


## Default field {#section_wgq_cvx_pdb .section}

**Normal Docker**

The following fields are uploaded by each log by default.

|Field|Description|
|:----|:----------|
| `_image_name_` |Image name.|
| `_container_name_` |Container name|
|`_container_ip_`|Container IP address|

**Kubernetes**

If the cluster is a Kubernetes cluster, the following fields are uploaded by each log by default.

|Field|Description|
|:----|:----------|
| `_image_name_` |Image name|
| `_container_name_` |Container name|
| `_pod_name_` |Pod name|
| `_namespace_` |Namespace where a Pod resides|
| `_pod_uid_` |The unique identifier of a Pod|
|`_container_ip_`|Pod IP address|

