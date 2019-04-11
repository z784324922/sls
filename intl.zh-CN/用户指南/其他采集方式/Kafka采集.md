# Kafka采集 {#concept_n3m_pkc_ghb .concept}

日志服务支持使用Kafka协议对数据进行采集。

## 相关限制 {#section_gg2_34c_ghb .section}

-   支持的Kafka协议版本为：0.8.0到2.1.IV2。
-   为保证数据传输安全性，连接协议必须为SASL\_SSL。
-   若您的Logstore有多个Shard，请使用负载均衡模式写入数据。

## 配置方式 {#section_mxs_54c_ghb .section}

使用Kafka协议写入时，您需要配置以下参数：

|配置名|说明|示例|
|---|--|--|
|连接类型|为保证数据传输安全性，连接协议必须为SASL\_SSL。|SASL\_SSL|
|hosts|初始连接的集群地址，内网（经典网络/VPC）地址端口号为10011，公网地址端口号为10012。请选择您对应Project所在的服务入口。服务入口列表参考：[服务入口](../../../../../intl.zh-CN/API 参考/服务入口.md#)。| -   example.com:10011
-   example.com:10012

 |
|topic|Kafka中的topic映射为日志服务中的Logstore名称，请提前创建好Logstore。|test-logstore-1|
|username|用户名映射为日志服务中的Project名称。|<yourusername\>|
|password|密码为您的AK信息，格式为$\{access-key-id\}\#$\{access-key-secret\}，请将其中的$\{access-key-id\}替换为您的AccessKey ID、$\{access-key-secret\}替换为您的AccessKey Secret。建议使用子账号AK，请参考[授权](intl.zh-CN/用户指南/         访问控制 RAM/授权RAM 用户.md#)。|<yourpassword\>|
|证书文件|日志服务的域名均具备可信任证书，您只需使用机器自带的根证书即可。|/etc/ssl/certs/ca-bundle.crt|

## 错误码说明 {#section_qpq_nrc_ghb .section}

使用Kafka协议采集日志信息，当采集失败时，会按照Kafka的错误码返回对应的错误类型（Kafka协议错误码请参考[error list](https://kafka.apache.org/20/javadoc/org/apache/kafka/common/errors/package-summary.html)），具体错误码、错误说明以及推荐解决方式请参考下述表格。

|错误码|说明|推荐解决方式|
|---|--|------|
|NetworkException|出现网络错误时返回该错误类型。|遇到该错误时您尝试等待1秒后重试即可。|
|TopicAuthorizationException|鉴权失败时返回该错误类型，一般是您提供的AccessKey错误或没有对应Project、Logstore的权限。|一般是您提供的AccessKey错误或没有写入对应Project、Logstore的权限。请填写正确的且具备写入权限的AccessKey。|
|UnknownTopicOrPartitionException| 出现该错误可能有两种原因：

 -   不存在对应的Project或Logstore。
-   Project所在地域与填入的endpoint不一致。

 | 1.  提前创建对应的Project和Logstore。
2.  若已经创建还是错误，请检查Project所在地域是否和填入的endpoint一致。

 |
|KafkaStorageException|服务端出现异常时返回该错误类型。|遇到该错误时您尝试等待1秒后重试即可。|

## 配置示例 {#section_gl1_ftc_ghb .section}

用户希望将数据采集到日志服务，日志服务Project名为`test-project-1`，Logstore名为`test-logstore-1`，Project所在地域为杭州（cn-hangzhou），具有写入权限的子账号AccessKey ID为`<yourAccessKeyId>`、AccessKey Secret为`<yourAccessKeySecret>`。

-   示例1：Beats采集数据到日志服务

    Beats（MetricBeat、PacketBeat、Winlogbeat、Auditbeat、Filebeat、Heartbeat等）系列软件采集均支持将数据输出到Kafka（详细配置请参考[Beats-Kafka-Output](https://www.elastic.co/guide/en/beats/filebeat/master/kafka-output.html)）。

    ```
    output.kafka: 
      # initial brokers for reading cluster metadata 
      hosts: ["example.com:10012"] 
      username: "<yourusername>" 
      password: "<yourpassword>" 
      ssl.certificate_authorities: 
    
      # message topic selection + partitioning 
      topic: 'test-logstore-1' 
      partition.round_robin: 
        reachable_only: false 
     
      required_acks: 1 
      compression: gzip 
      max_message_bytes: 1000000
    ```

    Beats默认输出到Kafka的日志为JSON类型，您可以给content字段创建JSON类型的索引（创建方式参考[JSON类型](intl.zh-CN/用户指南/查询与分析/索引数据类型/JSON类型.md#)），日志样例如下：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/150479/155494703441999_zh-CN.png)

-   示例2：Collectd采集数据到日志服务

    [Collectd](https://collectd.org/)是一个守护（daemon）进程，用来定期收集系统和应用程序的性能指标，Collectd也支持将数据输出到Kafka中（插件请参考[Write Kafka Plugin](https://collectd.org/wiki/index.php/Plugin:Write_Kafka)）。

    Collectd输出到Kafka需要额外安装Kafka插件以及相关依赖，在Centos下，您可以直接使用yum安装（RPM参见[Collectd-write\_kafka](https://rpmfind.net/linux/rpm2html/search.php?query=collectd-write_kafka)），命令为`sudo yum install collectd-write_kafka`。

    配置模板样例如下：

    ```
    <Plugin write_kafka>
      Property "metadata.broker.list" "example.com:10012" 
      Property "security.protocol" "sasl_ssl" 
      Property "sasl.mechanism" "PLAIN" 
      Property "sasl.username" "<yourusername>" 
      Property "sasl.password" "<yourpassword>" 
      Property "broker.address.family" "v4"  
      <Topic "test-logstore-1">
        Format JSON 
        Key "content"  
      </Topic>
    </Plugin>
    
    
    ```

    上述示例中将输出到Kafka的Format设置为JSON，除JSON外Collectd还支持Command、Graphite类型（具体请参考[Collectd配置文档](https://collectd.org/documentation/manpages/collectd.conf.5.shtml)）。

    使用JSON模式采集后，您可以给content字段创建JSON类型的索引（创建方式参考[JOSN索引类型](intl.zh-CN/用户指南/查询与分析/索引数据类型/JSON类型.md#)），日志样例如下：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/150479/155494703442000_zh-CN.png)

-   示例3：使用Telegraf采集数据到日志服务

    [Telegraf](https://github.com/influxdata/telegraf)是 InfluxData 下的子项目，是由 Go 语言编写的 metrics 收集、处理、聚合的代理。其设计目标是较小的内存使用，通过插件来构建各种服务和第三方组件的 metrics 收集。Telegraf 具有插件或集成功能，可以直接从其运行的系统中获取各种指标，从第三方API中提取指标，甚至通过 statsd 和 Kafka 消费者服务监听指标。

    Telegraf支持将数据发送到Kafka，因此您只需修改配置文件即可使用Telegraf采集数据并发送到日志服务：

    ```
    [[outputs.kafka]] 
      ## URLs of kafka brokers 
      brokers = ["example.com:10012"] 
      ## Kafka topic for producer messages 
      topic = "test-logstore-1" 
      routing_key = "content" 
    
      ## CompressionCodec represents the various compression codecs recognized by 
      ## Kafka in messages. 
      ## 0 : No compression 
      ## 1 : Gzip compression 
      ## 2 : Snappy compression 
      ## 3 : LZ4 compression 
      compression_codec = 1 
    
      ## Optional TLS Config tls_ca = "/etc/ssl/certs/ca-bundle.crt" 
      # tls_cert = "/etc/telegraf/cert.pem" # tls_key = "/etc/telegraf/key.pem" 
      ## Use TLS but skip chain & host verification 
      # insecure_skip_verify = false 
    
      ## Optional SASL Config 
      sasl_username = "<yourusername>" 
      sasl_password = "<yourpassword>" 
    
      ## Data format to output. 
      ## https://github.com/influxdata/telegraf/blob/master/docs/DATA_FORMATS_OUTPUT.md 
      data_format = "json"
    ```

    **说明：** Telegraf必须配置一个合法的`tls_ca`路径，您只需使用机器自带的根证书即可。通常Linux环境下根证书CA路径为`/etc/ssl/certs/ca-bundle.crt`。

    上述示例中将输出到Kafka的Format设置为JSON，除JSON外还支持Graphite、Carbon2等类型（具体请参考[Telegraf输出格式](https://github.com/influxdata/telegraf/blob/master/docs/DATA_FORMATS_OUTPUT.md)）。

    使用JSON模式采集后，您可以给content字段创建JSON类型的索引（创建方式参考[JOSN索引类型](intl.zh-CN/用户指南/查询与分析/索引数据类型/JSON类型.md#)），日志样例如下：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/150479/155494703442180_zh-CN.png)

-   示例4：使用Fluentd采集数据到日志服务

    [Fluentd](https://www.fluentd.org/)是一个用于统一日志层的开源数据收集器。Fluentd允许您统一收集数据，以便更好地使用和理解数据。Fluentd是云端原生计算基金会\(CNCF\)的成员项目之一，遵循Apache 2 License协议。

    Fluentd具有众多输入、处理、输出插件，其中[Kafka插件](https://github.com/fluent/fluent-plugin-kafka)支持将数据发送到Kafka，您只需[安装](https://github.com/fluent/fluent-plugin-kafka)并配置该插件即可将数据采集到日志服务。

    配置样例如下：

    ```
    <match **>
      @type kafka 
    
      # Brokers: you can choose either brokers or zookeeper. 
      brokers      example.com:10012 
    
      default_topic test-logstore-1 
      default_message_key content 
      output_data_type json 
      output_include_tag true 
      output_include_time true 
      sasl_over_ssl true 
      username <yourusername> 
      password <yourpassword> 
      ssl_ca_certs_from_system true 
    
      # ruby-kafka producer options 
      max_send_retries 10000 
      required_acks 1 
      compression_codec gzip 
    </match>
    ```

    上述示例中将输出到Kafka的Format设置为JSON，除JSON外还支持数十种Format类型（具体请参考[Fluentd Formatter](https://docs.fluentd.org/v1.0/articles/formatter-plugin-overview)）。

    使用JSON模式采集后，您可以给content字段创建JSON类型的索引（创建方式参考[JOSN索引类型](intl.zh-CN/用户指南/查询与分析/索引数据类型/JSON类型.md#)），日志样例如下：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/150479/155494703442204_zh-CN.png)

-   示例5：使用Logstash采集数据到日志服务

    [Logstash](https://www.elastic.co/products/logstash)是一个具备实时处理能力开源的数据收集引擎。可以动态地从不同的来源收集数据，将数据处理（过滤、变形）过之后统一输出到某个特定地址，为将来更多样化的数据分析做准备。

    Logstash内置了Kafka输出插件，因此您可以直接配置Logstash将数据采集到日志服务。由于日志服务Kafka协议使用SASL\_SSL连接协议，因此需要额外配置SSL证书、SASL的jaas文件。

    1.  创建jaas文件，保存到任意路径，例如： /etc/kafka/kafka\_client\_jaas.conf。

        ```
        KafkaClient { 
          org.apache.kafka.common.security.plain.PlainLoginModule required 
          username="<yourusername>" 
          password="<yourpassword>"; 
        };
        ```

    2.  配置SSL信任证书，保存到任意路径，例如：/etc/kafka/client-root.truststore.jks。

        日志服务的域名均为可信任证书，您只需下载[GlobalSign Root CA](https://support.globalsign.com/customer/portal/articles/1426602-globalsign-root-certificates)根证书即可，保存base64编码的根证书到任意路径，例如/etc/kafka/ca-root。然后输入[keytool](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html)命令生成jks格式的文件（首次生成jks您需要配置密码）。

        ```
        keytool -keystore client.truststore.jks -alias root -import -file /etc/kafka/ca-root
        ```

    3.  配置Logstash，示例如下：

        ```
        input { stdin { } } 
        
        output { 
          stdout { codec => rubydebug } 
          kafka { 
            topic_id => "test-logstore-1" 
            bootstrap_servers => "example.com:10012" 
            security_protocol => "SASL_SSL" 
            ssl_truststore_location => "/etc/client-root.truststore.jks" 
            ssl_truststore_password => "123456" 
            jaas_path => "/etc/kafka_client_jaas.conf" 
            sasl_mechanism => "PLAIN" 
            codec => "json" 
            client_id => "kafka-logstash" 
          } 
        }
        ```

        **说明：** 上述示例为连通性测试的配置，您的生产环境中建议删除stdout的输出配置。

        上述示例中将输出到Kafka的Format设置为JSON，除JSON外还支持数十种Format类型（具体请参考[Logstash Codec](https://www.elastic.co/guide/en/logstash/current/codec-plugins.html)）。

        使用JSON模式采集后，您可以给content字段创建JSON类型的索引（创建方式参考[JOSN索引类型](intl.zh-CN/用户指南/查询与分析/索引数据类型/JSON类型.md#)），日志样例如下：

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/150479/155494703442205_zh-CN.png)


