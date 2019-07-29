# Collect standard Docker logs {#concept_jc1_m4v_vdb .concept}

Logtail supports collecting standard Docker logs and uploading these logs together with the container-related metadata information to Log Service.

## Configuration process {#section_y5k_ffq_pdb .section}

![](images/2674_en-US.png "Configuration process")

1.  Deploy a Logtail container.
2.  Configure a Logtail machine group.

    Create a machine group with a custom ID in the Log Service console. No additional O&M is needed for further container cluster scaling.

3.  Create collection configurations for the server side.

    Create collection configurations in the Log Service console. All the collection configurations are for the server side. No local configuration is needed.


## Step 1. Deploy a Logtail container {#section_yqz_nfq_pdb .section}

1.  Pull the Logtail image.

    ``` {#codeblock_0es_vi3_myr}
    docker pull registry.cn-hangzhou.aliyuncs.com/log-service/logtail
    ```

2.  Start the Logtail container.

    Set the `${your_region_name}`, `${your_aliyun_user_id}`, and `${your_machine_group_user_defined_id}` parameters in the startup template.

    ``` {#codeblock_aun_frl_6wp}
    docker run-d -v /:/logtail_host:ro -v /var/run/docker.sock:/var/run/docker.sock --env 
    ALIYUN_LOGTAIL_CONFIG=/etc/ilogtail/conf/${your_region_name}/ilogtail_config.json 
    --env ALIYUN_LOGTAIL_USER_ID=${your_aliyun_user_id} --env
     ALIYUN_LOGTAIL_USER_DEFINED_ID=${your_machine_group_user_defined_id} registry.cn-hangzhou.aliyuncs.com/log-service/logtail
    ```

    **Note:** Take either of the following actions before setting the parameters. Otherwise, the `container text file busy` error may occur when you remove another container.

    -   For CentOS 7.4 and later, set fs.may\_detach\_mounts to 1. For more information, see [bug 1468249](https://bugzilla.redhat.com/show_bug.cgi?id=1468249), [bug 1441737](https://bugzilla.redhat.com/show_bug.cgi?id=1441737), and [issue 34538](https://github.com/moby/moby/issues/34538).
    -   Add the ``--privileged`` flag to the startup parameters. For more information, see [Docker run reference](https://docs.docker.com/engine/reference/run/).
    |Parameter|Description|
    |---------|-----------|
    |`${your_region_name}`|The region where the Log Service project is located. Set this parameter to an appropriate value according to the network type. Valid values:     -   For the Internet, specify the region in the `region-internet` format. For example, the value for the China \(Hangzhou\) region is `cn-hangzhou-internet`.
    -   For the Alibaba Cloud internal network, specify the region in the `region` format. For example, the value for the China \(Hangzhou\) region is `cn-hangzhou`.
 For more information about installation parameters in each region, see [Table 1 Logtail installation parameters](reseller.en-US/User Guide/Logtail collection/Install/Install Logtail in Linux.md#table_eyz_pmv_vdb). Set this parameter according to the region of the project.

 |
    |`${your_aliyun_user_id}`|The user ID. Set this parameter to the ID of your Alibaba Cloud account, which is of the String type. For more information about how to view the ID, see step 1 in [Configure AliUids for ECS servers under other Alibaba Cloud accounts or on-premises IDCs](reseller.en-US/User Guide/Logtail collection/Machine Group/Configure AliUids for ECS servers under other Alibaba Cloud accounts or on-premises IDCs.md).|
    |`${your_machine_group_user_defined_id}`|The custom ID of your cluster machine group. For more information about how to set the custom ID, see step 1 in [Create an ID to identify a machine group](reseller.en-US/User Guide/Logtail collection/Machine Group/Create an ID to identify a machine group.md).|

    ``` {#codeblock_io3_lom_wjh}
    docker run -d -v /:/logtail_host:ro -v /var/run/docker.sock:/var/run/docker.sock 
    --env ALIYUN_LOGTAIL_CONFIG=/etc/ilogtail/conf/cn_hangzhou/ilogtail_config.json --env
     ALIYUN_LOGTAIL_USER_ID=1654218******--env ALIYUN_LOGTAIL_USER_DEFINED_ID=log-docker-demo registry.cn-hangzhou.aliyuncs.com/log-service/logtail
    ```


**Note:** 

You can customize the startup parameter configurations of the Logtail container if the following conditions are met:

1.  You have the following three environment variables before starting the Logtail container: `ALIYUN_LOGTAIL_USER_DEFINED_ID`, `ALIYUN_LOGTAIL_USER_ID`, and `ALIYUN_LOGTAIL_CONFIG`.
2.  The domain socket of Docker is mounted to `/var/run/docker.sock`.
3.  To collect standard container output, container logs, or host files, mount the root directory to the `/logtail_host` directory of the Logtail container.
4.  If there is an error log `The parameter is invalid : uuid=none` in the Logtail log file /usr/local/ilogtail/ilogtail.LOG, create a product\_uuid file on the host. Write any legal UUID, for example, `169E98C9-ABC0-4A92-B1D2-AA6239C0D261`, to the file and mount the file to the /sys/class/dmi/id/product\_uuid directory of the Logtail container.
5.  If the [live restore](https://docs.docker.com/config/containers/live-restore/) setting is enabled for your Docker Engine, run the curl --unix-socket /var/run/docker.sock http:/x \> /dev/null 2\>&1 command before the Docker Engine is restarted to verify that the domain socket used by Logtail is valid.

## **Step 2. Configure a Logtail machine group** {#section_gwf_kpv_vdb .section}

1.  Activate Log Service, and then create a project and a Logstore. For more information, see [Preparation](reseller.en-US/User Guide/Preparation/Preparation.md).
2.  Click [Create a machine group with an IP address as its identifier](reseller.en-US/User Guide/Logtail collection/Machine Group/Create a machine group with an IP address as its identifier.md) on the Machine Groups page in the Log Service console.
3.  Select **Custom ID** from the Identification drop-down list. Enter the value of `ALIYUN_LOGTAIL_USER_DEFINED_ID` set in the previous step in the **Custom ID** field.

    ![](images/2677_en-US.png "Configure the Logtail machine group")


Click Confirm to create the machine group. One minute later, click **Status** on the right of the **Machine Groups** page to view the heartbeat status of the deployed Logtail container. For more information, see the **View status** section in [Manage a machine group](reseller.en-US/User Guide/Logtail collection/Machine Group/Manage a machine group.md).

## **Step 3. Create collection configurations** {#section_kkf_4pv_vdb .section}

Create Logtail collection configurations in the console as needed. For more information, see:

-   [Container text logs \(recommended\)](reseller.en-US/User Guide/Logtail collection/Container log collection/Container text logs.md)
-   [Container standard output \(recommended\)](reseller.en-US/User Guide/Logtail collection/Container log collection/Container stdout.md)
-   [Host text files](reseller.en-US/User Guide/Logtail collection/Text logs/Collect text logs.md) 

    By default, the root directory of a host is mounted to the `/logtail_host` directory of the Logtail container. You need to prefix the configuration path with /logtail\_host. For example, to collect data in the `/home/logs/app_log/` directory of the host, set the log path on the configuration page as `/logtail_host/home/logs/app_log/`.

-   [Syslog](reseller.en-US//Syslog.md)

## Other operations {#section_sxz_4pv_vdb .section}

-   Check the running status of the Logtail container

    You can run the `docker exec ${logtail_container_id} /etc/init.d/ilogtaild status` command to check the running status of Logtail.

-   View the version number, IP address, and startup time of Logtail

    You can run the `docker exec ${logtail_container_id} cat /usr/local/ilogtail/app_info.json` command to view relevant information about Logtail.

-   View the operational logs of Logtail

    Logtail operational logs are stored in the `ilogtail.LOG` file in the `/usr/local/ilogtail/` directory. If the log file is rotated and compressed, it is stored as `ilogtail.LOG.x.gz`.

    Example:

    ``` {#codeblock_4y7_odr_ff3}
    [root@iZbp17enxc2us3624wexh2Z ilogtail]# docker exec a287de895e40 tail -n 5 /usr/local/ilogtail/ilogtail.LOG
    [2018-02-06 08:13:35.721864]    [INFO]    [8]    [build/release64/sls/ilogtail/LogtailPlugin.cpp:104]    logtail plugin Resume:start
    [2018-02-06 08:13:35.722135]    [INFO]    [8]    [build/release64/sls/ilogtail/LogtailPlugin.cpp:106]    logtail plugin Resume:success
    [2018-02-06 08:13:35.722149]    [INFO]    [8]    [build/release64/sls/ilogtail/EventDispatcher.cpp:369]    start add existed check point events, size:0
    [2018-02-06 08:13:35.722155]    [INFO]    [8]    [build/release64/sls/ilogtail/EventDispatcher.cpp:511]    add existed check point events, size:0    cache size:0    event size:0    success count:0
    [2018-02-06 08:13:39.725417]    [INFO]    [8]    [build/release64/sls/ilogtail/ConfigManager.cpp:3776]    check container path update flag:0    size:1
    ```

    The container standard output is not for reference. Ignore the following standard output:

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

-   Restart Logtail

    The following sample code shows how to restart Logtail:

    ``` {#codeblock_bgk_76p_3g7}
    [root@iZbp17enxc2us3624wexh2Z ilogtail]# docker exec a287de895e40 /etc/init.d/ilogtaild stop
    kill process Name: ilogtail pid: 7
    kill process Name: ilogtail pid: 8
    stop success
    [root@iZbp17enxc2us3624wexh2Z ilogtail]# docker exec a287de895e40 /etc/init.d/ilogtaild start
    ilogtail is running
    ```


