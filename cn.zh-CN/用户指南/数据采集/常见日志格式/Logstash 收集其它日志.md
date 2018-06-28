# Logstash 收集其它日志 {#concept_rm4_1x5_vdb .concept}

使用Logstash采集日志前，可以修改配置文件以解析日志字段。

## 使用系统时间作为日志时间上传 {#section_z5w_cfr_z1b .section}

-   **日志样例**

    ```
    2016-02-25 15:37:01 [main] INFO com.aliyun.sls.test_log4j - single line log
    2016-02-25 15:37:11 [main] ERROR com.aliyun.sls.test_log4j - catch exception !
     java.lang.ArithmeticException: / by zero
        at com.aliyun.sls.test_log4j.divide(test_log4j.java:23) ~[bin/:?]
        at com.aliyun.sls.test_log4j.main(test_log4j.java:13) [bin/:?]
    2016-02-25 15:38:02 [main] INFO com.aliyun.sls.test_log4j - normal log
    ```

-   **采集配置**

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

    **说明：** 

    -   配置文件格式必须以 UTF-8 无 BOM 格式编码，可以下载 notepad++ 修改文件编码格式。
    -   path 填写文件路径时请使用 UNIX 模式的分隔符，如：C:/test/multiline/\*.log，否则无法支持模糊匹配。
    -   type 字段需要统一修改并在该文件内保持一致，如果单台机器存在多个Logstash配置文件，需要保证各配置 type 字段唯一，否则会导致数据处理的错乱。
    相关插件：[file](https://www.elastic.co/guide/en/logstash/current/plugins-inputs-file.html)、[multiline](https://www.elastic.co/guide/en/logstash/current/plugins-filters-multiline.html)（若日志文件是单行日志，可以去掉 codec =\> multiline 配置）。

-   **重启Logstash生效**

    创建配置文件到 conf 目录，参考 [配置 Logstash 为 Windows Service](cn.zh-CN/用户指南/数据采集/使用 Logstash 采集日志/配置 Logstash 为 Windows Service.md) 重启 Logstash 生效。


