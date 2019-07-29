# Set startup parameters {#concept_sdg_czb_wdb .concept}

This topic describes how to set the Logtail startup parameters. You can refer to this topic for parameter setting as needed.

## Scenarios {#section_gqn_4zb_wdb .section}

In the following scenarios, you need to set the Logtail startup parameters:

-   A large number of log files are to be collected. They may occupy a large amount of memory. The metadata of each file, such as the file signature, collection location, and file name, needs to be maintained in memory.
-   A high volume of log data leads to a high CPU usage.
-   A high volume of log data leads to heavy traffic sent to Log Service.

## Startup configurations {#section_at3_rjb_ry .section}

-   File path:

    ``` {#codeblock_91k_e67_qfe}
    /usr/local/ilogtail/ilogtail_config.json
    ```

-   File format:

    JSON

-   File sample \(only partial configuration items are shown\):

    ``` {#codeblock_li3_uir_0ez}
    {
        ...
        "cpu_usage_limit" : 0.4,
        "mem_usage_limit" : 100,
        "max_bytes_per_sec" : 2097152,
        "process_thread_count" : 1,
        "send_request_concurrency" : 4,
        "buffer_file_num" : 25,
        "buffer_file_size" : 20971520,
        "buffer_file_path" : "",
        ...
    }
    ```


## Common configuration parameters {#section_zdg_xjb_ry .section}

|Parameter|Description|Value|
|:--------|:----------|:----|
|cpu\_usage\_limit|The CPU usage threshold, which is calculated by core. In most cases, the single-core processing capability is about 24 Mbit/s in simple mode and about 12 Mbit/s in full mode. For more information, see

 | The value is of the Double type. Valid values: \[0.1, the number of CPU cores of the current machine\]. Default value: 2.

 For example, the value 0.4 indicates that the CPU usage of Logtail is limited to 40% of single-core CPUs. Logtail restarts automatically when the threshold is exceeded.

 |
|mem\_usage\_limit|The usage threshold of resident memory. To collect more than 1,000 distinct files, increase the threshold value properly.

 | The value is of the Int type. Unit: MB. Valid values: \[128, the valid memory value of the current machine\]. Default value: 2048.

 For example, the value 100 indicates that the memory usage of Logtail is limited to 100 MB. Logtail restarts automatically when the threshold is exceeded.

 |
|max\_bytes\_per\_sec|The traffic limit on the raw data sent by Logtail. Traffic exceeding 20 MB/s is not throttled.| The value is of the Int type. Unit: Byte/s. Valid values: \[1024, 52428800\]. Default value: 20971520.

 For example, the value 2097152 indicates that the data transfer rate of Logtail is limited to 2 MB/s.

 |
|process\_thread\_count|The number of threads with which Logtail processes written data of log files. Generally, Logtail supports a write speed of 24 Mbit/s in simple mode and 12 Mbit/s in full mode. By default, there is no need to modify this value, but you can increase the threshold value when necessary.

 |The value is of the Int type. Valid values: \[1, 64\]. Default value: 1.|
|send\_request\_concurrency|The asynchronous concurrency. By default, Logtail sends data packets asynchronously. You can set a larger asynchronous concurrency value if the write TPS is large. A single concurrency occupies 0.5 to 1 Mbit/s network throughput, depending on the network delay.

 **Note:** A large value will lead to excessive network port usage. In this case, you need to modify relevant TCP parameters.

 | The value is of the Int type. Valid values: \[1, 1000\]. Default value: 20.

 |
