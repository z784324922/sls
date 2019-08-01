# 日志采集功能与 Kafka对比 {#concept_29088_zh .concept}

Kafka是分布式消息系统，由于其高吞吐和水平扩展，被广泛使用于消息的发布和订阅。以开源软件的方式提供，各用户可以根据需要搭建Kafka集群。

日志服务\(Log Service\)是基于飞天Pangu构建的针对日志平台化服务。服务提供各种类型日志的实时采集，存储，分发及实时查询能力。通过标准话的Restful API对外提供服务。

Log Service Loghub提供公共的日志采集、分发通道，用户如果不想自己搭建、运维kafka集群，可以使用Log Service LogHub功能。

|概念|Kafka|Loghub|
|:-|:----|:-----|
|存储对象|topic|logstore|
|水平分区|partition|shard|
|数据消费位置|offset|cursor|

## Loghub & Kafka 功能比较 { .section}

|功能|Kafka|LogHub|
|:-|:----|:-----|
|使用依赖|自建或共享Kafka集群|Log Service服务|
|通信协议|TCP 打通网络|Http \(restful API\)，80端口|
|访问控制|无|基于云账号的签名认证+访问控制|
|动态扩容|暂无|支持动态shard个数弹性伸缩\(Merge/Split\)，对用户无影响|
|多租户Qos|暂无|基于shard的标准化流控|
|数据拷贝数|用户自定义|暂不开放，默认3份拷贝|
|failover/replication|调用工具完成|自动，用户无感知|
|扩容/升级|调用工具完成，影响服务|用户无感知|
|写入模式|round robin/key hash|暂只支持round robin/key hash|
|当前消费位置|保存在kafka集群的zookeeper|服务端维护、无需关心|
|保存时间|配置确定|根据需求动态调整|

## 成本对比 { .section}

参见[../../../../dita-oss-bucket/SP\_7/DNSLS11885958/ZH-CN\_TP\_13015.md](../../../../cn.zh-CN/产品简介/产品优势/成本优势.md)中LogHub部分。

