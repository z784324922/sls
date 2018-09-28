# Collect Kubernetes logs {#concept_h24_wd4_xdb .concept}

Log Service enables Logtail to collect Kubernetes cluster logs, and uses the CustomResourceDefinition \(CRD\) API to manage collection configurations. This document describes how to install and use Logtail to collect Kubernetes cluster logs.

## Collection procedure {#section_xcs_dnr_xdb .section}

1.  Install the alibaba-log-controller Helm package.
2.  Configure the collection.

    You can configure the collection in the Log Service console or by using the CRD API as required. To configure the collection in the console, follow these steps:


![](images/3793_en-US.png "Procedure")

## Step 1 Install the package. {#section_pc2_1pr_xdb .section}

1.  Log on to the Master node of the Alibaba Cloud Container Service for Kubernetes.

    For how to log in, see [Access Kubernetes clusters by using SSH key pairs](../../../../reseller.en-US/User Guide/Kubernetes cluster/Clusters/Access Kubernetes clusters by using SSH key pairs.md).

2.  Replace the parameters and  run the following command.

    `${your_k8s_cluster_id}` to your Kubernetes cluster ID in the following installation command, and run this command:

    ```
     wget http://logtail-release.oss-cn-hangzhou.aliyuncs.com/linux64/alicloud-log-k8s-install.sh -O alicloud-log-k8s-install.sh; chmod 744 ./alicloud-log-k8s-install.sh; sh ./alicloud-log-k8s-install.sh ${your_k8s_cluster_id}
    ```


**Installation example**

Run the installation command to obtain the following echo:

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
NAME: alibaba-log-controller
LAST DEPLOYED: Wed May 16 18:43:06 2018
NAMESPACE: default
STATUS: DEPLOYED


RESOURCES:
==> v1beta1/ClusterRoleBinding
NAME AGE
alibaba-log-controller 0s


==> v1beta1/DaemonSet
NAME DESIRED CURRENT READY UP-TO-DATE AVAILABLE NODE SELECTOR AGE
logtail 2 2 0 2 0 0s


==> v1beta1/Deployment
NAME DESIRED CURRENT UP-TO-DATE AVAILABLE AGE
alibaba-log-controller 1 1 1 0 0s


==> v1/Pod(related)
NAME READY STATUS RESTARTS AGE
logtail-ff6rf 0/1 ContainerCreating 0 0s
logtail-q5s87 0/1 ContainerCreating 0 0s
alibaba-log-controller-7cf6d7dbb5-qvn6w 0/1 ContainerCreating 0 0s


==> v1/ServiceAccount
NAME SECRETS AGE
alibaba-log-controller 1 0s


==> v1beta1/CustomResourceDefinition
NAME AGE
aliyunlogconfigs.log.alibabacloud.com 0s


==> v1beta1/ClusterRole
alibaba-log-controller 0s




[SUCCESS] install helm package : alibaba-log-controller success.
```

You can use helm status `helm status alibaba-log-controller` to check the current Pod status. The Running status indicates a successful installation.

Then, Log Service creates the project that is named starting with **k8s-log**. You can search for this project by using the **k8s-log** keyword in the Log Service console.

## Step 2: Configure the collection. {#section_n11_zpr_xdb .section}

To create Logstore and collect standard output \(stdout\) from all K8s containers, follow these steps:

1.  Go to the Logstore List page.

    Click the project created in Step 1 to go to the Logstore List page.

2.  Create Logstore.

    Click **Create** in the upper-right corner, and in the dialog box that appears, create Logstore.

    ![](images/3794_en-US.png "Creating Logstore")

3.  Configure the collection.

    1.  Go to the **Data Import Wizard** page.
    2.  Select **Docker Stdout** from **Third-Party Software**.

        Click **Apply to Machine Group** on the configuration pages.  Then, you can collect all stdout files from all containers.

        ![](images/3796_en-US.png "Docker stdout")

4.  Apply the configuration to the machine group.

    On the Apply to Machine Group page, select a machine group, and click Next.

    ![](images/3797_en-US.png "Applying the configuration to the machine group")


Now you have configured the collection. To configure indexes and log shipping, continue with the follow-up configurations. You can also exit the current page to complete the configuration.

## View collected logs {#section_h4p_dsr_xdb .section}

Based on the collection configuration, Logtail can collect stdout logs one minute after a container in your cluster receives stdout input.  On the **Logstore List** page, click **Preview** to  quickly preview collected logs, or click **Search** to customize searching and analysis of these logs.

![](images/3798_en-US.png "Previewing and searching")

As shown in the following image of the Search page, click any keyword of a log to start quick searching, or enter the keyword in the search box to search the specified logs.

![](images/3804_en-US.png "Searching logs")

## Other methods for configuring collections {#section_e3x_qsr_xdb .section}

For more information about other methods for configuring collections, see:

## Console Configuration {#section_jvy_r5r_g2b .section}

For more information about Console configuration, see:

-   [Container text log \(recommended\)](../../../../reseller.en-US/User Guide/Logtail collection/Data Source/Container text logs.md)
-   [Container standard output \(recommended\)](../../../../reseller.en-US/User Guide/Logtail collection/Data Source/Containers-standard output.md)
-   [Host text file](../../../../reseller.en-US/User Guide/Logtail collection/Data Source/Collect text logs.md)

    By default, the root directory of the host is mounted to the /logtail\_host directory of the Logtail container. You must add this prefix when configuring the path.  For example, to collect data in the /home/logs/app\_log/ directory of the host, set the log path on the configuration page to /logtail\_host/home/logs/app\_log/.


## CRD Configuration {#section_gvy_r5r_g2b .section}

For more information about CRD\(CustomResourceDefinition\) configuration, see [Configure Kubernetes log collection on CRD](../../../../reseller.en-US/User Guide/Logtail collection/Data Source/Configure Kubernetes log collection on CRD.md#).

