# Configure startup parameters {#concept_sdg_czb_wdb .concept}

This document describes the Logtail startup configuration parameters. You can configure the startup parameters by following this document when you have any special requirements.

## Scenarios {#section_gqn_4zb_wdb .section}

In the following scenarios, you must configure the Logtail startup configuration parameters:

-   The metadata information of each file,  such as file signature, collection location, and file name, must be maintained in the memory. 
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

|Parameter name|Parameter description|Value|
|:-------------|:--------------------|:----|
|cpu\_usage\_limit |The CPU usage threshold. Calculated per core.  In most cases, the single-core processing capability is about 24 MB/s in simple mode and about 12 MB/s

| Double type. The minimum value is 0.1, and the maximum value is the number of CPU cores of the current machine. The default value is 2.

 For example, the value 0.4 indicates the CPU usage of Logtail is limited to 40% of single-core CPUs. Logtail restarts automatically when the threshold is exceeded. 

 |
|mem\_usage\_limit |The usage threshold of resident memory. To collect more than 1,000 distinct files, properly increase the threshold value.

| Int type. Measured in MBs.  The minimum value is 128, and the maximum value is the current machine effective memory value. The default value is 2048.

 For example, the value 100 indicates the memory usage of Logtail is limited to 100 MB. Logtail restarts automatically when the threshold is exceeded. 

 |
|max\_bytes\_per\_sec | The traffic limit on the raw data sent by Logtail, more than 20MB/s stream is not limited.| Int type. Measured in bytes per second.  The range is 1024 - 52428800, the default value is 20971520.

 For example, the value 2,097,152 indicates the data transfer rate of Logtail is limited to 2 MB/s.

 |
|process\_thread\_count |The number of threads that Logtail processes written data of log files.   Generally supports a write speed of 24 MB/s in simple mode and 12 MB/s in full mode.  By default, it is not required to adjust this value, but you can increase the threshold value when necessary.

|Int type. Measured in units. The range is 1 - 64, the default value is 1.|
|send\_request\_concurrency |The number of asynchronous concurrency.  By default, Logtail sends data packets asynchronously. You can set a larger asynchronous concurrency value if the write TPS is large.Can be supported with a single concurrency of 0.5 Mb/s ~ It is based on the network delay to calculate the throughput of 1 Mb/s network.

**Note:** Quantity based on the condition that one concurrency supports 0.5–1 MB/s network throughout. The actual concurrency quantity varies with network delay.

| Int type. Measured in units. The range is 1 - 1000, default value is 20.

 |
|streamlog\_open |Whether to enable the syslog collection function.  For more information, see [Reference for collecting syslog data](intl.en-US/User Guide/Logtail collection/Data Source/Reference for collecting syslog data.md).|Bool type, wherein:-   true indicates that the syslog collection feature is enabled.
-   false indicates that the sysylog collection feature is disabled.

The default value is false.|
|streamlog\_pool\_size\_in\_mb |Size of the cache storing received syslog data.  Logtail requests a specified size of memory at one time when started. Configure the size according to the memory size of your machine and your actual requirements.|Int type. Measured in MBs.  The range is 10 - 2048, default value is  50.|
|streamlog\_rcv\_size\_each\_call |Size of the buffer Logtail uses when calling the Linux socket rcv interface. Measured in bytes. You can increase the value if the syslog traffic is heavy.  |Int type. Measured in bytes. The range is 32 - 524288, default value is 1024.|
|streamlog\_formats |The method of parsing received syslogs.  For more information, see [Reference for collecting syslog data](intl.en-US/User Guide/Logtail collection/Data Source/Reference for collecting syslog data.md).|The parameter is empty by default.|
|streamlog\_tcp\_addr |The binding address that Logtail uses to receive syslogs. For more information, see [Reference for collecting syslog data](intl.en-US/User Guide/Logtail collection/Data Source/Reference for collecting syslog data.md).|The default value is 0.0.0.0. |
|streamlog\_tcp\_port |The TCP port that Logtail uses to receive syslogs. |Can be set to any valid port number that is occupied. The default value is 11111.|
|buffer\_file\_num |When a network exception occurs or the writing quota is exceeded, Logtail writes the logs that are parsed in real time to a local file \(located in the installation directory\) as a cache and then tries to resend the logs to Log Service after the recovery.  This parameter indicates the maximum number of cached files.|Int type. Measured in units. The range is 1 - 100, default value is 25.|
|buffer\_file\_size |The maximum number of bytes that a cached file allows. The \(buffer\_file\_num \* buffer\_file\_size\) indicates the maximum disk space available for cached files. |Int type. Measured in bytes. The range is 1048576 - 104857600, the default is 20971520 Bytes \(20 MB\).|
|buffer\_file\_path |The directory that stores cached files. After modifying this parameter value, you must manually move the files named in the format of  logtail\\\_buffer\\\_file\_\* in the old cache directory to the new cache directory so that  Logtail can read the cached files and delete them after sending logs.|The default value is null, indicating the cached files are stored in the Logtail installation directory \(/usr/local/ilogtail\).|
|bind\_interface |The name of the network card that is bound to the local machine, such as eth1  \(only Linux versions are supported\).|The parameter is empty by default. The available network card is bound automatically. If this parameter is configured, Logtail will force to use this network card to upload logs.|
|check\_point\_filename |The full path stored by the checkpoint file, which is used to customize the checkpoint storage location of Logtail.    We recommend that Docker users modify this file storage address and mount the directory where the checkpoint file resides to the host. Otherwise, duplicate collection occurs when the container is released because the checkpoint information is missing.  For example, configure the check\_point\_filename in Docker as /data/logtail/check\_point.dat, and add -v /data/docker1/logtail:/data/logtail in the Docker startup command to mount the /data/docker1/logtail directory of the host to the /data/logtail directory of Docker.

|The default value is /tmp/logtail\_check\_point. |

**Note:** 

-   The preceding table only lists the common startup parameters that need your attention. If ilogtail\_config.json  has parameters that are not listed in the table, the default values are applied.
-   Add or modify the values of configuration parameters as per your needs. Unnecessary configuration parameters \(for example, parameters related to the collection of syslog data streams\) do not need to be added to ilogtail\_config.json.

## Modify configurations {#section_n5z_dkb_ry .section}

1.  Configure ilogtail\_config.json as per your needs. 

    Confirm the modified configurations are in the valid JSON format.

2.  Restart Logtail to apply the modified configurations.

    ```
    /etc/init.d/ilogtaild stop
    /etc/init.d/ilogtaild start
    /etc/init.d/ilogtaild status
    ```


