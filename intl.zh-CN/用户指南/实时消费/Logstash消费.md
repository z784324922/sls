# Logstash消费 {#concept_473580 .concept}

日志服务除了支持各种语言、流计算消费方式外，还支持通过Logstash消费日志数据，您可以直接配置日志服务的[Logstash输入源插件](https://github.com/aliyun/logstash-input-logservice)来获取日志服务中数据，写入到其他系统中，例如Kafka、HDFS等。

## 功能特性 {#section_g0s_xla_135 .section}

-   **支持分布式协同消费**：可配置多台服务器同时消费某一Logstore。
-   **高性能**：基于Java ConsumerGroup实现，单核消费速度可达20MB/s。
-   **高可靠**：消费进度保存到服务端，异常恢复后会从上一次消费的checkpoint处自动恢复消费。
-   **自动负载均衡**：根据消费者数量自动分配Shard，消费者增加或退出后会自动Rebalance。

