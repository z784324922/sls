# Configure startup parameters {#concept_sdg_czb_wdb .concept}

This document describes the Logtail startup configuration parameters. You can configure the startup parameters by following this document when you have any special requirements.

## Scenarios {#section_gqn_4zb_wdb .section}

In the following scenarios, you must configure the Logtail startup configuration parameters:

-   The metadata information of each file, such as file signature, collection location, and file name, must be maintained in the memory. Therefore, the memory usage may be high if a large number of log files are to be collected. such as file signature, collection location, and file name, must be maintained in the memory. 
-   Therefore, the memory usage may be high if a large number of log files are to be collected.
-   The CPU usage is high because the volume of log data is large and the traffic sent to Log Service is heavy.
-   Syslog/TCP data streams are to be collected.

## Startup configuration {#section_at3_rjb_ry .section}

-   File path

    ```
     /usr/local/ilogtail/ilogtail_config.json  
    ```

-   File format

    JSON 

-   File sample \(which only shows partial configuration items\)

    ```
    {
        ...
        "cpu_usage_limit" : 0.4, 
        "mem_usage_limit" : 100, 
        "max_bytes_per_sec" : 2097152, 
        "process_thread_count" : 1, 
        "send_request_concurrency" : 4, 
        "streamlog_open" : false, 
        "streamlog_pool_size_in_mb" : 50, 
        "streamlog_rcv_size_each_call" : 1024, 
        "streamlog_formats":[], 
        "streamlog_tcp_port" : 11111, 
        "buffer_file_num" : 25, 
        "buffer_file_size" : 20971520, 
        "buffer_file_path" : "", 
        ...
    }
    ```


## Common configuration parameters  {#section_zdg_xjb_ry .section}

|Parameter name |Value|Description|
|:--------------|:----|:----------|
|cpu\_usage\_limit |The CPU usage threshold. Double type. Calculated per core. |For example, the value 0.4 indicates the CPU usage of Logtail is limited to 40% of single-core CPUs. Logtail restarts automatically when the threshold is exceeded.  In most cases, the single-core processing capability is about 24 MB/s in simple mode and about 12 MB/s in full mode.|
|mem\_usage\_limit |The usage threshold of resident memory. Int type. Measured in MBs. |For example, the value 100 indicates the memory usage of Logtail is limited to 100 MB.  Logtail restarts automatically when the threshold is exceeded.  To collect more than 1,000 distinct files, properly increase the threshold value.|
|max\_bytes\_per\_sec |The traffic limit on the raw data sent by Logtail. Int type. Measured in bytes per second. |For example, the value 2,097,152 indicates the data transfer rate of Logtail is limited to 2 MB/s.|
|process\_thread\_count |The number of threads that Logtail processes written data of log files.  |The default value is 1, which generally supports a write speed of 24 MB/s in simple mode and 12 MB/s in  full mode.  Increase the threshold value only when necessary.|
|send\_request\_concurrency |By default, Logtail sends data packets asynchronously. You can set a larger asynchronous concurrency value if the write TPS is large.|By default, four asynchronous concurrencies are available. You can calculate the concurrency **Note:** quantity based on the condition that one concurrency supports 0.5–1 MB/s network throughout. The actual concurrency quantity varies with network delay.

|
|streamlog\_open |Whether or not to enable the syslog reception function. Bool type. |false indicates to disable the function. true indicates to enable the function.  For more information, see [EN-US\_TP\_13068.md](intl.en-US/User Guide/Logtail collection/Data Source/Syslog.md). |
|streamlog\_pool\_size\_in\_mb |Size of the cache storing received syslog data.  Measured in MBs. |The size of the memory pool that syslog uses to receive logs. Logtail requests a specified size of memory at one time when started. Configure the size according to the memory size of your machine and your actual requirements.|
|streamlog\_rcv\_size\_each\_call |Size of the buffer Logtail uses when calling the Linux socket rcv interface. Measured in bytes. You can increase the value if the syslog traffic is heavy. |The recommended value range is 1024–8192.|
|streamlog\_formats |The method of parsing received syslogs. |For more information, see [EN-US\_TP\_13068.md](intl.en-US/User Guide/Logtail collection/Data Source/Syslog.md).|
|streamlog\_tcp\_addr |The binding address that Logtail uses to receive syslogs. The default value is 0.0.0.0.|For more information, see [Reference for collecting syslog data](LogService_user_guide_0033.md).|
|streamlog\_tcp\_port |The TCP port that Logtail uses to receive syslogs. |The default value is 11111.|
|buffer\_file\_num |When a network exception occurs or the writing quota is exceeded, Logtail writes the logs that are parsed in real time to a local file \(located in the installation directory\) as a cache and then tries to resend the logs to Log Service after the recovery.  This parameter indicates the maximum number of cached files. |The default value is 25 for public cloud users.|
|buffer\_file\_size |The maximum number of bytes that a cached file allows. The \(buffer\_file\_num \* buffer\_file\_size\) indicates the maximum disk space available for cached files. |The default value is 20,971,520 \(20 MB\).|
|buffer\_file\_path |The directory that stores cached files. After modifying this parameter value, you must manually move the files named in the format of  logtail\\\_buffer\\\_file\_\* in the old cache directory to the new cache directory so that  Logtail can read the cached files and delete them after sending logs.|The default value is null, indicating the cached files are stored in the Logtail installation directory \(/usr/local/ilogtail\).|
|bind\_interface |The name of the network card that is bound to the local machine, such as eth1  \(only Linux versions are supported\).|By default, the available network card is bound automatically. If this parameter is configured, Logtail will force to use this network card to upload logs.|
|check\_point\_filename |The full path stored by the checkpoint file, which is used to customize the checkpoint storage location of Logtail. |The default value is /tmp/logtail\_check\_point.    We recommend that Docker users modify this file storage address and mount the directory where the checkpoint file resides to the host. Otherwise, duplicate collection occurs when the container is released because the checkpoint information is missing. For example, configure the check\_point\_filename in Docker as /data/logtail/check\_point.dat, and add -v /data/docker1/logtail:/data/logtail in the Docker startup command to mount the /data/docker1/logtail directory of the host to the /data/logtail directory of Docker. For example, configure the check\_point\_filename  in Docker as `/data/logtail/check_point.dat`, and add `-v  /data/docker1/logtail:/data/logtail`  in the Docker startup command to mount the /data/docker1/logtail directory of the host to the /data/logtail directory of Docker.|

**Note:** 

-   The preceding table only lists the common startup parameters that need your attention. If ilogtail\_config.json  has parameters that are not listed in the table, the default values are applied.
-   Add or modify the values of configuration parameters as per your needs. Unused configuration parameters \(for example, parameters related to the collection of syslog data streams\) do not need to be added to ilogtail\_config.json.

## Modify configurations  {#section_n5z_dkb_ry .section}

1.  Configure ilogtail\_config.json as per your needs. 

    Confirm the modified configurations are in the valid JSON format.

2.  Restart Logtail to apply the modified configurations.

    ```
    /etc/init.d/ilogtaild stop
    /etc/init.d/ilogtaild start
    /etc/init.d/ilogtaild status
    ```


