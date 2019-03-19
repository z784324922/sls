# Overview {#concept_q2h_g4q_zdb .concept}

After you access a log source to Log Service, Log Service starts to collect logs in real time and allows you to consume and ship logs in the console or by using SDKs/APIs. Log Service can ship logs collected to LogHub to Alibaba Cloud storage products such as Object  Storage Service \(OSS\) and Table Store in real time. You can configure to ship logs in the console and LogShipper provides a complete status API and automatic retry function.

## Application scenarios {#section_ok4_njm_12b .section}

Interconnection with the data warehouse

## Log source {#section_ozc_njm_12b .section}

The LogShipper function of Log Service ships logs that are collected to LogHub.  After logs are generated, Log Service collects these logs in real time and ships them to other cloud products for storage and analysis.

## Targets {#section_z54_4jm_12b .section}

-   OSS \(large-scale object storage\)
    -   [Ship logs to OSS](intl.en-US/User Guide/Data shipping/Ship logs to OSS/Ship logs to OSS.md)
    -   Formats in OSS can be processed by using Hive. E-MapReduce is recommended.
-   Table Store \(NoSQL data storage service\)
-   Maxcompute \(large data computing services \):
    -   Delivery via dataworks Data Integration-operation -[Ship data to MaxCompute by using DataWorks](intl.en-US/User Guide/Data shipping/Ship data to MaxCompute/Ship data to MaxCompute by using DataWorks.md)

