# Kubernetes log collection {#concept_fy3_mrv_vdb .concept}

Log Service uses Logtail to collect Kubernetes cluster logs and manages collection configuration through custom resource definition \(CRD\).  This document describes how to install and use Logtail to collect Kubernetes cluster logs.

## Configuration process {#section_npn_jgq_pdb .section}

 ![](images/2684_en-US.png "Configuration process") 

1.  Run the installation command to install the alibaba-log-controller Helm package.
2.  Choose the CRD or console to manage collection configuration as required.

## Step 1 Installation {#section_ixs_qgq_pdb .section}

## Installation for Kubernetes on Alibaba Cloud Container Service {#section_lnl_jpr_zdb .section}

**Installation steps**

1.  Log on to the master node of your Alibaba Cloud Container Service Kubernetes. For more information on logon, see [Access Kubernetes clusters by using SSH](../../../../intl.en-US/User Guide/Kubernetes cluster/Clusters/Access Kubernetes clusters by using SSH.md).
2.  Replace `${your_k8s_cluster_id}` in the following command with your Kubernetes cluster ID and run the command:

    ```
    wget http://logtail-release.oss-cn-hangzhou.aliyuncs.com/linux64/alicloud-log-k8s-install.sh -O alicloud-log-k8s-install.sh; chmod 744 ./alicloud-log-k8s-install.sh; sh ./alicloud-log-k8s-install.sh ${your_k8s_cluster_id}
    ```

    After installation, Log Service automatically creates a Log Service project in the same region of your Kubernetes cluster. The name of the created project is `k8s-log-${your_k8s_cluster_id}`. Under the project, machine group `k8s-group-${your_k8s_cluster_id}`\} is created automatically.

    **Note:** Under project `k8s-log-${your_k8s_cluster_id}`, Logstore `config-operation-log` is created automatically. Do not delete the Logstore.


**Installation example**

After successful execution, the following information is output:

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
logtail 2 2 0 2 0 <none> 0s
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

You can run `helm Status alibaba-log-controller` to check the current states of pods. If all states are successful, installation is successful.

After successful installation, log on to the Log Service console. The Log Service project automatically created is displayed on the console. \(If you have many projects, search the keyword `k8s-log`.\)

## Self-built Kubernetes installation {#section_kdx_bqr_zdb .section}

**Restrictions**

1.  The Kubernetes cluster must be version 1.8 or later.
2.  Helm 2.6.4 or later has been installed.

**Installation procedure**

