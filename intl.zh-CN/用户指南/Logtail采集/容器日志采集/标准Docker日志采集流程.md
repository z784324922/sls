# 标准Docker日志采集流程 {#concept_jc1_m4v_vdb .concept}

Logtail支持采集标准Docker日志，并附加容器的相关元数据信息一起上传到日志服务。

## 配置流程 {#section_y5k_ffq_pdb .section}

![](images/2674_zh-CN.png "配置流程")

1.  部署Logtail容器。
2.  配置Logtail机器组。

    日志服务控制台创建自定义标识机器组，后续该容器集群伸缩无需额外运维。

3.  创建服务端采集配置。

    在日志服务控制台创建采集配置，所有采集均为服务端配置，无需本地配置。


## 步骤一 部署Logtail容器 {#section_yqz_nfq_pdb .section}

1.  拉取Logtail镜像。

    ``` {#codeblock_0es_vi3_myr}
    docker pull registry.cn-hangzhou.aliyuncs.com/log-service/logtail
    ```

2.  启动Logtail容器。

    替换启动模板中的3个参数：`${your_region_name}`、`${your_aliyun_user_id}`和`${your_machine_group_user_defined_id}`。

    ``` {#codeblock_aun_frl_6wp}
    docker run-d -v /:/logtail_host:ro -v /var/run/docker.sock:/var/run/docker.sock --env 
    ALIYUN_LOGTAIL_CONFIG=/etc/ilogtail/conf/${your_region_name}/ilogtail_config.json 
    --env ALIYUN_LOGTAIL_USER_ID=${your_aliyun_user_id} --env
     ALIYUN_LOGTAIL_USER_DEFINED_ID=${your_machine_group_user_defined_id} registry.cn-hangzhou.aliyuncs.com/log-service/logtail
    ```

    **说明：** 请在配置参数前执行以下任意一种配置，否则删除其他container时可能出现错误`container text file busy`。

    -   Centos 7.4 及以上版本设置fs.may\_detach\_mounts = 1，相关说明请参见[Bug 1468249](https://bugzilla.redhat.com/show_bug.cgi?id=1468249)、[Bug 1441737](https://bugzilla.redhat.com/show_bug.cgi?id=1441737)和[issue 34538](https://github.com/moby/moby/issues/34538)。
    -   为Logtail授予`privileged`权限，启动参数中添加`--privileged`。详细内容请参考[docker run命令](https://docs.docker.com/engine/reference/run/)。
    |参数|参数说明|
    |--|----|
    | `${your_region_name}` |日志服务Project所在Region，请根据网络类型输入正确的格式。包括：     -   公网：` region-internet`。例如，华东一地域为`cn-hangzhou-internet`。
    -   阿里云内网：` region `。例如，华东一地域为`cn-hangzhou`。
 其中，各区域的安装参数请参考[表1 Logtail安装参数](intl.zh-CN/用户指南/Logtail采集/安装/安装Logtail（Linux系统）.md#table_eyz_pmv_vdb)，请根据Project地域选择正确的参数。

 |
    | `${your_aliyun_user_id}` |用户标识，请替换为您的阿里云主账号用户ID。主账号用户ID为字符串形式，如何查看ID请参考[为非本账号ECS、自建IDC配置主账号AliUid](intl.zh-CN/用户指南/Logtail采集/机器组/为非本账号ECS、自建IDC配置主账号AliUid.md)中的操作步骤1。|
    | `${your_machine_group_user_defined_id}` |您集群的机器组自定义标识。如您尚未开启自定义标识，请参考[创建用户自定义标识机器组](intl.zh-CN/用户指南/Logtail采集/机器组/创建用户自定义标识机器组.md)的步骤一，开启userdefined-id。|

    ``` {#codeblock_io3_lom_wjh}
    docker run -d -v /:/logtail_host:ro -v /var/run/docker.sock:/var/run/docker.sock 
    --env ALIYUN_LOGTAIL_CONFIG=/etc/ilogtail/conf/cn_hangzhou/ilogtail_config.json --env
     ALIYUN_LOGTAIL_USER_ID=1654218******--env ALIYUN_LOGTAIL_USER_DEFINED_ID=log-docker-demo registry.cn-hangzhou.aliyuncs.com/log-service/logtail
    ```


**说明：** 

您可以自定义配置Logtail容器的启动参数，只需保证以下前提条件：

1.  启动时，必须具备3个环境变量：`ALIYUN_LOGTAIL_USER_DEFINED_ID`、`ALIYUN_LOGTAIL_USER_ID`、`ALIYUN_LOGTAIL_CONFIG`。
2.  必须将Docker的Domain Socket挂载到`/var/run/docker.sock`。
3.  如果您需要采集容器标准输出、容器或宿主机文件，需要将根目录挂载到Logtail容器的`/logtail_host`目录。
4.  如果Logtail日志/usr/local/ilogtail/ilogtail.LOG中出现`The parameter is invalid : uuid=none`的错误日志，请在宿主机上创建一个product\_uuid文件，在其中输入任意合法UUID（例如`169E98C9-ABC0-4A92-B1D2-AA6239C0D261`），并把该文件挂载到Logtail容器的/sys/class/dmi/id/product\_uuid路径上。
5.  如果您的Docker Engine 开启了[live restore](https://docs.docker.com/config/containers/live-restore/) 选项，需添加额外的健康检查，防止docker engine重启时Logtail使用的DomainSocket无效。健康检查命令：curl --unix-socket /var/run/docker.sock http:/x \> /dev/null 2\>&1 

##  **步骤2 配置机器组**  {#section_gwf_kpv_vdb .section}

1.  开通日志服务并创建Project、Logstore，详细步骤请参考[准备流程](intl.zh-CN/用户指南/准备工作/准备流程.md)。
2.  在日志服务控制台的机器组列表页面单击[创建IP地址机器组](intl.zh-CN/用户指南/Logtail采集/机器组/创建IP地址机器组.md)。
3.  选择**用户自定义标识**，将您上一步配置的 `ALIYUN_LOGTAIL_USER_DEFINED_ID`填入**用户自定义标识**内容框中。

    ![](images/2677_zh-CN.png "配置机器组")


配置完成一分钟后，在**机器组列表**页面单击右侧的**查看状态**按钮，即可看到已经部署Logtail容器的心跳状态。具体参见[管理机器组](intl.zh-CN/用户指南/Logtail采集/机器组/管理机器组.md)中的**查看状态**部分。

##  **步骤3 创建采集配置**  {#section_kkf_4pv_vdb .section}

请根据您的需求在控制台创建Logtail采集配置，采集配置步骤请参考：

-    [容器内文本文件（推荐）](intl.zh-CN/用户指南/Logtail采集/容器日志采集/容器文本日志.md) 
-    [容器标准输出（推荐）](intl.zh-CN/用户指南/Logtail采集/容器日志采集/容器标准输出.md) 
-    [宿主机文本文件](intl.zh-CN/用户指南/Logtail采集/文本日志/采集文本日志.md) 

    默认宿主机根目录挂载到Logtail容器的`/logtail_host`目录，配置路径时，您需要加上此前缀。例如需要采集宿主机上`/home/logs/app_log/`目录下的数据，配置页面中日志路径设置为`/logtail_host/home/logs/app_log/`。

-    [Syslog](intl.zh-CN/用户指南/隐藏文件夹/Syslog.md) 

## 其他操作 {#section_sxz_4pv_vdb .section}

-   查看Logtail容器运行状态

    您可以执行命令`docker exec ${logtail_container_id} /etc/init.d/ilogtaild status`查看Logtail运行状态。

-   查看Logtail的版本号信息、IP、启动时间等

    您可以执行命令`docker exec ${logtail_container_id} cat /usr/local/ilogtail/app_info.json`查看Logtail相关信息。

-   查看Logtail的运行日志

    Logtail运行日志保存在`/usr/local/ilogtail/`目录下，文件名为`ilogtail.LOG`，轮转文件会压缩存储为`ilogtail.LOG.x.gz`。

    示例如下：

    ``` {#codeblock_4y7_odr_ff3}
    [root@iZbp17enxc2us3624wexh2Z ilogtail]# docker exec a287de895e40 tail -n 5 /usr/local/ilogtail/ilogtail.LOG
    [2018-02-06 08:13:35.721864]    [INFO]    [8]    [build/release64/sls/ilogtail/LogtailPlugin.cpp:104]    logtail plugin Resume:start
    [2018-02-06 08:13:35.722135]    [INFO]    [8]    [build/release64/sls/ilogtail/LogtailPlugin.cpp:106]    logtail plugin Resume:success
    [2018-02-06 08:13:35.722149]    [INFO]    [8]    [build/release64/sls/ilogtail/EventDispatcher.cpp:369]    start add existed check point events, size:0
    [2018-02-06 08:13:35.722155]    [INFO]    [8]    [build/release64/sls/ilogtail/EventDispatcher.cpp:511]    add existed check point events, size:0    cache size:0    event size:0    success count:0
    [2018-02-06 08:13:39.725417]    [INFO]    [8]    [build/release64/sls/ilogtail/ConfigManager.cpp:3776]    check container path update flag:0    size:1
    ```

    容器stdout并不具备参考意义，请忽略以下stdout输出：

    ``` {#codeblock_s75_fmt_sdn}
    
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

-   重启Logtail

    请参考以下示例重启Logtail。

    ``` {#codeblock_bgk_76p_3g7}
    [root@iZbp17enxc2us3624wexh2Z ilogtail]# docker exec a287de895e40 /etc/init.d/ilogtaild stop
    kill process Name: ilogtail pid: 7
    kill process Name: ilogtail pid: 8
    stop success
    [root@iZbp17enxc2us3624wexh2Z ilogtail]# docker exec a287de895e40 /etc/init.d/ilogtaild start
    ilogtail is running
    ```


