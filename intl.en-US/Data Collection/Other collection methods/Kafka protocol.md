# Kafka protocol {#concept_n3m_pkc_ghb .concept}

In addition to the Logtail, SDK, and API, Log Service also allows you to write data into Log Service in compliance with the Kafka protocol. You can use the Kafka Producer SDK in various languages and collection agents that can export the collected data to Kafka.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/150479/156574389344691_en-US.png)

## Limits {#section_gg2_34c_ghb .section}

-   The supported Kafka protocol versions are from Kafka 0.8.0 to Kafka 2.1.1.
-   You must use the SASL\_SSL connection protocol for secure data transmission.
-   If your Logstore contains multiple shards, you need to write data in load balancing mode.
-   Currently, you can use only the producer or agent to write data into Log Service in compliance with the Kafka protocol.

## Configuration {#section_mxs_54c_ghb .section}

If you use the Kafka protocol to collect data, you must set some parameters. The following table describes the required parameters.

|Parameter|Description|Example|
|---------|-----------|-------|
|Connection protocol|The connection protocol for secure data transmission. You must use SASL\_SSL.|SASL\_SSL|
|hosts|The cluster address for the initial connection. The port number for an intranet \(either a classic network or VPC\) address is 10011. The port number for an Internet address is 10012. You need to select the service endpoint where your target project is located. For more information, see [Service endpoint](../../../../reseller.en-US/API Reference/Service endpoint.md#).| -   cn-hangzhou-intranet.log.aliyuncs.com:10011
-   cn-hangzhou.log.aliyuncs.com:10012

 |
|topic|The mapped Logstore name in Log Service. You must create a Logstore in advance.|test-logstore-1|
|username|The mapped project name in Log Service.|<yourusername\>|
|password|The information about your AccessKey, which is in the format of $\{access-key-id\}\#$\{access-key-secret\}. You need to replace $\{access-key-id\} with your AccessKey ID and $\{access-key-secret\} with your AccessKey Secret. We recommend that you use the AccessKey of a RAM user. For more information, see [Grant a RAM user the permission to access Log Service](reseller.en-US/Access control RAM/Grant a RAM user the permissions to access Log Service.md#).|<yourpassword\>|
|Certificate|The directory of the certificate. Each domain name in Log Service has a CA certificate. You only need to use the default root certificate.|/etc/ssl/certs/ca-bundle.crt|

## Error codes {#section_qpq_nrc_ghb .section}

If you fail to collect log data in compliance with the Kafka protocol, the system returns a Kafka error code for the specific cause of failure. For more information about Kafka error codes, see the [error list](https://kafka.apache.org/20/javadoc/org/apache/kafka/common/errors/package-summary.html). The following table describes the specific error codes, description, and corresponding solutions.

|Error code|Description|Solution|
|----------|-----------|--------|
|NetworkException|The error message returned because a network error has occurred.|Wait for 1 second and try again.|
|TopicAuthorizationException|The error message returned because the authentication fails. Generally, your AccessKey is invalid or has no permission to write data into the corresponding project or Logstore.|Enter a valid AccessKey and ensure that it has the required write permission.|
|UnknownTopicOrPartitionException| The error message returned because either of the following errors has occurred:

 -   The corresponding project or Logstore does not exist.
-   The region where the project is located is different from the region indicated by the endpoint that you entered.

 | 1.  Create a project and a Logstore in advance.
2.  Ensure that the region where the project is located is the same as the region indicated by the endpoint that you entered.

 |
|KafkaStorageException|The error message returned because a server error has occurred.|Wait for 1 second and try again.|

## Example {#section_gl1_ftc_ghb .section}

You want to write data into Log Service. The project in Log Service is named `test-project-1` and the Logstore is named `test-logstore-1`. The region where the project is located is cn-hangzhou. The AccessKey ID of the RAM user with the corresponding write permission is `<yourAccessKeyId>`, and the AccessKey Secret is `<yourAccessKeySecret>`.

-   Example 1: Use Beats software to write data into Log Service

    You can export the collected data to Kafka by using Beats software such as Metricbeat, Packetbeat, Winlogbeat, Auditbeat, Filebeat, and Heartbeat. For more information, see [Configure the Kafka output](https://www.elastic.co/guide/en/beats/filebeat/master/kafka-output.html). The sample code is as follows:

    ``` {#codeblock_kzn_4ud_cvd}
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

    By default, the Beats software exports JSON-formatted logs to Kafka. You can also create a JSON type index for the content field. For more information, see [JSON type](reseller.en-US/Index and query/Data type of index/JSON type.md#). The following figure shows a log sample.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/150479/156574389341999_en-US.png)

-   Example 2: Use Collectd to write data into Log Service

    [Collectd](https://collectd.org/) is a daemon used to collect the performance metrics of a system or application on a regular basis. You can also use Collectd to export the collected data to Kafka. For more information, see the [Write Kafka plug-in](https://collectd.org/wiki/index.php/Plugin:Write_Kafka).

    If you want to export the collected data from Collectd to Kafka, you need to install the Write Kafka plug-in and relevant dependencies. In the CentOS, you can directly run the `sudo yum install collectd-write_kafka` command to install the plug-in. For more information about the Red-Hat Package Manager \(RPM\) resources, see [RPM resource collectd-write\_kafka](https://rpmfind.net/linux/rpm2html/search.php?query=collectd-write_kafka).

    The sample code is as follows:

    ``` {#codeblock_vx8_reg_ss3}
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

    In the preceding sample code, the format of the data exported to Kafka is set to JSON. In addition to JSON, Collectd also supports the Command and Graphite formats. For more information, see the [Collectd configuration documentation](https://collectd.org/documentation/manpages/collectd.conf.5.shtml).

    If you use the JSON format, you can create a JSON type index for the content field. For more information, see [JSON type](reseller.en-US/Index and query/Data type of index/JSON type.md#). The following figure shows a log sample.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/150479/156574389342000_en-US.png)

-   Example 3: Use Telegraf to write data into Log Service

    [Telegraf](https://github.com/influxdata/telegraf) is a sub-project of InfluxData. It is the agent compiled in Go for collecting, processing, and aggregating metrics. It is designed to use less memory resources. Telegraf can be used to build services and collect the metrics of a third-party component through plug-ins. In addition, Telegraf has the integration feature. It can obtain metrics from the system where it runs, obtain metrics through a third-party API, and even monitor metrics through StatsD and Kafka consumer services.

    Telegraf can export data to Kafka. Therefore, you only need to modify the configuration file to use Telegraf to collect data and write data into Log Service. The sample code is as follows:

    ``` {#codeblock_e1l_iin_9s1}
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

    In the preceding sample code, the format of the data exported to Kafka is set to JSON. In addition to JSON, Telegraf also supports other formats such as Graphite and Carbon2. For more information, see [Telegraf output data formats](https://github.com/influxdata/telegraf/blob/master/docs/DATA_FORMATS_OUTPUT.md).

    If you use the JSON format, you can create a JSON type index for the content field. For more information, see [JSON type](reseller.en-US/Index and query/Data type of index/JSON type.md#). The following figure shows a log sample.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/150479/156574389442180_en-US.png)

-   Example 4: Use Fluentd to write data into Log Service

    [Fluentd](https://www.fluentd.org/) is an open-source data collector that provides a unified logging layer. It allows you to collect data in a uniform manner so that you can easily use and understand data. Fluentd is a member project of Cloud Native Computing Foundation \(CNCF\). It complies with the Apache 2 License protocol.

    Fluentd provides many input, processing, and output plug-ins. Specifically, the [Kafka plug-in](https://github.com/fluent/fluent-plugin-kafka) can help Fluentd export data to Kafka. You only need to [install](https://github.com/fluent/fluent-plugin-kafka) and configure this plug-in.

    The sample code is as follows:

    ``` {#codeblock_eg4_gva_lce}
    <match **>
      @type kafka 
      # Brokers: You can choose either brokers or zookeeper. 
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

    In the preceding sample code, the format of the data exported to Kafka is set to JSON. In addition to JSON, Fluentd also supports more than 10 formats. For more information, see [Fluentd Formatter](https://docs.fluentd.org/v1.0/articles/formatter-plugin-overview).

    If you use the JSON format, you can create a JSON type index for the content field. For more information, see [JSON type](reseller.en-US/Index and query/Data type of index/JSON type.md#). The following figure shows a log sample.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/150479/156574389442204_en-US.png)

-   Example 5: Use Logstash to write data into Log Service

    [Logstash](https://www.elastic.co/products/logstash) is an open-source engine for collecting data in real time. Using Logstash, you can dynamically collect data from different sources, process the data \(for example, filter or convert the data\), and export the result to a target address. You can analyze the data further based on the output result.

    Logstash provides a built-in Kafka output plug-in. It allows you to directly enable Logstash to write data into Log Service. However, you must configure the SSL certificate and the SASL jass file because Log Service uses the SASL\_SSL connection protocol in compliance with the Kafka protocol.

    1.  Create a jaas file, and then save it to a target directory, such as /etc/kafka/kafka\_client\_jaas.conf.

        ``` {#codeblock_4t6_i8l_frt}
        KafkaClient { 
          org.apache.kafka.common.security.plain.PlainLoginModule required 
          username="<yourusername>" 
          password="<yourpassword>"; 
        };
        ```

    2.  Set the SSL certificate, and then save it to a target directory, such as /etc/kafka/client-root.truststore.jks.

        Each domain name in Log Service has a CA certificate. You only need to download the [GlobalSign Root CA](https://support.globalsign.com/customer/portal/articles/1426602-globalsign-root-certificates), and save the Base64-encoded root certificate to a target directory, such as /etc/kafka/ca-root. Then, run a [keytool](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html) command to generate a JKS file. When you generate a JKS file for the first time, you need to set a password.

        ``` {#codeblock_1et_szm_wqy}
        keytool -keystore client.truststore.jks -alias root -import -file /etc/kafka/ca-root
        ```

    3.  Configure the Logstash. The sample code is as follows:

        ``` {#codeblock_7jq_kil_yvf}
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

        **Note:** The configuration in the preceding sample code is used for a connectivity test. In actual applications, we recommend that you remove the stdout output configuration.

        In the preceding sample code, the format of the data exported to Kafka is set to JSON. In addition to JSON, Logstash also supports more than 10 formats. For more information, see [Logstash Codec plug-ins](https://www.elastic.co/guide/en/logstash/current/codec-plugins.html).

        If you use the JSON format, you can create a JSON type index for the content field. For more information, see [JSON type](reseller.en-US/Index and query/Data type of index/JSON type.md#). The following figure shows a log sample.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/150479/156574389442205_en-US.png)


