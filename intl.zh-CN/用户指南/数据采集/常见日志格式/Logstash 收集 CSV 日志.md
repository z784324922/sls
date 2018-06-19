# Logstash 收集 CSV 日志 {#concept_s54_3w5_vdb .concept}

使用Logstash采集CSV日志前，需要修改配置文件以解析CSV日志字段。采集CSV日志可以使用采集日志的系统时间作为上传日志时间，也可以将日志内容中的时间作为上传日志时间。针对日志时间的不同定义方式，可以通过两种方式配置Logstash收集CSV日志。

## 使用系统时间作为日志时间上传 {#section_z5w_cfr_z1b .section}

-   **日志样例**

    ```
    10.116.14.201,-,2/25/2016,11:53:17,W3SVC7,2132,200,0,GET,project/shenzhen-test/logstore/logstash/detail,C:\test\csv\test_csv.log
    
    ```

-   **采集配置**

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

    **说明：** 

    -   配置文件格式必须以 UTF-8 无 BOM 格式编码，可以下载 notepad++ 修改文件编码格式。
    -   path 填写文件路径时请使用 UNIX 模式的分隔符，如：C:/test/multiline/\*.log，否则无法支持模糊匹配。
    -   type 字段需要统一修改并在该文件内保持一致，如果单台机器存在多个Logstash配置文件，需要保证各配置 type 字段唯一，否则会导致数据处理的错乱。
    相关插件：[file](https://www.elastic.co/guide/en/logstash/current/plugins-inputs-file.html)、[csv](https://www.elastic.co/guide/en/logstash/current/plugins-filters-csv.html)。

-   **重启Logstash生效**

    创建配置文件到 conf 目录，参考[配置 Logstash 为 Windows Service](intl.zh-CN/用户指南/数据采集/使用 Logstash 采集日志/配置 Logstash 为 Windows Service.md)重启 Logstash 生效。


## 使用日志字段内容作为日志时间上传 {#section_bwc_qfr_z1b .section}

-   **日志样例**

    ```
    10.116.14.201,-,Feb 25 2016 14:03:44,W3SVC7,1332,200,0,GET,project/shenzhen-test/logstore/logstash/detail,C:\test\csv\test_csv_withtime.log
    ```

-   **采集配置**

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

    **说明：** 

    -   配置文件格式必须以 UTF-8 无 BOM 格式编码，可以下载 notepad++ 修改文件编码格式。
    -   path 填写文件路径时请使用 UNIX 模式的分隔符，如：C:/test/multiline/\*.log，否则无法支持模糊匹配。
    -   type 字段需要统一修改并在该文件内保持一致，如果单台机器存在多个Logstash配置文件，需要保证各配置 type 字段唯一，否则会导致数据处理的错乱。
    相关插件：[file](https://www.elastic.co/guide/en/logstash/current/plugins-inputs-file.html)、[csv](https://www.elastic.co/guide/en/logstash/current/plugins-filters-csv.html)。

-   **重启Logstash生效**

    创建配置文件到 conf 目录，参考 [配置 Logstash 为 Windows Service](intl.zh-CN/用户指南/数据采集/使用 Logstash 采集日志/配置 Logstash 为 Windows Service.md) 重启 Logstash 生效。


