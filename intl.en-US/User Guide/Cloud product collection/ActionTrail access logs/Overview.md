# Overview {#concept_w25_ddy_32b .concept}

At present, ActionTrail is in connection with Log Service, which provides functions of log collection and analysis in real time. The operation log data collected by ActionTrail is delivered to Log Service in real time. Log Service provides rich functions such as real-time query and analysis, and dashboard presentation for this part of logs.

## Benefits {#section_brb_flq_k2b .section}

-    **Simple configuration**: Easily configure to collect real-time logs. For information about configuration steps and log fields, see [Procedure](reseller.en-US/User Guide/Cloud product collection/ActionTrail access logs/Procedure.md).
-    **Real-time analysis**: Relying on Log Service, it provides real-time log analysis, an out-of-the-box report center, and details available for real-time mining with records of operations on important cloud assets.
-    **Real-time alarms**: Supports custom quasi-real-time monitoring and alarming based on specific indicators to ensure timely response to critical business exceptions.
-    **Ecosystem**: Supports dock with other ecosystems such as stream computing, cloud storage, and visualization solutions to further explore data value.
-    **Free quota**: Provides 500 MB free quotas of data import and storage per month. You can expand the storage time for compliance, traceability, and filing. The storage service without time limitation is provided at a low price of 0.0875 USD/GB/month. For information about billing, see [Billing method](../../../../reseller.en-US/Pricing/Billing method.md).

## Application scenarios {#section_kvq_kcs_k2b .section}

-    **Troubleshooting and analysis for abnormal operations** 

    Monitors cloud resource operations under all names in real time and supports real-time troubleshooting and analysis for abnormal operations. Accidental deletion, high-risk operations, and other operations can be traced through logging.

    For example, to view the Elastic Compute Service \(ECS\) release operation log:

    ![](images/6966_en-US.png "View the ECS release operation log")

-    **Distribution and source tracking of important resource operations** 

    You can track and trace the distribution and source of important resource operations by analyzing the log content, and specify and optimize resolution strategies based on the analysis results.

    For example, to view the country distribution of operators who deleted the Relational Database Service \(RDS\):

    ![](images/6967_en-US.png "View the distribution of RDS deletion")

-    **Resource operation distribution view** 

    You can query and analyze the collected ActionTrail operation logs through SQL query statements in real time, and view the distribution and time trends of all resource operations, and other operation and maintenance actions. By doing this, you assist the operation and maintenance personnel to monitor the resource running status in real time. Operation and maintenance reliability indicators are clear at a glance.

    For example, to view trends of failed operations:

    ![](images/6968_en-US.png "Trends of failed operations")

-    **Real-time analysis of operation data** 

    Customize diverse query statements based on operation requirements, customize fast queries and analysis dashboard for different data requirements, and you can also customize real-time data dashboard for data such as resource usage status and user logon status.

    For example, to view the frequency distribution of operators from network operators:

    ![](images/6969_en-US.png "Frequency distribution of operators from network operators")


