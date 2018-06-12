# Log4j日志 {#reference_xk5_3t5_vdb .reference}

## 接入方式 {#section_u42_cvb_ry .section}

日志服务支持通过以下方式采集log4j日志：

-   LogHub Log4j Appender
-   Logtail

## 通过Loghub Log4j Appender采集Log4j日志 {#section_wjz_fvb_ry .section}

详细内容及采集步骤请参考[Log4j Appender](intl.zh-CN/用户指南/数据采集/Log4j Appender.md)。

## 通过Logtail采集Log4j日志 {#section_scp_h23_dbb .section}

Log4j日志分第一代和第二代，本文档以第一代的默认配置为例，讲述如何配置正则式，如果采用Log4j 2，需要修改默认配置，把日期完整打印出来。

```
<Configuration status="WARN">
  <Appenders>
    <Console name="Console" target="SYSTEM_OUT">
      <PatternLayout pattern="%d{yyyy-MM-dd HH:mm:ss:SSS zzz} [%t] %-5level %logger{36} - %msg%n"/>
    </Console>
  </Appenders>
  <Loggers>
    <Logger name="com.foo.Bar" level="trace">
      <AppenderRef ref="Console"/>
    </Logger>
    <Root level="error">
      <AppenderRef ref="Console"/>
    </Root>
  </Loggers>
</Configuration>
```

配置Logtail收集Log4j日志的详细操作步骤请参考[Apache 日志](intl.zh-CN/用户指南/数据采集/常见日志格式/Apache 日志.md)，根据您的网络部署和实际情况选择对应配置。

在生成正则式的部分，由于自动生成的正则式只参考了日志样例，无法覆盖所有的日志情况，所以需要用户在自动生成之后做一些微调。

Log4j 默认日志格式打印到文件中的日志样例如下：

```
2013-12-25 19:57:06,954 [10.207.37.161] WARN impl.PermanentTairDaoImpl - Fail to Read Permanent Tair,key:e:470217319319741_1,result:com.example.tair.Result@172e3ebc[rc=code=-1, msg=connection error or timeout,value=,flag=0]
```

多行日志起始匹配（使用IP信息表示一行开头）：

```
\d+-\d+-\d+\s.*
```

提取日志信息的正则表达式：

```
(\d+-\d+-\d+\s\d+:\d+:\d+,\d+)\s\[([^\]]*)\]\s(\S+)\s+(\S+)\s-\s(.*)
```

时间转换格式：

```
%Y-%m-%d %H:%M:%S
```

样例日志提取结果：

|Key|Value|
|---|:----|
|time|2013-12-25 19:57:06,954|
|ip|10.207.37.161|
|level|WARN|
|class|impl.PermanentTairDaoImpl|
|message|Fail to Read Permanent Tair,key:e:470217319319741\_1,result:com.example.tair.Result@172e3ebc\[rc=code=-1, msg=connection error or timeout,value=,flag=0\]|

