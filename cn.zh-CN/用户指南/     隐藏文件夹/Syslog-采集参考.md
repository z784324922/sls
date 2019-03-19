# Syslog-采集参考 {#concept_l3b_1zz_gfb .concept}

Logtail目前支持的接入端为syslog和文本文件，如下图所示：

![](images/2924_zh-CN.png "Logtail支持的接入端")

Logtail通过TCP协议支持syslog。配置Logtail采集syslog日志详细步骤请参见[Syslog](intl.zh-CN/用户指南/     隐藏文件夹/Syslog.md)通过Logtail采集syslog日志。

## syslog优势 {#section_o3b_1zz_gfb .section}

syslog概念请参考 [syslog](http://linux.vbird.org/linux_basic/0570syslog.php?spm=5176.doc43759.2.1.jP9SMF)。

和利用文本文件相比，使用syslog时日志数据直接收集到LogHub，syslog不落盘、保密性好。免去了文件落盘和解析的代价，单机可达 80MB/S 吞吐率。

## 基本原理 {#section_p3b_1zz_gfb .section}

Logtail 支持在本地配置TCP端口，接收syslog Agent转发的日志。Logtail开启TCP端口，接收rsyslog或者其他syslog Agent通过TCP协议转发过来的syslog数据，Logtail解析接收到的数据并转发到LogHub中。配置Logtail采集syslog日志过程请参见[Syslog](intl.zh-CN/用户指南/     隐藏文件夹/Syslog.md)。Logtail、syslog、LogHub三者之间的关系如下图所示。

![](images/2925_zh-CN.png "基本原理")

## syslog日志格式 {#section_s3b_1zz_gfb .section}

Logtail 通过 TCP 端口接收到的数据是流式的，如果要从流式的数据中解析出一条条的日志，日志格式必须满足以下条件：

-   每条日志之间使用换行符（`\n`）分隔，一条日志内部不可以出现换行符。
-   日志内部除了消息正文可以包含空格，其他字段不可以包含空格。

syslog日志格式如下：

```
$version $tag $unixtimestamp $ip [$user-defined-field-1 $user-defined-field-2 $user-defined-field-n] $msg\n"
```

各个字段含义为：

|日志字段|含义|
|:---|:-|
|version|该日志格式的版本号，Logtail使用该版本号解析user-defined-field 字段。|
|tag|数据标签，用于寻找Project或Logstore，不可以包含空格和换行符。|
|unixtimestamp：|该条日志的时间戳。|
|ip|该条日志的对应的机器IP，如果日志中的该字段是 127.0.0.1，最终发往服务端的日志数据中该字段会被替换成TCP socket的对端地址。|
|user-defined-field|用户自定义字段，中括号表示是可选字段，可以有 0 个或多个，不可以包含空格和换行符。|
|msg|日志消息正文，不可以包含换行符，末尾的 `\n` 表示换行符。|

以下示例日志即为满足格式要求的日志：

```
2.1 streamlog_tag 1455776661 10.101.166.127 ERROR com.alibaba.streamlog.App.main(App.java:17) connection refused, retry
```

另外，不仅 syslog 日志可以接入Logtail，任何日志工具只要能满足以下条件，都可以接入：

-   可以将日志格式化，格式化之后的日志格式满足格式要求。
-   可以通过 TCP 协议将日志 append 到远端。

## Logtail解析syslog日志规则 {#section_z3b_1zz_gfb .section}

Logtail 需要增加配置以解析syslog日志。例如：

```
"streamlog_formats":
[
    {"version": "2.1", "fields": ["level", "method"]},
    {"version": "2.2", "fields": []},
    {"version": "2.3", "fields": ["pri-text", "app-name", "syslogtag"]}
]
```

Logtail通过读取到的`version`字段到`streamlog_formats`中找到对应的`user-defined`字段的格式，应用该配置，上面的日志样例`version`字段为 2.1，包含两个自定义字段`level`和`method`，因此日志样例将被解析为如下格式：

```
{
    "source": "10.101.166.127",
    "time": 1455776661,
    "level": "ERROR",
    "method": "com.alibaba.streamlog.App.main(App.java:17)",
    "msg": "connection refused, retry"
}
```

`version`用于解析`user-defined`字段，tag用于寻找数据将要被发送到的Project或Logstore，这两个字段不会作为日志内容发送到阿里云日志服务。另外，Logtail预定义了一些日志格式，这些内置的格式都使用 0.1、1.1 这样以“0.”、“1.”开头的`version`值，所以用户自定义`version`不可以以“0.”、“1.”开头。

## 常见日志工具接入 Logtail syslog log {#section_bjb_1zz_gfb .section}

-   **log4j**
    -   引入 log4j 库。

        ```
        <dependency>
              <groupId>org.apache.logging.log4j</groupId>
              <artifactId>log4j-api</artifactId>
              <version>2.5</version>
          </dependency>
          <dependency>
              <groupId>org.apache.logging.log4j</groupId>
              <artifactId>log4j-core</artifactId>
              <version>2.5</version>
          </dependency>
        ```

    -   程序中引入 log4j 配置文件 `log4j_aliyun.xml`。

        ```
         <?xml version="1.0" encoding="UTF-8"?>
          <configuration status="OFF">
              <appenders>
                  <Socket name="StreamLog" protocol="TCP" host="10.101.166.173" port="11111">
                      <PatternLayout pattern="%X{version} %X{tag} %d{UNIX} %X{ip} %-5p %l %enc{%m}%n" />
                  </Socket>
              </appenders>
              <loggers>
                  <root level="trace">
                      <appender-ref ref="StreamLog" />
                  </root>
              </loggers>
          </configuration>
        ```

        其中 10.101.166.173:11111 是 Logtail 所在机器的地址。

    -   程序中设置 ThreadContext。

        ```
        package com.alibaba.streamlog;
          import org.apache.logging.log4j.LogManager;
          import org.apache.logging.log4j.Logger;
          import org.apache.logging.log4j.ThreadContext;
          public class App 
          {
              private static Logger logger = LogManager.getLogger(App.class);
              public static void main( String[] args ) throws InterruptedException
              {
                  ThreadContext.put("version", "2.1");
                  ThreadContext.put("tag", "streamlog_tag");
                  ThreadContext.put("ip", "127.0.0.1");
                  while(true)
                  {
                      logger.error("hello world");
                      Thread.sleep(1000);
                  }
                  //ThreadContext.clearAll();
              }
          }
        ```

-   **tengine**

    tengine 可以通过 syslog 接入 ilogtail。

    tengine 使用 `ngx_http_log_module`模块将日志打入本地 syslog agent，在本地 syslog agent 中 forward 到 rsyslog。

    tengine 配置 syslog 请参考：[tengine配置syslog](http://tengine.taobao.org/document_cn/http_log_cn.html)

    **示例**：

    以 user 类型和 info 级别将 access log 发送给本机 Unix dgram\(/dev/log\)，并设置应用标记为 nginx。

    ```
    access_log syslog:user:info:/var/log/nginx.sock:nginx
    ```

    rsyslog 配置：

    ```
    module(load="imuxsock") # needs to be done just once
    input(type="imuxsock" Socket="/var/log/nginx.sock" CreatePath="on")
    $template ALI_LOG_FMT,"2.3 streamlog_tag %timegenerated:::date-unixtimestamp% %fromhost-ip% %pri-text% %app-name% %syslogtag% %msg:::drop-last-lf%\n"
    if $syslogtag == 'nginx' then @@10.101.166.173:11111;ALI_LOG_FMT
    ```

-   **nginx**

    以收集 nginx accesslog 为例。

    access log 配置：

    ```
    access_log syslog:server=unix:/var/log/nginx.sock,nohostname,tag=nginx;
    ```

    rsyslog 配置：

    ```
    module(load="imuxsock") # needs to be done just once
    input(type="imuxsock" Socket="/var/log/nginx.sock" CreatePath="on")
    $template ALI_LOG_FMT,"2.3 streamlog_tag %timegenerated:::date-unixtimestamp% %fromhost-ip% %pri-text% %app-name% %syslogtag% %msg:::drop-last-lf%\n"
    if $syslogtag == 'nginx' then @@10.101.166.173:11111;ALI_LOG_FMT
    ```

    参考：[http://nginx.org/en/docs/syslog.html](http://nginx.org/en/docs/syslog.html)

-   **Python Syslog**

    示例：

    ```
    import logging
    import logging.handlers
    logger = logging.getLogger('myLogger')
    logger.setLevel(logging.INFO)
    #add handler to the logger using unix domain socket '/dev/log'
    handler = logging.handlers.SysLogHandler('/dev/log')
    #add formatter to the handler
    formatter = logging.Formatter('Python: { "loggerName":"%(name)s", "asciTime":"%(asctime)s", "pathName":"%(pathname)s", "logRecordCreationTime":"%(created)f", "functionName":"%(funcName)s", "levelNo":"%(levelno)s", "lineNo":"%(lineno)d", "time":"%(msecs)d", "levelName":"%(levelname)s", "message":"%(message)s"}')
    handler.formatter = formatter
    logger.addHandler(handler)
    logger.info("Test Message")
    ```