|buffer\_file\_num|The maximum number of cached files. When a network exception occurs and the writing quota is exceeded, Logtail writes the logs that are parsed in real time to local files in the installation directory, and then tries to resend the logs to Log Service after the recovery.|The value is of the Int type. Valid values: \[1, 100\]. Default value: 25.|
|buffer\_file\_size|The maximum number of bytes that each cached file allows. The product of the value of buffer\_file\_num and that of buffer\_file\_size indicates the maximum disk space available for cached files.|The value is of the Int type. Unit: Byte. Valid values: \[1048576, 104857600\]. Default value: 20971520, which is 20 MB.|
|buffer\_file\_path|The directory that stores cached files. After modifying this parameter, you need to move the files named in the format of logtail\\\_buffer\\\_file\_\* in the old cache directory to the new directory so that Logtail can read the cached files and delete them after sending logs.|By default, the value is an empty string. In this case, the cached files are stored in the Logtail installation directory /usr/local/ilogtail.|
|bind\_interface|The name of the NIC bound to the local machine. For example, eth1. This parameter is valid only for Logtail for Linux.|By default, the value is an empty string. The available NIC is bound automatically. If this parameter is set, Logtail uses the specified NIC to upload logs.|
|check\_point\_filename|The full path for storing the checkpoint file of Logtail. We recommend that Docker users modify this file storage path and mount the directory where the checkpoint file resides to the host. Otherwise, duplicate collection occurs when the container is released due to checkpoint information loss. For example, set check\_point\_filename to `/data/logtail/check_point.dat` in Docker, and add `-v /data/docker1/logtail:/data/logtail` to the Docker startup command to mount the /data/docker1/logtail directory of the host to the /data/logtail directory of Docker.

 |Default value: /tmp/logtail\_check\_point.|
|`user_config_file_path`|The full path for storing the collection configuration file of Logtail. We recommend that Docker users modify this file storage path and mount the directory where the collection configuration file resides to the host. Otherwise, duplicate collection occurs when the container is released due to checkpoint information loss.

 For example, set `user_config_file_path` to /data/logtail/user\_log\_config.json in Docker, and add `-v /data/docker1/logtail:/data/logtail` to the Docker startup command to mount the /data/docker1/logtail directory of the host to the /data/logtail directory of Docker.

 |By default, the user\_log\_config.json file is stored in the directory where the binary process is located.|
|`discard_old_data`|Specifies whether to discard historical logs. A value of true indicates that logs generated more than 12 hours ago will be discarded.|The value is of the Boolean type. Default value: true.|
|`working_ip`|The local IP address reported by Logtail. If the value is an empty string, Logtail automatically obtains the IP address of the local machine.|The value is an IP address. By default, the value is an empty string.|
|`working_hostname`|The local hostname reported by Logtail. If the value is an empty string, Logtail automatically obtains the hostname of the local machine.|The value is of the String type. By default, the value is an empty string.|
|`max_read_buffer_size`|The maximum size of a log, in Bytes. If the size of a single log exceeds 512 KB, you can adjust the parameter value.|The value is of the Long type. Default value: 524288, which is 512 KB.|
|oas\_connect\_timeout|The connection timeout period when Logtail sends a request, for example, to obtain the configuration or AccessKey. This parameter applies to scenarios where the connection takes a long period of time due to poor network conditions.

 |The value is of the Long type. Unit: second. Default value: 5.|
|oas\_request\_timeout|The total timeout period when Logtail sends a request, for example, to obtain the configuration or AccessKey. This parameter applies to scenarios where the connection takes a long period of time due to poor network conditions.

 |The value is of the Long type. Unit: second. Default value: 10.|

**Note:** 

-   The preceding table only lists the common startup parameters. If the ilogtail\_config.json file contains parameters that are not listed in the table, use the default settings.
-   Add parameters or modify the values of existing parameters as needed. Do not add unnecessary parameters to the ilogtail\_config.json file.

## Modify configurations {#section_n5z_dkb_ry .section}

1.  Modify the ilogtail\_config.json file as needed.

    Ensure that the modified configurations are in the valid JSON format.

2.  Restart Logtail for the modified configurations to take effect.

    ``` {#codeblock_u7p_ol5_xc4}
    /etc/init.d/ilogtaild stop
    /etc/init.d/ilogtaild start
    /etc/init.d/ilogtaild status
    ```


