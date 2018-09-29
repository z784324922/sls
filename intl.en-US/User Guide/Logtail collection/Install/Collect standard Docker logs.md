# Collect standard Docker logs {#concept_jc1_m4v_vdb .concept}

Logtail supports collecting standard Docker logs and uploading these logs together with the container-related metadata information to Log Service.

## Configuration process {#section_y5k_ffq_pdb .section}

![](images/2674_en-US.png "Configuration process")

1.  Deploy a Logtail container.
2.  Configure the Logtail container.

    In the logging service control panel, create a machine group with a custom ID. In the future this container will not need further O&M to expand or contract.

3.  Create a new configuration for collection on the server side.

    Create collection configurations in the Log Service console. All the collection configurations are for the server side. No local configuration is needed.


## Step 1. Deploy the Logtail container {#section_yqz_nfq_pdb .section}

1.  Pull the Logtail image.

    ```
    docker pull registry.cn-hangzhou.aliyuncs.com/log-service/logtail
    ```

2.  Start the Logtail container.

    Replace the following three parameters in the startup template: `${your_region_name}`, `${your_aliyun_user_id}`, and `${your_machine_group_user_defined_id}`.

    ```
    docker run-d -v /:/logtail_host:ro -v /var/run/docker.sock:/var/run/docker.sock --env 
    ALIYUN_LOGTAIL_CONFIG=/etc/ilogtail/conf/${your_region_name}/ilogtail_config.json 
    --env ALIYUN_LOGTAIL_USER_ID=${your_aliyun_user_id} --env
     ALIYUN_LOGTAIL_USER_DEFINED_ID=${your_machine_group_user_defined_id} registry.cn-hangzhou.aliyuncs.com/log-service/logtail
    ```

    **Note:** Please perform any of the following configurations before the configuration parameters, otherwise, there may be an error in removing the other container, `container text file busy`。

    -   Centos version 7.4 and later sets up FS. may\_detach\_mounts = 1, see Bug 1468249, bug 1441737, and issue 34538 for instructions.
    -   Give logtail a `privileged` permission, and add -`--privileged` to the startup parameter. For more information, see [docker run command](https://docs.docker.com/engine/reference/run/).
    |Parameter|Description|
    |---------|-----------|
    |`${your_region_name}`|The region name. Replace it with the region where your created Log Service project resides.  For the region name, see the [Linux ](intl.en-US/User Guide/Logtail collection/Install/Linux .md) used in Install Logtail. **Note:** We recommend that you copy the region name directly from the list.

|
    |`${your_aliyun_user_id}`|User ID, replace it with the ID of your main Alibaba Cloud account. The user identification. Replace it with the ID of your Alibaba Cloud main account, which is in the string format. For how to check the ID, see 2.1 in [Collect logs from non-Alibaba Cloud ECS instances or ECS instances not in your account](intl.en-US/User Guide/Logtail collection/Machine Group/Collect logs from non-Alibaba Cloud ECS instances or ECS instances not in your account.md).|
    |`${your_machine_group_user_defined_id}`|The user-defined identity of your cluster machine group.  If user-defined identity is not enabled yet, enable userdefined-id by following the corresponding steps in [Configure a user-defined identity for a machine group](intl.en-US/User Guide/Logtail collection/Machine Group/Configure a user-defined identity for a machine group.md).|

    ```
    docker run -d -v /:/logtail_host:ro -v /var/run/docker.sock:/var/run/docker.sock 
    --env ALIYUN_LOGTAIL_CONFIG=/etc/ilogtail/conf/cn_hangzhou/ilogtail_config.json --env
     ALIYUN_LOGTAIL_USER_ID=1654218******--env ALIYUN_LOGTAIL_USER_DEFINED_ID=log-docker-demo registry.cn-hangzhou.aliyuncs.com/log-service/logtail
    ```


**Note:** 

You can customize the startup parameter configurations of Logtail containers if the following conditions are met:

1.  You have the following three environment variables when starting the Logtail containers: `ALIYUN_LOGTAIL_USER_DEFINED_ID`, `ALIYUN_LOGTAIL_USER_ID`, and `ALIYUN_LOGTAIL_CONFIG`.
2.  The domain socket of Docker is mounted to `/var/run/docker.sock`.
3.  If you want to collect logs from other containers or host files, the root directory is mounted to the `/logtail_host` directory of Logtail containers.
4.  If there is a error log `The parameter is invalid : uuid=none` in logtail logs /usr/local/ilogtail/ilogtail.LOG, create a product\_uuid file on the host machine, then you enter any legal UUID \(for example, `169E98C9-ABC0-4A92-B1D2-AA6239C0D261` \), and mount the file to the /sys/class/dmi/id/product\_uuid path of the logtail container.

## **Step 2. Configure machine group** {#section_gwf_kpv_vdb .section}

1.  Activate Log Service, and create Project and Logstore. For more information, see [Preparation](intl.en-US/User Guide/Preparation/Preparation.md).
2.  Click [Create a machine group](intl.en-US/User Guide/Logtail collection/Machine Group/Create a machine group.md) on the Machine Groups page in the Log Service console.
3.  Select **User-defined Identity** from the Machine Group Identification drop-down list. Enter the `ALIYUN_LOGTAIL_USER_DEFINED_ID`configured in the previous step in the User-defined Identity field.

    ![](images/2677_en-US.png "Configuring the machine group")


Click Confirm to create the machine group. One minute later, click **Machine Status** at the right of the machine group on the **Machine Groups** page to view the heartbeat status of the deployed Logtail container. For more information, see **View status** in [Manage a machine group](intl.en-US/User Guide/Logtail collection/Machine Group/Manage a machine group.md).

## **Step 3. Create collection configurations** {#section_kkf_4pv_vdb .section}

Create Logtail collection configurations in the console as needed. For how to create collection configurations, see:

-   [Container text log \(recommended\)](intl.en-US/User Guide/Logtail collection/Data Source/Container text logs.md)
-   [Container standard output \(recommended\)](intl.en-US/User Guide/Logtail collection/Data Source/Containers-standard output.md)
-   [Host text file](intl.en-US/User Guide/Logtail collection/Data Source/Collect text logs.md)

    By default, the root directory of a host is mounted in the `/logtail_host` directory of the Logtail container. You need to prefix the configuration path with /logtail\_host.  For example, to collect data in the `/home/logs/app_log/` directory of the host, you must set the log path on the configuration page to `/logtail_host/home/logs/app_log/`.

-   [Syslog](intl.en-US//Syslog.md)

## Other operations {#section_sxz_4pv_vdb .section}

-   Check the running status of the Logtail container

    You can run the `docker exec ${logtail_container_id} /etc/init.d/ilogtaild status` command to check the running status of Logtail.

-   Check version number, IP, and startup time of Logtail

    You can run the `docker exec ${logtail_container_id} cat /usr/local/ilogtail/app_info.json`command to check the information related to Logtail.

-   Check Logtail running logs

    Logtail running logs are stored in the `/usr/local/ilogtail/` directory. The file name is `ilogtail.LOG`. The rotation file is compressed and stored as `ilogtail.LOG.x.gz`.

    For example:

    ```
    [root@iZbp17enxc2us3624wexh2Z ilogtail]# docker exec a287de895e40 tail -n 5 /usr/local/ilogtail/ilogtail.LOG
    [2018-02-06 08:13:35.721864] [INFO] [8] [build/release64/sls/ilogtail/LogtailPlugin.cpp:104] logtail plugin Resume:start
    [2018-02-06 08:13:35.722135] [INFO] [8] [build/release64/sls/ilogtail/LogtailPlugin.cpp:106] logtail plugin Resume:success
    [2018-02-06 08:13:35.722149] [INFO] [8] [build/release64/sls/ilogtail/EventDispatcher.cpp:369] start add existed check point events, size:0
    [2018-02-06 08:13:35.722155] [INFO] [8] [build/release64/sls/ilogtail/EventDispatcher.cpp:511] add existed check point events, size:0 cache size:0 event size:0 success count:0
    [2018-02-06 08:13:39.725417] [INFO] [8] [build/release64/sls/ilogtail/ConfigManager.cpp:3776] check container path update flag:0 size:1
    ```

    The container stdout is not of reference significance, ignore the following stdout output:

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

-   Restart Logtail

    See the following example to restart Logtail:

    ```
    [root@iZbp17enxc2us3624wexh2Z ilogtail]# docker exec a287de895e40 /etc/init.d/ilogtaild stop
    kill process Name: ilogtail pid: 7
    kill process Name: ilogtail pid: 8
    stop success
    [root@iZbp17enxc2us3624wexh2Z ilogtail]# docker exec a287de895e40 /etc/init.d/ilogtaild start
    ilogtail is running
    ```


