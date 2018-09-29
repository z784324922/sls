# Use Logstash to collect CSV logs {#concept_s54_3w5_vdb .concept}

You need to modify the configuration file to parse the CSV log fields before you use logsturg to capture the CSV log. The acquisition of the CSV log can use the system time of the acquisition log as the upload log time, you can also use the time in the contents of the log as the upload log time. For different definitions of log time, there are two ways to configure logstroudsburg to collect CSV logs.

## Use the system time as the uploaded log time {#section_z5w_cfr_z1b .section}

-   **Log sample **

    ```
    10.116.14.201,-,2/25/2016,11:53:17,W3SVC7,2132,200,0,GET,project/shenzhen-test/logstore/logstash/detail,C:\test\csv\test_csv.log
    
    ```

-   **Collection configuration**

    ```
    input {
      file {
        type => "csv_log_1"
        path => ["C:/test/csv/*.log"]
        start_position => "beginning"
      }
    }
    filter {
      if [type] == "csv_log_1" {
      csv {
        separator => ","
        columns => ["ip", "a", "date", "time", "b", "latency", "status", "size", "method", "url", "file"]
      } 
      }
    }
    output {
      if [type] == "csv_log_1" {
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
    -   path 填写文件路径时请使用 UNIX format, for example, C:/test/multiline/\*.log . Otherwise, fuzzy match is not supported.
    -   type  field must be modified unitedly and kept consistent in the file. If a machine has multiple Logstash configuration files,  type  field in each configuration file must be unique. Otherwise, data cannot be processed properly.
    Related plug-ins: [file](https://www.elastic.co/guide/en/logstash/current/plugins-inputs-file.html) and [csv](https://www.elastic.co/guide/en/logstash/current/plugins-filters-csv.html).

-   **Restart Logstash to apply configurations**

    Create a configuration file in the conf  directory and restart Logstash to apply the file. For more information, see Set [Set Logstash as a Windows service](reseller.en-US/User Guide/Data Collection/Logstash/Set Logstash as a Windows service.md) as a Windows service.


## Upload the log field content as the log time {#section_bwc_qfr_z1b .section}

-   **Log sample **

    ```
    10.116.14.201,-,Feb 25 2016 14:03:44,W3SVC7,1332,200,0,GET,project/shenzhen-test/logstore/logstash/detail,C:\test\csv\test_csv_withtime.log
    ```

-   **Collection configuration**

    ```
    input {
      file {
        type => "csv_log_2"
        path => ["C:/test/csv_withtime/*.log"]
        start_position => "beginning"
      }
    }
    filter {
      if [type] == "csv_log_2" {
      csv {
        separator => ","
        columns => ["ip", "a", "datetime", "b", "latency", "status", "size", "method", "url", "file"]
      } 
      date {
        match => [ "datetime" , "MMM dd YYYY HH:mm:ss" ]
      }
      }
    }
    output {
      if [type] == "csv_log_2" {
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
    -   path 填写文件路径时请使用 UNIX format, for example, C:/test/multiline/\*.log . Otherwise, fuzzy match is not supported.
    -   type  field must be modified unitedly and kept consistent in the file. If a machine has multiple Logstash configuration files,  type  field in each configuration file must be unique. Otherwise, data cannot be processed properly.
    Related plug-ins: [file](https://www.elastic.co/guide/en/logstash/current/plugins-inputs-file.html) and [csv](https://www.elastic.co/guide/en/logstash/current/plugins-filters-csv.html).

-   **Restart Logstash to apply configurations**

    Create a configuration file in the conf directory and restart Logstash to apply the file. For more information, see Set [Set Logstash as a Windows service](reseller.en-US/User Guide/Data Collection/Logstash/Set Logstash as a Windows service.md) as a Windows service.


