# Use Logstash to collect other logs {#concept_rm4_1x5_vdb .concept}

You can modify the configuration file to parse log fields before you use logsturg to capture logs.

## Upload using system time as log time {#section_z5w_cfr_z1b .section}

-   **Log sample**

    ```
    2016-02-25 15:37:01 [main] INFO com.aliyun.sls.test_log4j - single line log
    2016-02-25 15:37:11 [main] ERROR com.aliyun.sls.test_log4j - catch exception !
     java.lang.ArithmeticException: / by zero
        at com.aliyun.sls.test_log4j.divide(test_log4j.java:23) ~[bin/:?]
        at com.aliyun.sls.test_log4j.main(test_log4j.java:13) [bin/:?]
    2016-02-25 15:38:02 [main] INFO com.aliyun.sls.test_log4j - normal log
    ```

-   **Collection configuration**

    ```
    input {
      file {
        type => "common_log_1"
        path => ["C:/test/multiline/*.log"]
        start_position => "beginning"
        codec => multiline {
          pattern => "^\d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2}"
          negate => true
          auto_flush_interval => 3
          what => previous
        }
      }
    }
    output {
      if [type] == "common_log_1" {
      logservice {
            codec => "json"
            endpoint => "***"
            project => "***"
            logstore => "***"
            topic => ""
            source => ""
            access_key_id => "***"
            access_key_secret => "***"
            max_send_retry => 10
        }
        }
    }
    ```

    **Note:** 

    -   The configuration file must be encoded in UTF-8 format without BOM. You can use Notepad++ to modify the file encoding format. 
    -    path indicates the file path, which must use delimiters in the UNIX  format, for example, C:/test/multiline/\*.log . Otherwise, fuzzy match is not supported.
    -   type field must be modified unitedly and kept consistent in the file. If a machine has multiple Logstash configuration files, the type  field in each configuration file must be unique. Otherwise, data cannot be processed properly.
    Related plug-ins: [file](https://www.elastic.co/guide/en/logstash/current/plugins-inputs-file.html) and [multiline](https://www.elastic.co/guide/en/logstash/current/plugins-filters-multiline.html)\(for a single-line log file, remove the codec =\>  multiline configuration\).

-   **Restart Logstash to apply configurations**

    Create a configuration file in the confdirectory and restart Logstash to apply the file. For more information, see [Set Logstash as a Windows service](intl.en-US/User Guide/Data Collection/Logstash/Set Logstash as a Windows service.md).


