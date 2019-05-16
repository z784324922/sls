# Use the Kafka protocol to collect data to Log Service {#concept_n3m_pkc_ghb .concept}

This topic describes how to use the Kafka protocol to collect data to Log Service.

## Limits {#section_gg2_34c_ghb .section}

-   Supported Kafka protocol versions are 0.8.0 to 2.1.IV2.
-   You must use the SASL\_SSL connection protocol for secure data transmission.
-   If multiple shards are contained in your Logstore, enable load balance mode before you enter data.

## Required parameters {#section_mxs_54c_ghb .section}

If you use the Kafka protocol, you must set the following parameters.

|Configuration|Description|Example|
|-------------|-----------|-------|
|Connection protocol|You must use the SASL\_SSL connection protocol for secure data transmission.|SASL\_SSL|
|`hosts`|The cluster address for the initial connection. The port number of an intranet \(for either a Classic network or VPC\) address is 10011. The port number of the Internet address is 10012. You need to select the service port where your target project is located. For more information, see [Service endpoint](../../../../reseller.en-US/API Reference/Service endpoint.md#).| cn-hangzhou-intranet.log.aliyuncs.com:10011

 cn-hangzhou.log.aliyuncs.com:10012

 |
|`topic`|The `topic` parameter in Kafka is mapped to the Logstore name in Log Service. You must create a Logstore before you begin to collect data.|test-logstore-1|
|`username`|This parameter is mapped to the project name in Log Service.|<yourusername\>|
|`password`|This parameter denotes your AK, which is in the format of $\{access-key-id\}\#$\{access-key-secret\}. You need to replace $\{access-key-id\} with your AccessKey ID, and replace $\{access-key-secret\} with your AccessKey Secret. Furthermore, we recommend that you use the AK of a RAM user. For more information, see [Grant a RAM user the permissions to access Log Service](reseller.en-US/User Guide/Access control RAM/Grant a RAM user the permissions to access Log Service.md#).|<yourpassword\>|
|`Certificate`|Each domain name in Log Service has a valid certificate. You only need to use the default root certificate.|/etc/ssl/certs/ca-bundle.crt|

## Errors {#section_qpq_nrc_ghb .section}

If you fail to use the Kafka protocol to collect log data, the system returns a corresponding error code for the specific cause of failure. For more information about the errors corresponding the Kafka protocol, see [Error list](https://kafka.apache.org/20/javadoc/org/apache/kafka/common/errors/package-summary.html). The following are common errors and their description and corresponding solutions.

|Error code|Description|Solution|
|----------|-----------|--------|
|NetworkException|Indicates a network failure.|Try again one second later.|
|TopicAuthorizationException|Indicates an authorization exception. Your AccessKey is invalid or does not have the permissions for the corresponding project or Logstore.

 |Enter a valid AccessKey and make sure that it has the required permissions.|
|UnknownTopicOrPartitionException|Indicates that the topic or partition does not exist. The error can be caused by either of the following conditions:

 -   The corresponding project or Logstore does not exist.
-   The region where the project is located is different from the region indicated by the endpoint that you entered.

 | Check if you have created the corresponding project and Logstore before you collect data.

 Check if the region where the project is located is the same as the region indicated by the endpoint that you entered.

 |
|KafkaStorageException|Indicates a server exception.|Try again one second later.|

## Configuration examples {#section_gl1_ftc_ghb .section}

In this example, data is collected to Log Service. The project in Log Service is named `test-project-1`, and the Logstore is named `test-logstore-1`. The region where the project is located is `cn-hangzhou`. The AccessKey ID with the corresponding write permission is `<yourAccessKeyId>`, and the AccessKey Secret is `<yourAccessKeySecret>`.

-   Example 1: Use Beats software to collect data to Log Service

    You can output the collected data to Kafka by using Beats software such as MetricBeat, PacketBeat, Winlogbeat,Auditbeat, Filebeat, and Heartbeat. For more information, see [Beats-Kafka-Output](https://www.elastic.co/guide/en/beats/filebeat/master/kafka-output.html).

    ```
    output.kafka: 
           # initial brokers for reading cluster metadata 
           hosts: ["cn-hangzhou.log.aliyuncs.com:10012"] 
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

    By default, the Beats software outputs the JSON type logs to Kafka. You can also create a JSON type index for the content field. For more information, see [JSON type](reseller.en-US/User Guide/Index and query/Data type of index/JSON type.md#). The following is a log example.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/150479/155797578041999_en-US.png)

-   Example 2: Use Collectd to collect data to Log Service

    [Collectd](https://collectd.org/) is a daemon used to collect the performance metrics of system and application on regular basis. You can also use Collectd to output collected data to Kafka. For more information, see [Write Kafka plugin](https://collectd.org/wiki/index.php/Plugin:Write_Kafka).

    If you want to output the collected data to Kafka by using Collectd, you need to install the Kafka plugin and related dependency. In the Centos operating system, you can directly run the `sudo yum install collectd-write_kafka` command to install the plugin. For more information about the Red-Hat Package Manager \(RPM\) resources, see [Collectd-write\_kafka](https://rpmfind.net/linux/rpm2html/search.php?query=collectd-write_kafka).

    The following is an example configuration template:

    ```
    <Plugin write_kafka>
           Property "metadata.broker.list" "cn-hangzhou.log.aliyuncs.com:10012" 
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

    This example configuration template shows that the format of the data output to be sent to Kafka is set to JSON. In addition to JSON, Collectd also supports the Command and Graphite formats. For more information, see [Collectd configuration document](https://collectd.org/documentation/manpages/collectd.conf.5.shtml).

    If you use JSON format, you can create a JSON type index for the content field. For more information, see [JOSN index types](reseller.en-US/User Guide/Index and query/Data type of index/JSON type.md#). The following is a log example.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/150479/155797578042000_en-US.png)

-   Example 3: Use Telegraf to collecte data to Log Service

    [Telegraf](https://github.com/influxdata/telegraf) is a sub-project of InfluxData. Telegraf is the agent for collecting, processing, and aggregating metrics. It is compiled by the Go language, and designed to use less memory resource. Telegraf can be used to build services and collect the metrics of a third party component through plugins. In addtion, Telegraf has the integration function, which can obtain metrics in the system where Telegraf runs, obtain metrics from a third party API, and even listen for metrics through StatsD and Kafka consumer services.

    Telegraf can output data to Kafka. Therefore, you only need to modify the configuration file to use Telegraf to collect data to Log Service.

    ```
    [[outputs.kafka]] 
           ## URLs of kafka brokers 
           brokers = ["cn-hangzhou.log.aliyuncs.com:10012"] 
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

    **Note:** You must set a valid `tls_ca` directory for Telegraf. You can use the default root certificate. The typical root certificate directory in a Linux environment is `/etc/ssl/certs/ca-bundle.crt`.

    This example configuration template indicates that the format of the data output to Kafka is set as JSON. Besides JSON, Graphite, Carbon2, and other formats are also supported. For more information, see [Telegraf output data format](https://github.com/influxdata/telegraf/blob/master/docs/DATA_FORMATS_OUTPUT.md).

    If you use the JSON format, you can create a JSON type index for the content field. For more information, see [JOSN index types](reseller.en-US/User Guide/Index and query/Data type of index/JSON type.md#). The following is a log example.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/150479/155797578042180_en-US.png)

-   Example 4: Use Fluentd to collect data to Log Service

    [Fluentd](https://www.fluentd.org/) is an open-source data collector for Unified Logging Layer. Fluentd allows you to unify data collection and usage for overall better optimization. Fluentd a Cloud Native Computing Foundation \(CNCF\) member project, and follows the Apache 2 License protocol.

    Many plugins for inputting, processing, and outputting data are available for Fluentd. Specifically, the [Kafka](https://github.com/fluent/fluent-plugin-kafka) plugin can help Fluentd output data to Kafka. You only need to install and configure this plugin. For more information, see [fluent-plugin-kafka](https://github.com/fluent/fluent-plugin-kafka).

    The following is an example configuration temple:

    ```
    <match **>
           @type kafka 
    
           # Brokers: you can choose either brokers or zookeeper. 
           brokers      cn-hangzhou.log.aliyuncs.com:10012 
    
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

    This example configuration template shows that the format of the data output to be sent to Kafka is set to JSON. In addition to JSON, more formats are also supported. For more information, see [Fluentd formatter](https://docs.fluentd.org/v1.0/articles/formatter-plugin-overview).

    If you use the JSON format, you can create a JSON type index for the content field. For more information, see [JOSN index types](reseller.en-US/User Guide/Index and query/Data type of index/JSON type.md#). The following is a log example.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/150479/155797578042204_en-US.png)

-   Example 5: Use Logstash to collect data to Log Service

    [Logstash](https://www.elastic.co/products/logstash) is an open-source engine for collecting data in real time. With Logstash, you can dynamically collect data from different sources, process the data \(for example, filter or convert the data\), and output the result. You can analyze the data further based on the output result.

    The Kafka output plugin is built in Logstash, allowing you to directly configure Logstash to collect data to Log Service. However, you must configure the jass files for the SSL certificate and SASL because the Kafka protocol uses the SASL\_SSL connection protocol.

    1.  Create a jaas file, and then save it to a target directory, such as /etc/kafka/kafka\_client\_jaas.conf.

        ```
        KafkaClient { 
                 org.apache.kafka.common.security.plain.PlainLoginModule required 
                 username="<yourusername>" 
                 password="<yourpassword>"; 
                 };
        ```

    2.  Set the SSL trusted certificate, and then save it to a target directory, such as /etc/kafka/client-root.truststore.jks.

        The domain names in Log Service all have trusted certificates. You only need to download the [GlobalSign Root CA](https://support.globalsign.com/customer/portal/articles/1426602-globalsign-root-certificates), and save the root certificate of base 64 code to any directory \(for example, /etc/kafka/ca-root\). Then run a [keytool](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html) command to generate a JKS format file. When you generate a JKS format file for the first time, you need to set a password.

        ```
        keytool -keystore client.truststore.jks -alias root -import -file /etc/kafka/ca-root
        ```

    3.  Configure the Logstash as follows:

        ```
        input { stdin { } } 
        
                 output { 
                 stdout { codec => rubydebug } 
                 kafka { 
                 topic_id => "test-logstore-1" 
                 bootstrap_servers => "cn-hangzhou.log.aliyuncs.com:10012" 
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

        **Note:** This example configuration template is only for a connectivity test. In actual applications, we recommend that you remove the stdout output settings.

        This example configuration template indicates that the format of the data output to Kafka is set as JSON. Other formats in addition to JSON are supported. For more information, see [Logstash Codec](https://www.elastic.co/guide/en/logstash/current/codec-plugins.html).

        If you use the JSON format, you can create a JSON type index for the content field. For more information, see [JOSN index types](reseller.en-US/User Guide/Index and query/Data type of index/JSON type.md#). The following is a log example.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/150479/155797578042205_en-US.png)


