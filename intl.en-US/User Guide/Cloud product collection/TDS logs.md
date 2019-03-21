# TDS logs {#concept_s5h_xsn_lfb .concept}

Alibaba Cloud Threat Detection Service \(TDS\) provides a log analysis function to collect, analyze, query, store, and distribute risk and threat data in real time. This frees you from the need to manually collect, query, and analyze data, improving your overall O&M efficiency.

## Features {#section_bqd_ttn_4fb .section}

## Overview {#section_r1c_4tn_lfb .section}

Alibaba Cloud TDS is fully integrated with Log Service and provides TDS log collection and analysis functions, which can help you better understand and more effectively address server security risks and manage your assets on the cloud. TDS is suitable for the following enterprise-level scenarios:

-   Large-scale enterprises and organizations, such as finance companies and government agencies, which require strict storage compliance for hosts, networks, and security logs, among other assets on the cloud
-   Large-scale real-estate, e-commerce, or finance companies, along with government agencies, which posses on-premises security operations centers \(SOCs\) and require centralization collection and management of security and alarm logs
-   Enterprises with advanced technologies, such as companies in IT, gaming, or finance, which require in-depth analysis of logs collected from various cloud assets and automated alarm handling

## Benefits {#section_tvv_wly_32b .section}

-   Quick analysis capabilities: The analysis of security and host logs can be completed in seconds, and analysis of network logs within an hour.
-   Comprehensive support: A total of 14 log types are provided, including network, host, and security logs.
-   Fully integrated: TDS is fully integrated with the open-source streaming and big data system solutions on Alibaba Cloud and is publicly open to our partners.
-   Flexible to various applications: With support for WYSIWYG analysis capabilities, you can customize service views as needed.

## Limits {#section_lwt_czf_j2b .section}

-   TDS-dedicated Logstores cannot store non-TDS data.

    TDS logs are stored in dedicated Logstores. These Logstores cannot store non-TDS data that is written through APIs or SDKs. Dedicated Logstores have no limits on queries, statistics, alarms, and stream consumption.

-   Basic settings, such as the storage period of dedicated Logstrores, cannot be modified.

-   Dedicated Logstores do not incur charges.

    Dedicated Logstores do not incur charges on the condition that the Log Service functions normally.

    **Note:** The TDS log analysis function is unavailable in the case that Log Service charges are overdue. In such s case, you need to first pay your overdue payments before you can gain access to this function.


## Scenarios {#section_udn_twn_lfb .section}

-   Track host and network logs and trace the source of security threats.

    You can retrieve the `__topic__` field in logs and view the time distribution of different types of logs to track host and network logs in real time.

-   View host and network operations in real time to gain insight into security status and trends.

    You can view host and network operations in the Web access center dashboard to assess the security of your assets in a timely manner.

-   Understand security operating efficiency and handle issues and threats in a prompt manner.

    You can view your current security operating efficiency in the vulnerability center dashboard.