1.  In the Log Service console, create a project. The project name must begin with `k8s-log-custom-`.
2.  In the following command, replace the parameters with your own, and run the command.

    ```
    wget http://logtail-release.oss-cn-hangzhou.aliyuncs.com/linux64/alicloud-log-k8s-custom-install.sh -O alicloud-log-k8s-custom-install.sh; chmod 744 ./alicloud-log-k8s-custom-install.sh; sh ./alicloud-log-k8s-custom-install.sh {your-project-suffix} {region-id} {aliuid} {access-key-id} {access-key-secret}
    ```

    The parameters and their descriptions are as follows:

    |Name|Description|
    |----|-----------|
    |\{your-project-suffix\} |The maid-later part of the project name that you created in the second step.`k8s-log-custom-` that you have created in the second step. For example, the created project is `k8s-log-custom-xxxx`, then you mast enter `xxxx`.|
    |\{regionId\} |The ID of the region where your project is located. You can view the [Service endpoint](../../../../intl.en-US/API Reference/Service endpoint.md), for example, the region ID of `China East 1  (Hangzhou)` is  `cn-hangzhou`|
    |\{aliuid\}|User ID, please replace with your Alibaba Cloud master account user ID. The master account user ID is in the form of a string, and how to view the ID please refer to section 2.1 [of](intl.en-US/User Guide/Logtail collection/Machine Group/Collect logs from non-Alibaba Cloud ECS instances or ECS instances not in your account.md)the user identity configuration.|
    |\{access-key-id\}|Your account access key ID. Sub-account access is recommended Key, and grant permission, specific settings reference.[Authorization - Overview](intl.en-US/User Guide/Access control RAM/Authorization - Overview.md)|
    |\{access-key-secret\} |Your account access key secret. We recommend that you use the sub-account AccessKey and  grant AliyunLogFullAccess permission. For more information, see [Authorization - Overview](intl.en-US/User Guide/Access control RAM/Authorization - Overview.md).|

    After installation, Log Service automatically creates a machine group in the project. The machine group name is `k8s-group-${your_k8s_cluster_id}`.

    **Note:** 

    -   Logstore `config-operation-log` is automatically created in the project k8s-log-$\{your\_k8s\_cluster\_id\}. Do not delete this Logstore.
    -   After self-built kubernetes installation, Logtail is granted `privileged` permissions to avoid the error during the deletion of other pods `container text file busy`  error during the deletion of other pods. For more information, see [bug 1468249](https://bugzilla.redhat.com/show_bug.cgi?spm=a2c4g.11186623.2.10.QhaVGc&id=1468249), [bug 1441737](https://bugzilla.redhat.com/show_bug.cgi?spm=a2c4g.11186623.2.11.QhaVGc&id=1441737), and [issue 34538](https://github.com/moby/moby/issues/34538?spm=a2c4g.11186623.2.12.QhaVGc).

**Installation example**

The output of the successful execution is as follows:

```
[root@iZbp1dsxxxxxqfbiaZ ~]# wget http://logtail-release.oss-cn-hangzhou.aliyuncs.com/linux64/alicloud-log-k8s-custom-install.sh -O alicloud-log-k8s-custom-install.sh; chmod 744 ./alicloud-log-k8s-custom-install.sh; sh ./alicloud-log-k8s-custom-install.sh xxxx cn-hangzhou 165xxxxxxxx050 LTAxxxxxxxxxxx AIxxxxxxxxxxxxxxxxxxxxxxxxxxxxxe
....
....
....
NAME: alibaba-log-controller
LAST DEPLOYED: Fri May 18 16:52:38 2018
NAMESPACE: default
STATUS: DEPLOYED
RESOURCES:
==> v1beta1/ClusterRoleBinding
NAME AGE
alibaba-log-controller 0s
==> v1beta1/DaemonSet
NAME DESIRED CURRENT READY UP-TO-DATE AVAILABLE NODE SELECTOR AGE
logtail-ds 2 2 0 2 0 <none> 0s
==> v1beta1/Deployment
NAME DESIRED CURRENT UP-TO-DATE AVAILABLE AGE
alibaba-log-controller 1 1 1 0 0s
==> v1/Pod(related)
NAME READY STATUS RESTARTS AGE
logtail-ds-7xf2d 0/1 ContainerCreating 0 0s
logtail-ds-9j4bx 0/1 ContainerCreating 0 0s
alibaba-log-controller-796f8496b6-6jxb2 0/1 ContainerCreating 0 0s
==> v1/ServiceAccount
NAME SECRETS AGE
alibaba-log-controller 1 0s
==> v1beta1/CustomResourceDefinition
NAME AGE
aliyunlogconfigs.log.alibabacloud.com 0s
==> v1beta1/ClusterRole
alibaba-log-controller 0s
[INFO] your k8s is using project : k8s-log-custom-xxx, region : cn-hangzhou, aliuid : 1654218965343050, accessKeyId : LTAxxxxxxxxxxx
[SUCCESS] install helm package : alibaba-log-controller success.
```

You can use the `helm  status alibaba-log-controller` to view the current pod status. If all the statuses are successful, the installation is complete.

Log on to the Log Service console after installation. You can view the automatically created Log Service project. If you have many projects, search by the keyword `k8s-log`.

## Step 2  Configure {#section_axv_ghq_pdb .section}

Log collection supports the console configuration mode by default. Meanwhile, CRD configuration mode for the Kubernetes microservice development is also provided. You can use kubectl to manage the configuration.  The comparison of the two configurations is as follows:

|-|CRD Mode |Console mode|
|:-|:--------|:-----------|
|Operational complexity|Low|Medium|
|Function|Supports advanced configuration with the exception of Console mode|Medium|
|Complexity|Medium|Low|
|Network connection |Connect to the Kubernetes cluster |Connect to the Internet|
|Integration with deployment components |Supported|Not supported|
|Authentication method |Kubernetes authorization |Cloud account authentication|

We recommend you use the CRD method for collection configuration management, as this method is better integrated with the Kubernetes deployment and publishing process.

## Manage collection configurations on the console {#section_mnv_fsv_vdb .section}

Create Logtail collection configurations on the console as required. For configuration steps, see:

-   [Container text log \(recommended\)](intl.en-US/User Guide/Logtail collection/Data Source/Container text logs.md)
-   [Container standard output \(recommended\)](intl.en-US/User Guide/Logtail collection/Data Source/Containers-standard output.md)

-   [Host text file](intl.en-US/User Guide/Logtail collection/Data Source/Collect text logs.md)

    By default, the root directory of the host is mounted to the `/logtail_host` directory of the Logtail container. You must add this prefix when configuring the path.  For example, to collect data in the `/home/logs/app_log/` directory of the host, you must set the log path on the configuration page to `/logtail_host/home/logs/app_log/`.

-   [Syslog](intl.en-US/User Guide/Logtail collection/Data Source/Syslog.md)

## Acquisition configuration through CRD Management {#section_lwy_5n1_f2b .section}

For the kubernetes microservice development model, the logging service also provides a way to configure the CRD, you can directly use kubectl to manage the configuration, the integration of this approach with the kubernetes deployment and release process is more complete.

## Other operations {#section_ulb_3sv_vdb .section}

## Glasonset deployment migration step {#section_yk4_htr_zdb .section}

If you previously deployed the Log Service logtail by using the WebSphere set method that you used earlier, you will not be able to use CRD for configuration management. You can migrate to a new version in the following ways:

**Note:** During the upgrade, some logs are duplicated. The CRD management configuration can be used only for the configuration created using the CRD. The historical configuration does not support the CRD management mode because the historical configuration is created using a non-CRD mode.

1.  Install in the form of a new version, the installation command last adds a parameter for the Log Service Project name that was used by your previous kubernetes cluster.

    For example, the project name was `k8s-log-demo`, the cluster ID was `c12ba2028cxxxxxxxxxx6939f0b`, then the installation command is 

    ```
    wget
              http://logtail-release.oss-cn-hangzhou.aliyuncs.com/linux64/alicloud-log-k8s-install.sh -O
              alicloud-log-k8s-install.sh; chmod 744 ./alicloud-log-k8s-install.sh; sh
              ./alicloud-log-k8s-install.sh c12ba2028cxxxxxxxxxx6939f0b k8s-log-demo. 
    ```

2.  After successful installation, in the Log Service console apply the historical collection configuration to the new machine group `k8s-group-${your_k8s_cluster_id}`.
3.  In a minute, the historical collection configuration is bind to the historical machine group.
4.  After the log collection is normalized, you can delete the previously installed Logtail DaemonSet.

## Use multiple clusters in the same Log Service project {#section_ewp_ltr_zdb .section}

You can use multiple clusters to collect logs to the same Log Service project. When installing other clusters Log Service components, you must replace `${your_k8s_cluster_id}` in the installation parameters with the clusters ID you installed for the first time.

For example, you now have three clusters with IDs:  abc001, abc002, and abc003. The installation parameters for the three clusters, `${your_k8s_cluster_id}`, must all be filled as `abc001`.

**Note:** This method does not support Kubernetes multi-cluster sharing across regions.

## Logtail container logs {#section_olb_4tr_zdb .section}

Logtail logs are stored in the `/usr/local/ilogtail/` directory in the Logtail container, the file name is `ilogtail.LOG` and `ilogtail.plugin`, the container stdout does not have the reference significance, so you can ignore the following stdout output:

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

## View the status of log related components in the Kubernetes cluster {#section_cx4_ttr_zdb .section}

```
helm status alibaba-log-controller
```

## alibaba-log-controller failed to start  {#section_dx4_ttr_zdb .section}

Make sure that you perform the installation as follows:

1.  The installation command is executed on the master node of the kubernetes Cluster
2.  The installation command parameter is entered in the cluster ID.

If the installation fails due to these problems, use `helm del --purge  alibaba-log-controllerr` to remove the installation package and perform the installation again.

If the installation failure persists, open a ticket to contact Alibaba Cloud technical support.

## \#\#\#\# Check the status of the Logtail DaemonSet in the Kubernetes cluster {#section_ej5_ytr_zdb .section}

You can run the command `kubectl get ds -n kube-system` to check the running status of Logtail.

**Note:** The default namespace of Logtail is `kube-system`.

## \#\#\#\# How to adjust Logtail resource limits {#section_aj5_ytr_zdb .section}

By default Logtail can only occupy at most 40% CPU and 200M of ram. If you need to increase processing speed, you will need to adjust parameters in these two sections:

-   `limits` and `requests` in the `resources` of the YAML template.
-   The path of the Logtail startup configuration file is the `ALIYUN_LOGTAIL_CONFIG` environment variable in the YAML template. For the modification method, see [Configure startup parameters](intl.en-US/User Guide/Logtail collection/Install/Configure startup parameters.md).

## \#\#\#\# Force update Logtail DaemonSet {#section_e2b_d5r_zdb .section}

Run the following command to force Logtail DaemonSet update after modifying the `logtail-daemonset.yaml` file:

```
kubectl --namespace=kube-system delete ds logtail
kubectl apply -f ./logtail-daemonset.yaml
```

**Note:** Data duplication may occur during force update.

## \#\#\#\# Check configuration information for Logtail DaemonSet {#section_b2b_d5r_zdb .section}

```
'kubectl describe ds logtail -n kube-system'
```

## \#\#\#\# Check Logtail version number, IP, starting time, etc. {#section_glj_f5r_zdb .section}

An example is as follows:

```
[root@iZbp1dsu6v77zfb40qfbiaZ ~]# kubectl get po -n kube-system -l k8s-app=logtail
NAME READY STATUS RESTARTS AGE
logtail-gb92k 1/1 Running 0 2h
logtail-wm7lw 1/1 Running 0 4d
[root@iZbp1dsu6v77zfb40qfbiaZ ~]# kubectl exec logtail-gb92k -n kube-system cat /usr/local/ilogtail/app_info.json
{
   "UUID" : "",
   "hostname" : "logtail-gb92k",
   "instance_id" : "********************************",
   "ip" : "*.*.*.*",
   "logtail_version" : "0.16.2",
   "os" : "Linux; 3.10.0-693.2.2.el7.x86_64; #1 SMP Tue Sep 12 22:26:13 UTC 2017; x86_64",
   "update_time" : "2018-02-05 06:09:01"
}
```

## View the run log for logtail {#section_dlj_f5r_zdb .section}

Logtail running logs are stored in the `/usr/local/ilogtail/` directory. The file name is `ilogtail.LOG`. The rotation file is compressed and stored as `ilogtail.LOG.x.gz`.

An example is as follows:

```
[root@iZbp1dsu6v77zfb40qfbiaZ ~]# kubectl exec logtail-gb92k -n kube-system tail /usr/local/ilogtail/ilogtail.LOG
[2018-02-05 06:09:02.168693] [INFO] [9] [build/release64/sls/ilogtail/LogtailPlugin.cpp:104] logtail plugin Resume:start
[2018-02-05 06:09:02.168807] [INFO] [9] [build/release64/sls/ilogtail/LogtailPlugin.cpp:106] logtail plugin Resume:success
[2018-02-05 06:09:02.168822] [INFO] [9] [build/release64/sls/ilogtail/EventDispatcher.cpp:369] start add existed check point events, size:0
[2018-02-05 06:09:02.168827] [INFO] [9] [build/release64/sls/ilogtail/EventDispatcher.cpp:511] add existed check point events, size:0 cache size:0 event size:0 success count:0

```

## Restart the Logtail of a pod {#section_n4q_h5r_zdb .section}

An example is as follows:

```
[root@iZbp1dsu6v77zfb40qfbiaZ ~]# kubectl exec logtail-gb92k -n kube-system /etc/init.d/ilogtaild stop
kill process Name: ilogtail pid: 7
kill process Name: ilogtail pid: 9
stop success
[root@iZbp1dsu6v77zfb40qfbiaZ ~]# kubectl exec logtail-gb92k -n kube-system /etc/init.d/ilogtaild start
ilogtail is running

```

