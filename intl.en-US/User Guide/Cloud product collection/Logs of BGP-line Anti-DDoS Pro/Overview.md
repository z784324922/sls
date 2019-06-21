# Overview {#concept_p2k_q3h_1gb .concept}

Alibaba Cloud Anti-DDoS Pro provides BGP bandwidth resources that are exclusively available in Chinese Mainland to mitigate volumetric DDoS attacks. The service can scrub Terabits of attack traffic per second based on eight ISPs. Compared with earlier versions, Anti-DDoS Pro supports more reliable networks with less latency and provides quicker disaster recovery.

## Background {#section_up5_msr_k2b .section}

Security is always a challenge facing the Internet. Network threats represented by DDoS attacks have a serious impact on network security.

DDoS attacks are becoming more large-scale, mobile, and global. According to recent survey reports, the frequency of DDoS attacks is on the rise. Attackers are difficult to detect and can manipulate a large number of cloud service providers with poor security measures, IDCs, and even cameras to launch attacks. The attackers have formed a mature black industry chain and become increasingly organized. At the same time, the attack methods develop towards polarization. The proportion of slow and hybrid attacks, especially HTTP flood attacks, is increasing, which makes detection and defense more difficult. On the one hand, attacks peaking at 1 Tbit/s or higher become common, and the number of 100 GB attacks is multiplied. On the other hand, application layer attacks are also doubled.

According to [Kaspersky Lab DDoS Q1 2018 Intelligence Report](https://securelist.com/DDoS-report-in-q1-2018/85373/), China remains the main source and target of DDoS attacks. The main industries under attack include the Internet, games, software companies, and financial companies. More than 80% of DDoS attacks mix with HTTP attacks, and HTTP flood attacks are especially difficult to detect. Therefore, it is particularly important to analyze and study access and attack activities through logs and set protection policies accordingly.

Log Service can collect website access logs and HTTP flood attack logs of Alibaba Cloud Anti-DDoS Pro in real time. Log Service also supports real-time retrieval and analysis of the collected log data and displays the query results in the form of dashboards.

## Benefits {#section_tvv_wly_32b .section}

-   **Simple configuration**: Real-time anti-DDoS logs can be collected with simple configuration. Log collection is automatically enabled for new websites after they are added.
-   **Real-time analysis**: Relying on Log Service, this function provides real-time log analysis and an out-of-the-box report center. This helps you understand the HTTP attack status and customer access details.
-   **Real-time alerts**: Supports real-time monitoring and alerts based on customized metrics to ensure timely response to critical business exceptions.
-   **Collaboration**: This function can be integrated with real-time computing, cloud storage, visualization, and other data solutions to discover more data value.
-   **Free quota**: Provides a free data import quota of 3 TB and also allows you to use the log storage, query, and real-time analysis functions for 30 days for free.

## Restrictions and guidelines {#section_lwt_czf_j2b .section}

-   **Additional data cannot be written to the exclusive Logstore**.

    The exclusive Logstore is used to store Anti-DDoS Pro website logs. **Writing other data is not supported**. Other functions such as query, statistics, alerts, and streaming consumption are not restricted.

-   **The data TTL and total storage capacity of the exclusive Logstore cannot be modified.** 

    Purchase the storage capacity of the exclusive Logstore based on your business requirements. Up to 1,000 TB of storage capacity is supported, and logs can be stored for up to 180 days.


## Scenarios {#section_tbr_34y_32b .section}

-   **Troubleshoot website access exceptions** 

    After you configure Log Service to collect Anti-DDoS Pro logs, you can query and analyze the collected logs in real time. By using SQL statements to analyze the Anti-DDoS Pro access logs, you can quickly troubleshoot and analyze the website access exceptions. You can also query information such as read and write delays and exception distribution by ISP.

    For example, use the following statement to query the access log entries of Anti-DDoS Pro:

    ``` {#codeblock_fto_pyo_2t2}
    __topic__: DDoS_access_log
    ```

    The query results are displayed, as shown in the following figure.

    ![](images/6718_en-US.png "Access log entries of Anti-DDoS Pro")

-   **Track HTTP flood attack sources** 

    Anti-DDoS Pro access logs record information about the sources and distribution of HTTP flood attacks. You can query and analyze access logs in real time to identify the attackers, and use this information to select the most effective protection policy.

    For example, use the following statement to analyze the geographical distribution of HTTP flood attacks:

    ``` {#codeblock_efi_bf3_5xf}
    __topic__: DDoS_access_log and cc_blocks > 0| SELECT ip_to_country(if(real_client_ip='-', remote_addr, real_client_ip)) as country, count(1) as "number" group by country
    ```

    The analysis results are displayed in a dashboard as follows:

    ![](images/6719_en-US.png "HTTP flood attacks")

-   For example, use the following statement to view PVs:

``` {#codeblock_9ex_9a4_aio}
__topic__: DDoS_access_log | select count(1) as PV
```

    The analysis results are displayed in a dashboard, as shown in the following figure.

    ![](images/6720_en-US.png "PV access")

-   **Analyze website operations** 

    Anti-DDoS Pro access logs record information about website traffic in real time. You can use SQL queries to analyze log data to better understand your visitors and analyze website operations. For example, you can identify the most visited Web pages, the browsers that initiated the requests, and the clients, source IP addresses, and ISPs of the requests.

    For example, use the following statement to view the visitor distribution by ISP:

    ``` {#codeblock_yea_kwa_eio}
    __topic__: ddos_access_log | select ip_to_provider(if(real_client_ip='-', remote_addr, real_client_ip)) as provider, round(sum(request_length)/1024.0/1024.0, 3) as mb_in group by provider having ip_to_provider(if(real_client_ip='-', remote_addr, real_client_ip)) <> '' order by mb_in desc limit 10
    ```

    The analysis results are displayed in a dashboard, as shown in the following figure.

    ![](images/6721_en-US.png "Visitor distribution")


