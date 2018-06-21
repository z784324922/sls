# 采集Kubernetes日志 {#concept_h24_wd4_xdb .concept}

日志服务支持通过Logtail采集Kubernetes集群日志，并支持CRD（CustomResourceDefinition）进行采集配置管理。本文主要介绍如何安装并使用Logtail采集Kubernetes集群日志。

## 采集流程 {#section_xcs_dnr_xdb .section}

1.  安装alibaba-log-controller Helm包。
2.  创建采集配置。

    根据您的需求选择使用控制台或CRD（CustomResourceDefinition）创建采集配置，本文以控制台为例。


![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13801/3793_zh-CN.png "采集流程")

## 步骤1 安装软件包 {#section_pc2_1pr_xdb .section}

1.  登录您的阿里云容器服务Kubernetes的Master节点。

    如何登录参考[SSH密钥对访问Kubernetes集群](../../../../intl.zh-CN/用户指南/Kubernetes 集群/集群管理/SSH密钥对访问Kubernetes集群.md)。

2.  替换参数后执行以下安装命令。

    将下述命令中的`${your_k8s_cluster_id}`替换为您的Kubernetes集群ID，并执行此命令。

    ```
     wget http://logtail-release.oss-cn-hangzhou.aliyuncs.com/linux64/alicloud-log-k8s-install.sh -O alicloud-log-k8s-install.sh; chmod 744 ./alicloud-log-k8s-install.sh; sh ./alicloud-log-k8s-install.sh ${your_k8s_cluster_id}
    ```


**安装示例**

执行安装命令，回显信息如下：

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
logtail 2 2 0 2 0  0s


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

您可以使用`helm status alibaba-log-controller`查看Pod当前状态，若状态全部成功后，表示安装成功。

安装成功后，日志服务会自动为您创建**k8s-log**开头的Project，在日志服务控制台搜索**k8s-log**关键字即可查看。

## 步骤2 创建采集配置 {#section_n11_zpr_xdb .section}

在该步骤中为您演示如何使用控制台创建Logstore并采集K8S所有容器的stdout标准输出。

1.  进入Logstore列表。

    单击步骤1中自动创建的Project，进入Logstore列表页面。

2.  创建Logstore。

    单击右上角的**创建**按钮，在弹出的页面中创建一个Logstore。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13801/3794_zh-CN.png "创建Logstore")

3.  创建采集配置。

    1.  创建Logstore后，根据提示进入**数据接入向导**。
    2.  选择**自建软件**中的**Docker标准输出**。

        在配置页面中直接单击**下一步**。无需做任何修改，即可实现采集所有容器的stdout文件。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13801/3796_zh-CN.png "Docker标准输出")

4.  将配置应用到机器组。

    在机器组配置页面，勾选机器组并单击**下一步**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13801/3797_zh-CN.png "应用到机器组")


数据采集配置已经完成，如您需要配置索引和数据投递，请根据页面提示在后续步骤中填写配置。您也可以直接退出当前页面，结束配置。

## 查看采集到日志数据 {#section_h4p_dsr_xdb .section}

完成数据采集配置后，若您的集群中有容器在输入stdout，则一分钟后即可采集到stdout日志。在**Logstore列表**页面中单击**预览** 可以快速预览当前采集到的日志数据；或者单击**查询**对采集到的日志数据进行自定义查询和分析。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13801/3798_zh-CN.png "预览和查询")

如下图所示，查询页面中可直接单击日志中的关键字进行快速查询，也可以再查询输入框中输入指定的关键字进行查询。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13801/3804_zh-CN.png "查询日志")

## 其他采集配置方式 {#section_e3x_qsr_xdb .section}

以上示例为控制台方式配置标准输出的采集方式，其他采集方式及详细配置请参考：

-   [容器内文本文件（推荐）](../../../../intl.zh-CN/用户指南/Logtail 采集/数据源/容器-文本日志.md)
-   [容器标准输出（推荐）](../../../../intl.zh-CN/用户指南/Logtail 采集/数据源/容器-标准输出.md)
-   [宿主机文本文件](../../../../intl.zh-CN/用户指南/Logtail 采集/数据源/文本日志.md)

    默认宿主机根目录挂载到Logtail容器的/logtail\_host目录，配置路径时，您需要加上此前缀。例如需要采集宿主机上/home/logs/app\_log/目录下的数据，配置页面中日志路径设置为/logtail\_host/home/logs/app\_log/。


