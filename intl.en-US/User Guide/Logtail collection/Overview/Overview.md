# Overview {#concept_ppd_yx5_vdb .concept}

The Logtail access service is a log collection agent provided by Log Service. You can use Logtail to collect logs from servers such as Alibaba Cloud Elastic Compute Service \(ECS\) instances in real time in the Log Service console.

![](images/2650_en-US.png "Function advantages")

## Benefits {#section_ets_snw_w1b .section}

-   Non-invasive log collection based on log files. You do not have to modify codes of any application, and log collection does not affect the operating logic of your applications.
-   In addition to text log collection, more collection methods are supported, such as binlog, http, and container stdout.
-   Containers are well supported. This service supports data collection in standard containers, swarm clusters, and Kubernetes clusters.
-   Logtail handles exceptions occurred in the log collection process. When problems \(such as the network or Log Service is abnormal, and the user data temporarily exceeds the reserved bandwidth writing limit\) occur, Logtail actively retries and caches data locally to guarantee the data security.
-   Centralized management capability based on Log Service. After installing Logtail, you can configure settings such as the machines from which logs are to be collected and the collection method in Log Service in a centralized way, without logging on to the servers and configuring settings separately. For how to install Logtail, see [Install Logtail in Windows](reseller.en-US/User Guide/Logtail collection/Install/Install Logtail in Windows.md) and [Install Logtail in Linux](reseller.en-US/User Guide/Logtail collection/Install/Install Logtail in Linux.md).
-   Comprehensive self-protection mechanism.  To make sure that the collection agent running on your machine does not significantly affect the performance of your services, the Logtail client strictly protects and limits the usage of CPU, memory, and network resources.

## Processing capabilities and limits {#section_rbv_ydq_pdb .section}

See [Limits](reseller.en-US/User Guide/Logtail collection/Limits.md).

## Procedure {#section_crw_bht_qy .section}

![](images/2652_en-US.png "Configuration process")

Follow these steps to use Logtail to collect logs from servers:

