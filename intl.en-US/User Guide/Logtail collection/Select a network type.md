# Select a network type {#concept_st2_rwn_y2b .concept}

The collected log data can be sent to Log Service through the **Alibaba Cloud intranet**, the **Internet**, or through **Global Acceleration**.

## Network types {#section_ohn_j55_y2b .section}

-   **Internet**: Sending log data through the Internet can be limited by the network bandwidth. Additionally, network issues such as jitters, latency, and packet loss may affect the speed and stability of data transmission.
-   **Alibaba Cloud intranet**: The Alibaba Cloud intranet supports shared bandwidth at the gigabit-level and can transmit log data more quickly and stably than the Internet. The intranet includes the **Virtual Private Cloud \(VPC\)** environment and the **classic network** environment.
-   **Global Acceleration**: This network service accelerates log collection by using the edge nodes of Alibaba Cloud Content Delivery Network \(CDN\). Compared with the Internet, Global Acceleration provides lower transmission delay and greater stability.

## Select a network type {#section_bnz_cx4_y2b .section}

-   **Intranet**:

    Whether your log data is transmitted through the Alibaba Cloud intranet depends on your server type and if the server and the Log Service Project are in the same region. The Alibaba Cloud intranet can transmit log data in only the following two scenarios:

    -   **The ECS instances of your account and the Log Service Project are in the same region**.
    -   **The ECS instances of other accounts and the Log Service Project are in the same region**.
    Therefore, we recommend that you create a Log Service Project in the region where your ECS instances reside, and collect logs to this Project. Then the log data of the ECS instances is written to Log Service through the **Alibaba Cloud intranet**, without consuming the Internet bandwidth.

    **Note:** When you install a Logtail client on a server, you must select the region in which the Log Service Project resides. Otherwise, the log data cannot be collected.

-   **Global Acceleration**:

    If your servers are located in your self-built IDCs overseas, or your servers are hosted by overseas cloud vendors, using the Internet to transmit data may cause problems such as high latency and unstable transmission. In this case, you can use [Global Acceleration](reseller.en-US/User Guide/Data Collection/Collection acceleration/Overview.md) instead. [Global Acceleration](reseller.en-US/User Guide/Data Collection/Collection acceleration/Overview.md) accelerates log collection by using the edge nodes of Alibaba Cloud CDN. Compared with data transmission through the Internet, Global Acceleration offers a more stable network with minimal transmission delays.

-   **Internet**:

    We recommend that you select the Internet for the following two scenarios:

    -   The server is an ECS instance, but it does not reside in the same region as the Log Service Project.
    -   The server is located in your own IDC or provided by a vendors.

|Server type|Reside in the same region as the Project|Configure an AliUid|Network type|
|:----------|:---------------------------------------|:------------------|:-----------|
|ECS instances under your account|Yes|Not required|Alibaba Cloud intranet|
|No|Not required|Internet or Global Acceleration|
|ECS instances of other accounts|Yes|Required|Alibaba Cloud intranet|
|No|Required|Internet or Global Acceleration|
|Cloud vendor servers or your own IDC servers|-|Required|Internet or Global Acceleration|

**Note:** Log Service cannot obtain owner information of the ECS instances that are under other accounts or servers. Therefore, you need to configure an AliUid for each server after you complete the Logtail client installation. Otherwise, the server heartbeat is abnormal and the server logs cannot be collected. For more information, see [Collect logs from non-Alibaba Cloud ECS instances or ECS instances not in your account](reseller.en-US/User Guide/Logtail collection/Machine Group/Collect logs from non-Alibaba Cloud ECS instances or ECS instances not in your account.md).

## Examples of selecting a network type {#section_edy_bxx_y2b .section}

The following examples describe how to select an appropriate network in several common scenarios.

**Note:** In the Global Acceleration scenario, the speed and reliability of data collection are important factors because the Log Service Project is created in the Hong Kong region but the servers are from the IDCs located worldwide. Therefore, we recommend that you select the Global Acceleration network type in the Hong Kong region when installing a Logtail client in similar scenarios. Compared with the Internet, Global Acceleration transmits log data with higher stability and performance.

|Scenario|Region of the Log Service Project|Server type|Region of the ECS instance|Selected region for installing a Logtail client|Network type|Configure an AliUid|
|:-------|:--------------------------------|:----------|:-------------------------|:----------------------------------------------|:-----------|:------------------|
|ECS and the Project are in the same region.|China East 1 \(Hangzhou\)|ECS of your current account|China East 1 \(Hangzhou\)|China East 1 \(Hangzhou\)|Intranet|Not required|
|ECS and the Project are in different regions.|China East 2 \(Shanghai\)|ECS of your current account|China North 1 \(Beijing\)|China North 1 \(Beijing\)|Internet|Not required|
|Other accounts|China East 2 \(Shanghai\)|ECS belongs to other accounts.|China North 1 \(Beijing\)|China North 1 \(Beijing\)|Internet|Required|
|Server is in the local IDC.|China East 5 \(Shenzhen\)|Self-built IDC|-|China East 5 \(Shenzhen\)|Internet|Required|
|Global Acceleration|Hong Kong|Self-built IDC|-|Hong Kong|Global Acceleration|Required|

![](images/12057_en-US.png "Examples of selecting a network type")

## Update configurations after a classic network is switched to a VPC {#section_efx_swn_y2b .section}

After a Logtail client is installed, you must update the network configurations if your ECS instance is switched from a classic network to a VPC. To do so, follow these steps:

1.  Reboot the Logtail client as the administrator.
    -   **Linux**:

        ```
        sudo /etc/init.d/ilogtaild stop
        sudo /etc/init.d/ilogtaild start
        ```

    -   **Windows**:

        Open **Management Tool** in **Control Panel**, open **Service**, right-click LogtailWorker, and then select **Reboot**.

2.  Update machine group configurations.
    -   **Custom ID**

        If a custom ID is set to define the machine group, you can directly use the VPC network without updating machine group configurations.

    -   **IP address**

        If the ECS instance IP address is used when you define the machine group, you must replace the original IP address with the new IP address obtained by the rebooted Logtail client. That is, the IP address field in the app\_info.json file.

        The file path of app\_info.json:

        -   Linux: /usr/local/ilogtail/app\_info.json
        -   Windows x64: C:\\Program Files \(x86\)\\Alibaba\\Logtail\\app\_info.json
        -   Windows x32: C:\\Program Files\\Alibaba\\Logtail\\app\_info.json

