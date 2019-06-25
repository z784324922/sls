# Node.js日志 {#reference_z2j_xt5_vdb .reference}

Node.js的日志默认打印到控制台，为数据收集和问题调查带来不便。通过log4js可以实现把日志打印到文件、自定义日志格式等功能，便于数据收集和整理。

``` {#codeblock_1b0_zuk_3e6}
var log4js = require('log4js');
log4js.configure({
  appenders: [
    {   
      type: 'file', //文件输出
      filename: 'logs/access.log', 
      maxLogSize: 1024,
      backups:3,
      category: 'normal' 
    }   
  ]
});
var logger = log4js.getLogger('normal');
logger.setLevel('INFO');
logger.info("this is a info msg");
logger.error("this is a err msg");
```

## 日志格式 {#section_zrl_wbp_tcb .section}

通过log4js实现日志数据存储为文本文件格式后，日志在文件中显示为以下格式：

``` {#codeblock_fhr_fq0_5vq}
[2016-02-24 17:42:38.946] [INFO] normal - this is a info msg
[2016-02-24 17:42:38.951] [ERROR] normal - this is a err msg
```

log4js分为6个输出级别，从低到高分别为trace、debug、info、warn、error、fatal。

## 通过Logtail收集Node.js日志 {#section_orl_wbp_tcb .section}

配置Logtail收集Python日志的详细操作步骤请参考[Python日志](intl.zh-CN/用户指南/其他采集方式/常见日志格式/Python日志.md)，根据您的网络部署和实际情况选择对应配置。

在生成正则式的部分，由于自动生成的正则式只参考了日志样例，无法覆盖所有的日志情况，所以需要用户在自动生成之后做一些微调。您可以参考以下Node.js日志示例，为您的日志撰写正确、全面的正则表达式。

 **常见的Node.js日志及其正则表达式**：

-   Node.js日志示例1
    -   日志示例：

        ``` {#codeblock_aoc_zh7_qhu}
        [2016-02-24 17:42:38.946] [INFO] normal - this is a info msg
        ```

    -   正则表达式：

        ``` {#codeblock_4x0_5kb_fsn}
        \[([^]]+)]\s\[([^\]]+)]\s(\w+)\s-(.*)
        ```

    -   提取字段：

         `time`、`level`、`loggerName`和`message`。


-   Node.js日志示例2
    -   日志示例：

        ``` {#codeblock_jmh_ahn_5v6}
        [2016-01-31 12:02:25.844] [INFO] access - 42.120.73.203 - - "GET /user/projects/ali_sls_log?ignoreError=true HTTP/1.1" 304 - "http://
        aliyun.com/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/48.0.2564.97 Safari/537.36"
        ```

    -   正则表达式：

        ``` {#codeblock_055_6ko_vcd}
        \[([^]]+)]\s\[(\w+)]\s(\w+)\s-\s(\S+)\s-\s-\s"([^"]+)"\s(\d+)[^"]+("[^"]+)"\s"([^"]+).*
        ```

    -   提取字段：

         `time`、`level`、`loggerName`、`ip、` `request`、`status`、`referer`和`user_agent`。


