# Log4j logs {#reference_xk5_3t5_vdb .reference}

## Access Mode {#section_u42_cvb_ry .section}

Log Service supports collecting Log4j logs by using:

-   LogHub Log4j Appender
-   Logtail

## Collect Log4j logs by using LogHub Log4j Appender {#section_wjz_fvb_ry .section}

For more information, see [Log4j Appender](intl.en-US/User Guide/Data Collection/Log4j Appender.md).

## Collect Log4j logs by using Logtail {#section_scp_h23_dbb .section}

The log4j log consists of the first and second generations, and this document takes the default configuration of the first generation as an example, describes how to configure regular, if log4j is used 2. You need to modify the default configuration to print the date completely.

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

For how to configure Logtail to collect Log4j logs, see [Apache logs](intl.en-US/User Guide/Data Collection/Common log formats/Apache logs.md). Select the corresponding configuration based on your network deployment and actual situation. 

The automatically generated regular expression is only based on the log sample and does not cover all the situations of logs. Therefore, you must adjust the regular expression slightly after it is automatically generated.

Log4j e log sample of Log4j default log format printed to a file is as follows:

```
2013-12-25 19:57:06,954 [10.207.37.161] WARN impl.PermanentTairDaoImpl - Fail to Read Permanent Tair,key:e:470217319319741_1,result:com.example.tair.Result@172e3ebc[rc=code=-1, msg=connection error or timeout,value=,flag=0]
```

Matching of the beginning of a line in multiline logs \(use IP to indicate the beginning of a line\):

```
\d+-\d+-\d+\s.
```

The regular expression used to extract log information:

```
(\d+-\d+-\d+\s\d+:\d+:\d+,\d+)\s\[([^\]]*)\]\s(\S+)\s+(\S+)\s-\s(.
```

Time conversion format:

```
%Y-%m-%d %H:%M:%S
```

Extraction results of the log sample:

|Key|value|
|---|:----|
|time|2013-12-25 19:57:06,954|
|ip|10.207.37.161|
|level|WARN|
|class|impl.PermanentTairDaoImpl|
|message|Fail to Read Permanent Tair,key:e:470217319319741\_1,result:com.example.tair.Result@172e3ebc\[rc=code=-1, msg=connection error or timeout,value=,flag=0\]|

