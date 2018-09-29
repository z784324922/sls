# Text-Import history logs {#task_g1x_q2s_g2b .task}

Logtail only collects incremental logs by default. If you want to import history logs, use the history log importing feature of Logtail.

-   Logtail versions must be `0.16.6` and later.
-   Target history logs must belong to the collection configuration, and they have not been collected by Logtail.
-   Last modification time of history logs must be earlier than Logtail configuration time.
-   The maximum interval between generating and importing local configurations is one minute.
-   Due to the special action of loading local configurations, Logtail notifies you of this action by  sending `LOAD_LOCAL_EVENT_ALARM` to your server.

Logtail collects logs based on the events that are detected by listening on or performing round robin for log modifications. Logtail can also load local configurations, and trigger log collections. Logtail collects history logs by loading local configurations.

1.   Create collection configurations Configure the collection and apply the configuration to the machine group. Make sure that the target logs belong to the collection configuration. For more information about the collection configuration, see [Collect text logs](reseller.en-US/User Guide/Logtail collection/Data Source/Collect text logs.md).
2.   Gets the configuration unique identity. Obtain a unique identifier for the configuration from local `/usr/local/ilogtail/user_log_config.json`  as shown in the following example:

    ```
    grep "##" /usr/local/ilogtail/user_log_config.json | awk '{print $1}'
                        "##1.0##log-config-test$multi"
                        "##1.0##log-config-test$ecs-test"
                        "##1.0##log-config-test$metric_system_test"
                        "##1.0##log-config-test$redis-status"
                    
    ```

3.   Add local events. Save local events to JSON file  `/usr/local/ilogtail/local_event.json` by using the following format:

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
        |dir|Indicates the folder where logs are located.**Note:** The folder cannot end in `/`.

|`/data/logs`|
        |name |Indicates a log name.|`access.log. 2018-08-08`|

        **Note:** To prevent Logtail from loading invalid JSON files, we recommend that you save local event configurations to a temporary file, and after editing the temporary file, copy the content to `/usr/local/ilogtail/local_event.json` .

    -   **Configuration example** 

        ```
        $ cat /usr/local/ilogtail/local_event.json
        [
          {
            "config" : "##1.0##log-config-test$ecs-test",
            "dir" : "/data/log/",
            "name" : "access.log. 2017-08-08"
          },
          {
            "config" : "##1.0##log-config-test$ecs-test",
            "dir" : "/tmp",
            "name" : "access.log. 2017-08-09"
          }
        ]
                                    
        ```


-   **How can I check whether Logtail has loaded the configuration?**

    After you save local file `local_event.json`, Logtail loads this local configuration file to the memory within one minute, and clears the content in `local_event.json`.

    You can check whether Logtail has read local events by following these methods:

    -   Check whether the content in `local_event.json` has been cleared. If cleared, Logtail has read the local configurations.
    -   Check whether the file `/usr/local/ilogtail/ilogtail.LOG` includes `process local   event` keywords. If the content in `local_event.json` has been cleared, but these keywords cannot be found, the local configuration file may be invalid and has been filtered out.
    -   Search the [Query diagnosed errors](reseller.en-US/User Guide/Logtail collection/Troubleshoot/Procedure.md) result for the `LOAD_LOCAL_EVENT_ALARM` alarm.
-   **Logtail has loaded the configuration, but still cannot collect any data. How can I deal with this issue?**

    This issue may be caused by the following reasons:

    -   The configuration is invalid.
    -   The `config` item is available in the local configuration.
    -   The target log is not located in the specified path in the collection configuration.
    -   The target log has been collected.
-   **How can I collect data that has already been collected?**

    To collect data that has already been collected, follow these steps:

    1.  Run the `/etc/init.d/ilogtaild stop`command to stop Logtail.
    2.  Find the path of the log in the `/tmp/logtail_check_point` file.
    3.  Delete the checkpoint \(JSON object\) of this log and save the modification.
    4.  Add the local event by following step 3 in the Procedure section.
    5.  Run the `/etc/init.d/ilogtaild start` command to start Logtail.

