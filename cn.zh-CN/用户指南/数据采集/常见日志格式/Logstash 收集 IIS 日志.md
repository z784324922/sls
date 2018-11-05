# Logstash 收集 IIS 日志 {#concept_twh_bw5_vdb .concept}

使用Logstash采集IIS日志前，需要修改配置文件以解析IIS日志字段。

## 日志样例 {#section_slg_fbr_z1b .section}

查看 IIS 日志配置，选择格式为 W3C（默认字段设置）保存生效。

```
2016-02-25 01:27:04 112.74.74.124 GET /goods/list/0/1.html - 80 - 66.249.65.102 Mozilla/5.0+(compatible;+Googlebot/2.1;++http://www.google.com/bot.html) 404 0 2 703
```

## 采集配置 {#section_k54_hbr_z1b .section}

```
input {
  file {
    type => "iis_log_1"
    path => ["C:/inetpub/logs/LogFiles/W3SVC1/*.log"]
    start_position => "beginning"
  }
}
filter {
  if [type] == "iis_log_1" {
  #ignore log comments
  if [message] =~ "^#" {
    drop {}
  }
  grok {
    # check that fields match your IIS log settings
    match => ["message", "%{TIMESTAMP_ISO8601:log_timestamp} %{IPORHOST:site} %{WORD:method} %{URIPATH:page} %{NOTSPACE:querystring} %{NUMBER:port} %{NOTSPACE:username} %{IPORHOST:clienthost} %{NOTSPACE:useragent} %{NUMBER:response} %{NUMBER:subresponse} %{NUMBER:scstatus} %{NUMBER:time_taken}"]
  }
    date {
    match => [ "log_timestamp", "YYYY-MM-dd HH:mm:ss" ]
      timezone => "Etc/UTC"
  }    
  useragent {
    source=> "useragent"
    prefix=> "browser"
  }
  mutate {
    remove_field => [ "log_timestamp"]
  }
  }
}
output {
  if [type] == "iis_log_1" {
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

-   配置文件格式必须以 UTF-8 无 BOM 格式编码，可以通过notepad++修改文件编码格式。
-   path 填写文件路径时请使用UNIX模式的分隔符，如：C:/test/multiline/\*.log，否则无法支持模糊匹配。
-   type 字段需要统一修改并在该文件内保持一致，如果单台机器存在多个 Logstash 配置文件，需要保证各配置 type 字段唯一，否则会导致数据处理的错乱。

相关插件：[file](https://www.elastic.co/guide/en/logstash/current/plugins-inputs-file.html)、[grok](https://www.elastic.co/guide/en/logstash/current/plugins-filters-grok.html)。

## 重启Logstash生效 {#section_rmx_pbr_z1b .section}

创建配置文件到 conf 目录，参考[配置 Logstash 为 Windows Service](intl.zh-CN/用户指南/数据采集/Logstash/配置 Logstash 为 Windows Service.md)重启 Logstash 生效。

