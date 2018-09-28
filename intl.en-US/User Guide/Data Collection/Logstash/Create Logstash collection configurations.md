# Create Logstash collection configurations {#task_vvz_1q5_vdb .task}

**Related plug-ins**

-   **logstash-input-file**

    This plug-in is used to collect log files in tail mode. For more information, see[logstash-input-file](https://www.elastic.co/guide/en/logstash/current/plugins-inputs-file.html).

    **Note:** path indicates the file path, which must use UNIX separators, for example,  C:/test/multiline/\*.log. Otherwise, fuzzy match is not supported.

-   **logstash-output-logservice**

    This plug-in is used to output the logs collected by the logstash-input-file plug-in to Log Service.

    |Parameters|Description|
    |:---------|:----------|
    |endpoint|Log Service endpoint. Example:   http://regionid.example.com. For more information, see Log Service endpoint.|
    |project|The project name of Log Service.|
    |logstore|The Logstore name.|
    |topic|The log topic name. The default value is null.|
    |source| The log source. If this parameter is set to null, the IP address of the current machine is used as the log source. Otherwise, the log source is subject to the specified parameter value. |
    |access\_key\_id|The AccessKey ID of the Alibaba Cloud account.|
    |access\_key\_secret|The AccessKey Secret of the Alibaba Cloud account.|
    |max\_send\_retry|The maximum number of retries performed when data packets cannot be sent to Log Service because of an exception. Data packets with retry failures are discarded. The retry interval is 200  ms.|


1.   Create collection configurations 

    Create a configuration file in the C:\\logstash-2.2.2-win\\conf\\ directory  and then restart Logstash to apply the file.

    You can create a configuration file for each log type. The file name format is \*.conf .  For easier management, we recommend that you create all the configuration files in the C:\\logstash-2.2.2-win\\conf\\  directory.

    **Note:** The configuration file must be encoded in UTF-8 format without BOM. You can use Notepad++ to modify the file encoding format.

    -   IIS logs

        For more information, see  [Use Logstash to collect IIS logs](intl.en-US/User Guide/Data Collection/Common log formats/Use Logstash to collect IIS logs.md).

    -   CSV logs

        Use the system time of log collection as the log uploaded time. For more information, see CSV log configuration.

    -   Logs with built-in time

        Take CSV log format as an example. Use the time in the log content as the log uploaded time. For more information, see [Use Logstash to collect CSV logs](intl.en-US/User Guide/Data Collection/Common log formats/Use Logstash to collect CSV logs.md).

    -   General logs

        By default, the system time of log collection is used as the log uploaded time. Log fields are not parsed. Single-line logs and multiline logs are supported.  For more information, see  [Use Logstash to collect other logs](intl.en-US/User Guide/Data Collection/Common log formats/Use Logstash to collect other logs.md).

2.   Verify configuration syntax 

    1.  Run `PowerShell`  or  `cmd.exe` to go to the Logstash  installation directory:

        ```
        PS C:\logstash-2.2.2-win\bin> .\logstash.bat agent --configtest --config C:\logstash-2.2.2-win\conf\iis_log.conf
        ```

    2.  Modify the collection configuration file. Temporarily add a line of rubydebug configuration in the output phase to output the collection results to the console.  Set the type field  as per your needs.

        ```
        output {
        If [type] = "***"{
          stdout { codec => rubydebug }
          logservice {
          
          }
         
        
        ```

    3.  Run `PowerShell` or `cmd.exe` to go to the Logstash  installation directory and start the process:

        ```
        PS C:\logstash-2.2.2-win\bin> .\logstash.bat agent -f C:\logstash-2.2.2-win\conf
        ```

    After the verification, end thelogstash.bat process and delete the temporary configuration item rubydebug.


When logstash.bat is started in PowerShell, the Logstash process is  working in the frontend. Logstash is generally used for testing configurations and debugging collections. Therefore, we recommend that you set Logstash as a Windows service after the  debugging is passed so as to enable Logstash to work in the backend and start automatically when power-on.  For how to set Logstash as a Windows service, see  [Set Logstash as a Windows service](intl.en-US/User Guide/Data Collection/Logstash/Set Logstash as a Windows service.md).

