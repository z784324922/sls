# Use Logstash to collect IIS logs {#concept_twh_bw5_vdb .concept}

You need to modify the configuration file to parse the IIS log fields before you use logsturg to capture the IIS log.

## Collection configuration {#section_slg_fbr_z1b .section}

View IIS log configurations, select the W3C format  \(default field setting\), and save the format to put it into effect.

```
2016-02-25 01:27:04 112.74.74.124 GET /goods/list/0/1.html - 80 - 66.249.65.102 Mozilla/5.0+(compatible;+Googlebot/2.1;++http://www.google.com/bot.html) 404 0 2 703
```

## Collection configuration {#section_k54_hbr_z1b .section}

```
input {
  file {
    type => "iis_log_1"
    path => ["C:/inetpub/logs/LogFiles/W3SVC1/*.log"]
    start_position => "beginning"
  }

filter {
  if [type] == "iis_log_1" {
  #ignore log comments
  if [message] =~ "^#" {
    drop {}
  
  grok {
    # check that fields match your IIS log settings
    match => ["message", "%{TIMESTAMP_ISO8601:log_timestamp} %{IPORHOST:site} %{WORD:method} %{URIPATH:page} %{NOTSPACE:querystring} %{NUMBER:port} %{NOTSPACE:username} %{IPORHOST:clienthost} %{NOTSPACE:useragent} %{NUMBER:response} %{NUMBER:subresponse} %{NUMBER:scstatus} %{NUMBER:time_taken}"]
  
    date {
    match => [ "log_timestamp", "YYYY-MM-dd HH:mm:ss" ]
      timezone => "Etc/UTC"
      
  useragent {
    source=> "useragent"
    prefix=> "browser"
  
  mutate {
    remove_field => [ "log_timestamp"]
  
  

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
    
    

```

**Note:** 

-   •The configuration file must be encoded in UTF-8 format without BOM. You can use Notepad++ to modify the file encoding format. 
-    path  indicates the file path, which must use delimiters in the UNIX format, for example, C:/test/multiline/\*.log . Otherwise, fuzzy match is not supported.
-   The type  field must be modified unitedly and kept consistent in the file. If a machine has multiple Logstash configuration files, the type field in each configuration  file must be unique. Otherwise, data cannot be processed properly.

Related plug-ins: [file](https://www.elastic.co/guide/en/logstash/current/plugins-inputs-file.html) and [grok](https://www.elastic.co/guide/en/logstash/current/plugins-filters-grok.html).

## Restart Logstash to apply configurations {#section_rmx_pbr_z1b .section}

Create a configuration file in the conf  directory and restart Logstash to apply the file. See [Set Logstash as a Windows service](intl.en-US/User Guide/Data Collection/Use Logstash to collect logs/Set Logstash as a Windows service.md) for more information.

