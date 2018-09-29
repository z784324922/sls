# Overview {#concept_mnc_1jq_32b .concept}

Alibaba Cloud Anti-DDoS Pro is a paid service for Internet servers \(including non-Alibaba Cloud hosts\). To avoid the risk of service unavailability after large traffic DDoS attack, paid service can be applied. Configure Anti-DDoS Pro, and drain the attack traffic for high IP protection to ensure that the source is stable and reliable.

## Background information {#section_up5_msr_k2b .section}

The security of the Internet community has been constantly facing challenges. Network threats represented by DDoS attacks have a serious impact on the network security.

DDoS attacks are moving towards large-scale, mobile and global development. According to recent survey reports, the frequency of DDoS attacks is on the rise. The hacker attacks are concealed, and can control a large number of cloud service providers with poor security measures, IDC, and even massive cameras to launch attacks. The attacks have formed a mature black industry chain, which getting more organized. At the same time, the attack mode develops toward polarization, and the proportion of slow attacks, mixed attacks, especially CC attacks increases, which makes the detection of the defense more difficult. The peak of attacks exceeding 1Tbps are common , and the number of 100 GB attacks has doubled. However, application layer attacks are also increasing significantly.

According to [Kaspersky 2018Q1 DDoS Risk Report](https://securelist.com/DDoS-report-in-q1-2018/85373/), China remains the main source of DDoS attacks and targets. The main industries that have being attacked are Internet, games, software, and finance companies. More than 80% of DDoS attacks mix HTTP and CC attacks, and have a high level of concealment. Therefore, it is especially important to analyze the access and attack behavior by using logs, and apply a protection strategy.

Log Service supports real-time collection of Alibaba Cloud Anti-DDoS Pro website access logs, CC attack logs, and supports real-time query and analysis of collected log data. The results of the query are displayed in the form of dashboards.

## Functional advantages {#section_tvv_wly_32b .section}

-   **Simple configuration**: Easily configure to capture real-time protected logs.
-   **Real-time analysis**: Relying on Log Service, it provides real-time log analysis and out-of-box report center, that gives information about CC attack status and customer access details. 
-   **Real-time alarms**: Supports custom monitoring and alarms based on specific indicators in real time to provide timely response to critical business exceptions.  
-   **Ecosystem**: Supports the docking of other ecosystems, such as stream computing, cloud storage, and visualization solutions for the further data value exploration.
-   **FreeTier quota**: Provides a free data import quota, and three days free log storage, query and real-time analysis. You can freely expand your storage time for compliance management, tracing, and filing. Support unlimited storage time, and the storage cost is 0.35 USD/GB per month.

## Limits and instructions {#section_lwt_czf_j2b .section}

-   **Exclusive Logstores do not support writing additional data**.

    Exclusive Logstore is used to store Anti-DDoS Pro website logs, so **writing other data is not supported**. There are no restrictions on other functions such as query, statistics, alarms, and streaming consumption.

-   **Pay-As-You-Go billing method  If DDoS log collection protection is not enabled for any website, no charge appears.**

    DDOS log collection function is billed according to the charge item of Log Service. If DDoS log collection function is not enabled for any website, no charge appears. Log Service supports **Pay-As-You-Go** billing method, and provides **FreeTier quota**. For more information, see [Billing method](reseller.en-US/User Guide/Cloud product collection/DDoS log collection/Billing method.md).


## Scenarios {#section_tbr_34y_32b .section}

-   **Troubleshoot website access exceptions**

    Log Service has been configured to collect DDoS logs, you can query and analyze the collected logs in real time. Using SQL statement to analyze the DDoS access log, you can quickly check and analyze the website access exceptions, and view information such as read and write delays and operator distribution.  

    For example, view the DDoS access log by using the following statement:

    ```
    __topic__: ddos_access_log
    ```

-   **Track CC attack source**

    The distribution and source of CC attacks are recorded in the DDoS access log. By performing real-time query and analysis on the DDoS access log, you can conduct source tracking, trace CC attacks, and provide a reference for response strategy. 

    For example, analyze the CC attack country distribution recorded in the DDoS access log by the following statement:

    ```
    __topic__: ddos_access_log and cc_blocks > 0| SELECT ip_to_country(if(real_client_ip='-', remote_addr, real_client_ip)) as country, count(1) as "number of attacks" group by country
    ```

-   For example, view the PV access by the following statement:

```
__topic__: ddos_access_log | select count(1) as PV
```

-   **Website operation analysis**

    DDoS access log records the website access data in real time. You can perform SQL query analysis of the collected access log data to obtain real-time access status, such as determining the website popularity, the source and channel of the access, the client distribution, and assist in website operation analysis.

    For example, view the visitor traffic distribution from different network clouds:

    ```
    __topic__: ddos_access_log | select ip_to_provider(if(real_client_ip='-', remote_addr, real_client_ip)) as provider, round(sum(request_length)/1024.0/1024.0, 3) as mb_in group by provider having ip_to_provider(if(real_client_ip='-', remote_addr, real_client_ip)) <> '' order by mb_in desc limit 10
    ```


