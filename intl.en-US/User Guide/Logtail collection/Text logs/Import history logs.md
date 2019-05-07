# Import history logs {#task_g1x_q2s_g2b .task}

Logtail only collects incremental logs by default. If you want to import history logs, use the history log importing feature of Logtail.

-   Your Logtail must be v0.16.15 \(Linux\) or v1.0.0.1 \(Windows\) and later. If your Logtail is an earlier version, you must upgrade it to the latest version.
-   Target history logs do not have to be covered by the collection configuration. If a log file has been collected by Logtail, Logtail will collect the log file again after you import history logs.
-   The maximum interval between generating and importing local configurations is one minute.
-   Due to the special action of loading local configurations, Logtail notifies you of this action by sending `LOAD_LOCAL_EVENT_ALARM` to your server.
-   If you want to import a large number of log files, you must modify the startup parameters for Logtail. That is, increase the upper limit of the Logtail CPU and memory. We commend that you increase the CPU to 2.0 or any other greater specification, and increase the memory to 512 MB or any other greater specification. For more information, see [Configure startup parameters](reseller.en-US/User Guide/Logtail collection/Install/Configure startup parameters.md).

Logtail collects logs based on the events that are detected by listening on or performing round robin for log modifications. Logtail can also load local configurations, and trigger log collections. Logtail collects history logs by loading local configurations.

You must perform the operations for importing history logs in the Logtail installation directory, which varies by operating systems.

-   Linux operating system: /usr/local/ilogtail.
-   Windows operating system
    -   32-bit Windows operating system: C:\\Program Files\\Alibaba\\Logtail
    -   64-bit Windows operating system: C:\\Program Files \(x86\)\\Alibaba\\Logtail

1.  Create collection configurations Configure the collection and apply the configuration to the machine group. If you only want to configure the collection for importing history logs, you can set a collection directory that does not exist. For more information about the collection configuration, see [Collect text logs](reseller.en-US/User Guide/Logtail collection/Text logs/Collect text logs.md).
2.  Gets the configuration unique identity. Obtain a unique identifier for the configuration from the user\_log\_config.json file in the Logtail installation directory. For more information, see [Logtail installation directory](#). For the Linux operating system, run the grep command in this directory. For the Windows operating system, use a tool \(for example, the text tool\) to open the file. To view the identifier in the Linux operating system, do the following:

    ```
    grep "##" /usr/local/ilogtail/user_log_config.json | awk '{print $1}'
                        "##1.0##log-config-test$multi"
                        "##1.0##log-config-test$ecs-test"
                        "##1.0##log-config-test$metric_system_test"
                        "##1.0##log-config-test$redis-status"
    					
    ```

3.  Add local events. Save local events to JSON file local\_event.json in the Logtail installation directory \(for more information, see [Logtail installation directory](#)\). You must save the local events by using the following format:

    ```
    [ 
      {
        "config" : "${your_config_unique_id}",
        "dir" : "${your_log_dir}",
        "name" : "${your_log_file_name}"
       },
      {
       ...
       }
       ...
    ]
    					
    ```

    -   **Configuration items** 

        |Configuration items|Description:|Examples|
        |:------------------|:-----------|:-------|
        |Config|Indicates the configuration unique identifier that is obtained in step 2.|\#\#1.0\#\#log-config-test$ecs-test|
        |dir|Indicates the folder where logs are located. **Note:** The folder cannot end in `/`.

 |`/data/logs`|
        |name|Indicates a log name that support wildcards.|`access.log. 2018-08-08` or `access.log*`|

        **Note:** To prevent Logtail from loading invalid JSON files, we recommend that you save local event configurations to a temporary file, and after editing the temporary file, copy the content to local\_event.json.

    -   **Configuration example** 

        For the Windows operating system, directly use a tool \(for example, the text tool\) to modify the filelocal\_event.json. For the Linux operating system, add local events as follows:

        ```
        $ cat /usr/local/ilogtail/local_event.json
        [
          {
            "config": "##1.0##log-config-test$ecs-test",
            "dir": "/data/log",
            "name": "access.log*"
          },
          {
            "config": "##1.0##log-config-test$tmp-test",
            "dir": "/tmp",
            "name": "access.log.2017-08-09"
          }
        ]
        							
        ```


-   **How can I check whether Logtail has loaded the configuration?** 

    After you save local file `local_event.json`, Logtail loads this local configuration file to the memory within one minute, and clears the content in `local_event.json`.

    You can check whether Logtail has read local events by following these methods:

    -   Check whether the content in `local_event.json` has been cleared. If cleared, Logtail has read the local configurations.
    -   Check whether the file ilogtail.LOG in the Logtail installation directory \(for more information, see [Logtail installation directory](#)\) includes `process local event` keywords. If the content in `local_event.json` has been cleared, but these keywords cannot be found, the local configuration file may be invalid and has been filtered out.
    -   Search the [Query diagnosed errors](reseller.en-US/FAQ/Log collection/Diagnose collection errors.md) result for the `LOAD_LOCAL_EVENT_ALARM` alarm.
-   **Logtail has loaded the configuration, but still cannot collect any data. How can I deal with this issue?** 

    This issue may be caused by the following reasons:

    -   The configuration is invalid.
    -   The `config` item is available in the local configuration.
    -   The target log is not located in the specified path in the collection configuration.
    -   The target log has been collected.