1.  Install Logtail. Install Logtail on the servers from which logs are to be collected. For more information, see [Install Logtail in Windows](reseller.en-US/User Guide/Logtail collection/Install/Install Logtail in Windows.md) and [Install Logtail in Linux](reseller.en-US/User Guide/Logtail collection/Install/Install Logtail in Linux.md)
2.  [Create a machine group with a custom ID as its identifier](reseller.en-US/User Guide/Logtail collection/Machine Group/Create a machine group with a custom ID as its identifier.md). Skip this step if you are about to collect logs from Alibaba Cloud ECS instances
3.  [Create a machine group with an IP address as its identifier](reseller.en-US/User Guide/Logtail collection/Machine Group/Create a machine group with an IP address as its identifier.md). Log Service manages all the servers from which logs are to be collected by using the Logtail client in the form of machine groups. Log Service allows you to define machine groups by using IP addresses or custom identifiers. You can create a machine group as instructed when applying Logtail configurations to machine groups.
4.  Create a Logtail collection configuration and apply it to the machine group. You can collect data such as [Collect text logs](reseller.en-US/User Guide/Logtail collection/Text logs/Collect text logs.md) and [Syslog](reseller.en-US//Syslog.md) by creating a Logtail configuration in the **data import wizard**. Then, you can apply the Logtail configuration to the machine group.

After completing the preceding steps, incremental logs on servers from which logs are to be collected are actively collected and sent to the corresponding Logstore.  Historical logs are not collected. You can query these logs in the console or by using APIs/SDKs.  You can also query the Logtail log collection status in the console, such as check whether the collection is normal or if any error occurs.

For the complete procedure for Logtail access service in the Log Service console, see [Collect text logs](reseller.en-US/User Guide/Logtail collection/Text logs/Collect text logs.md) .

## Container {#section_vgn_vfw_w1b .section}

-   Alibaba Cloud Container Service Swarm cluster: see [Enable Log Service](../../../../../reseller.en-US/User Guide/Logs/Enable Log Service.md).
-   Alibaba Cloud Container Service Kubernetes cluster: see [Kubernetes log collection process](reseller.en-US/User Guide/Logtail collection/Container log collection/Kubernetes log collection process.md).
-   Self-built Kubernetes: see[Self-built Kubernetes installation](reseller.en-US/User Guide/Logtail collection/Container log collection/Kubernetes log collection process.md#section_kdx_bqr_zdb)
-   Other self-built Docker clusters: see[Collect standard Docker logs](reseller.en-US/User Guide/Logtail collection/Container log collection/Collect standard Docker logs.md#)

## Major concepts {#section_h1d_jmw_w1b .section}

-   **Machine group**: A machine group contains one or more machines from which a type of logs is to be collected. By applying a Logtail configuration to a machine group, Log Service collects logs from all the machines in the machine group according to the same Logtail configuration. You can also manage a machine group in the Log Service console, such as creating/deleting a machine group, and adding/removing a machine to/from a machine group. You must note that a single machine group cannot contain a mix of Windows and Linux machines, but may have machines with different versions of Windows Server or different release versions of Linux.
-   **Logtail client**: Logtail is the agent that collects logs and runs on servers from which logs are to be collected.  For how to install Logtail, see  [Install Logtail in Windows](reseller.en-US/User Guide/Logtail collection/Install/Install Logtail in Windows.md) and [Install Logtail in Linux](reseller.en-US/User Guide/Logtail collection/Install/Install Logtail in Linux.md).  After installing Logtail on the server, create a Logtail configuration and then apply it to a machine group.
    -   In **Linux**, Logtail is installed in the `/usr/local/ilogtail` directory  and starts two independent processes \(a collection process and a daemon process\) whose names start with ilogtail. The program running log is `/usr/local/ilogtail/ilogtail.LOG`.
    -   In **Windows**, Logtail is installed in the `C:\Program Files\Alibaba\Logtail` directory \(for 32-bit system\) or the `C:\Program Files (x86)\Alibaba\Logtail` directory \(for 64-bit system\).  Navigate to Windows Administrative Tools \> Services, you can view two Windows services: LogtailWorker and LogtailDaemon. LogtailWorker is used to collect logs and LogtailDaemon works as a daemon. The program running log is `logtail_*.log` in the installation directory.
-   **Logtail configuration**: Logtail configuration is a collection of policies to collect logs by using Logtail. By configuring Logtail parameters such as data source and collection mode, you can customize the log collection policy for all the machines in the machine group. A Logtail configuration is used to collect a type of logs from machines, parse the collected logs, and send them to a specified Logstore of Log Service. You can add a Logtail configuration for each Logstore in the console to enable the Logstore to receive logs collected by using this Logtail configuration.

## Basic functions {#section_bz3_bnw_w1b .section}

The Logtail access service provides the following functions:

-   **Real-time log collection**: Logtail dynamically monitors log files, and reads and parses incremental logs in real time. Generally, a delay of less than three seconds exists between the time when a log is generated and the time when a log is sent to Log Service.

    **Note:** Logtail does not support collection of historical data. Logs with an interval of more than 12 hours between the time when a log is read and the time when a log is generated are discarded.

-   **Automatic log rotation processing**: Many applications rotate log files according to the file size or date. During the rotation process, the original log file is renamed and a new blank log file is created for log writing. For example, the monitored app.LOG is rotated to generateapp.LOG. 1 and app.LOG. 2. You can specify the file to which collected logs are written, for example, app.LOG. Logtail automatically detects the log rotation process and guarantees that no log data is lost during this process.
-   **Multiple collection input sources**: Besides text logs, Logtail supports the input sources such as syslog, HTTP, MySQL, and binlog. For more information, see Data Source in Log Service user guide.
-   **Compatible with open-source collection agent**: Input source of Logtail can be data collected by open-source softwares , such as Logstash and Beats. For more information, see Data Source in Log Service user guide.
-   **Automatic handling of collection exceptions**When data transmission fails because of exceptions such as Log Service errors, network measures, and quota exceeding the limit, Logtail actively retries based on specific scenario. If the retry fails, Logtail writes the data to the local cache and then automatically resends the data later. 
-     **Flexible collection policy configuration**: You can use Logtail configuration to flexibly specify how logs are collected from a server. Specifically, you can select log directories and files, which support exact match or fuzzy match with wildcards, based on actual scenarios. You can customize the extraction method for log collection and the names of extracted fields. Log Service supports extracting logs by using regular expressions.  The log data models of Log Service require that each log must have a precise timestamp. Therefore, Logtail provides custom log time formats, allowing you to extract the required timestamp information from log data of different formats.
-   **Automatic synchronization of collection configuration:** Generally, after you create or update a configuration in the Log Service console, Logtail automatically accepts and brings the configuration into effect within three minutes. No collected data is lost when configuration is being updated.
-   **Automatic upgrade of client:** After you manually install Logtail on a server, Log Service automatically performs the Operation & Maintenance \(O&M\)  and upgrade of Logtail.  No log data is lost when Logtail is being upgraded.
-   **Status monitoring:** To prevent the Logtail client from consuming too many resources and thus affecting your services, the Logtail client monitors its consumption of CPU and memory in real time.  The Logtail client is automatically restarted when its resource usage exceeds the limit to avoid affecting other operations on the machine. The Logtail client actively limits network traffic to avoid excessive bandwidth consumption.
-   **Data transmission with a signature:** To prevent data tampering during the transmission process, the Logtail client obtains your Alibaba Cloud AccessKey \(AK\) and provides a signature to all log data packets to be sent.

    **Note:** To maintain security of your Alibaba Cloud AK, the Logtail client uses the HTTPS tunnel to obtain your AK.


## Data collection reliability {#section_gfl_w5s_cfb .section}

During data collection, Logtail stores the collected checkpoint information to your local directory on a regular basis. If any exception occurs \(such as the server unexpectedly shuts down or a process crashes\), Logtail will collect data from the last recorded checkpoint after being restarted to prevent data loss. Then, Logtail functions according to the [resource limits](reseller.en-US/User Guide/Logtail collection/Install/Configure startup parameters.md) listed in the configuration file. However, if a resource is occupied for five minutes more than the preset time period, Logtail will be forcibly restarted. After the force restart, some of the existing data may be regenerated.

Although Logtail uses multiple methods to guarantee data collection reliability, absolute data integrity cannot be guaranteed. Specifically, data loss may occur due to the following reasons:

-   Logtail is not functioning, but logs have been rotated several times.
-   The log rotation speed is exceedingly high, for example, one time per second.
-   The log collection speed is slower than the log generation speed for a long period of time.

