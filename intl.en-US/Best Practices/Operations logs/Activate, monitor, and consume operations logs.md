# Activate, monitor, and consume operations logs {#concept_183888 .concept}

Log Service provides the operations log feature to help you understand the usage of Log Service in real time and improve O&M efficiency.

## Enable the operations log feature {#section_12x_dxs_4on .section}

Operations logs are divided into detailed logs and important logs \(including Logtail-related logs, consumer group latency logs, and metering logs\), respectively stored in internal-operation\_log and internal-diagnostic\_log. The internal-diagnostic\_log Logstore is not charged, while the internal-operation\_log Logstore is charged as common Logstores. Detailed logs record each operation or API request of a user. Multiple operations logs are generated when there are multiple read and write requests.You can enable the operations log feature as required.We recommend that you select Automatic creation \(recommended\) for Log Storage. In this way, the operations logs in the same region can be stored in the same project, thus facilitating log management and statistics.

## Monitor the Logtail heartbeat {#section_0yr_hmp_lzm .section}

After Logtail is installed, you can use Logtail [status logs](../../../../reseller.en-US/Monitor Log Servic/Service log/Log types.md#section_wdh_xtt_ngb) in the operations logs to check the working status of Logtail.

Logtail status logs are stored in the internal-diagnostic\_log Logstore. On the internal-diagnostic\_log page, run the query statement `__topic__: logtail_status` to search for Logtail status logs. You can obtain the number of machines for a recent period of time and compare it with the number of machines in the Logtail application machine group. In addition, you can configure the alert rules. For example, an alert is triggered when the counted number is smaller than the number of machines in the machine group.

-   Query statement

    ``` {#codeblock_w4w_4ih_nug}
    __topic__: logtail_status | SELECT COUNT(DISTINCT ip) as ip_count
    ```

-   Query snapshot

    ![Query and analysis](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/158200/156895120444459_en-US.png)

-   Alert rule configuration \(assume that the number of machines in the machine group is 100\)

    ![Create an alert](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/158200/156895120744460_en-US.png)


If an alert is triggered, you can go to the console to view the status of the machine group and check the heartbeat information of the machines.

## View metering data {#section_q5b_1rk_g3d .section}

After you write logs to Log Service, you can view the metering information such as the log traffic, read and write times, and storage space to learn about the usage and billing item details of Log Service.

A Logstore generates a metering log per hour, including the read and write traffic and times in the statistical time period, and the storage size for raw logs and indexes at the current time point. For more information about the fields of metering logs, see [Log types](../../../../reseller.en-US/Monitor Log Servic/Service log/Log types.md#section_kgc_wpr_m2b). Metering logs of Log Service are stored in the internal-diagnostic\_log Logstore. On the internal-diagnostic\_log page, run the query statement `` \_\_topic\_\_: metering to search for metering logs.

Use the max\_by function to compute the storage size of each Logstore.

-   Query statement

    ``` {#codeblock_vts_he3_crh}
    __topic__: metering | SELECT max_by(storage_index+storage_raw, __time__) as total_storage, project, logstore GROUP BY project, logstore
    ```


The default dashboard of operations logs provides abundant charts for metering logs. For more information, see [Service log dashboards](../../../../reseller.en-US/Monitor Log Servic/Service log/Service log dashboards.md#section_ohd_jwt_ngb).

## View the consumer group latency {#section_1h4_zdi_cqh .section}

After logs are written to Log Service, you can consume logs in addition to querying and analyzing them. Log Service provides [consumer groups](../../../../reseller.en-US/Real-time subscription and consumption/Consumption by consumer groups/Use a consumer group to consume logs.md#) supported by multiple programming languages.

When you use consumer groups to consume logs, the latency for consuming logs is one of the most concerning problems. By monitoring the consumption latency, you can learn about the consumption progress. In case of a high latency, you can adjust the consumption speed by changing the number of consumers.

As a kind of operations logs, consumer group latency logs are also stored in the internal-diagnostic\_log Logstore, and are generated every 2 minutes. On the internal-diagnostic\_log page, run the query statement `__topic__: consumergroup_log` to search for all consumer group latency logs.

Query the consumption latency of the consumer group test-consumer-group.

## Monitor Logtail exceptions {#section_bu3_2mh_qbz .section}

The proper running of Logtail guarantees the log integrity. If Logtail exceptions can be found in time, you can adjust the Logtail configurations to avoid log missing.

You can run the query statement `__topic__: logtail_alarm` to search for the exception logs of Logtail.

Query the number of exceptions of each type within 15 minutes.

-   Query statement

    ``` {#codeblock_tw7_zmy_hkl}
    __topic__: logtail_alarm | select sum(alarm_count)as errorCount, alarm_type  GROUP BY alarm_type
    ```


## Monitor the data traffic written to a Logstore {#section_z2x_hfj_p1r .section}

You can use metering logs to obtain information about the read and write traffic within 1 hour. However, if you need to obtain the information within a narrower time period, such as 15 minutes, the operations logs are required. Each operation of a user corresponds to a request. Each request generates an operations log. For example, you can query the total traffic for write operations within 15 minutes.

1.  Query the traffic of raw logs and compressed logs written to a Logstore within 15 minutes.
    -   Query statement

        ``` {#codeblock_li9_8mr_cxd}
        Method: PostLogStoreLogs AND Project: my-project and LogStore: my-logstore | SELECT sum(InFlow) as raw_bytes, sum(NetInflow) as network_bytes
        ```

2.  Query the traffic decline ratio of the logs written to a Logstore within 15 minutes.
    -   Query statement

        ``` {#codeblock_fhh_2ue_uyb}
        Method: PostLogStoreLogs AND Project: my-project and LogStore: my-logstore | select round((diff[1]-diff[2])/diff[1],2) as rate from (select compare(network_bytes, 900) as diff from (select sum(NetInflow) as network_bytes from log))
        ```

3.  Create an alert.

    Set the alert rule so that an alert is triggered when the traffic decline ratio of the logs written to a Logstore exceeds 50%.

    ![Create an alert](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/158200/156895120844524_en-US.png)


## Audit operations logs {#section_kl7_sys_3xc .section}

The operations logs of all resources in the project are stored in the internal-operation\_log Logstore. The logs record the operation information, for example, the name of a machine group is recorded when you create the machine group, and the name of a Logstore is recorded when you operate the Logstore. In addition, the logs also record the information about the user who performs the operations. Currently, the user information includes three types, as listed in the following table.

|Type|Field|
|----|-----|
|Alibaba Cloud account| -   **InvokerUid**: the AliUid of the Alibaba Cloud account.
-   **CallerType**: the parent account.

 |
|RAM user| -   **InvokerUid**: the AliUid of the RAM user.
-   **CallerType**: the subuser account.

 |
|STS| -   **InvokerUid**: the AliUid of the Alibaba Cloud account.
-   **CallerType**: STS.
-   **RoleSessionName**: the name of the session.

 |

Based on the preceding table, you can obtain the user information corresponding to an operations log.

