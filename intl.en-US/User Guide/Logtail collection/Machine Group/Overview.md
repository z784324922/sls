# Overview {#concept_nqh_ycp_y2b .concept}

Log Service uses machine groups to manage all the servers whose logs are collected by Logtail clients.

A machine group is a virtual group that contains multiple servers. If you want the logs of multiple servers to be collected by Logtail clients with the same configuration, you can add the servers to a machine group and apply the Logtail configuration to the machine group.

You can define a machine group by using either of the following identification types:

-   [IP address](#): Add the IP addresses of all the servers to the machine group. Each server in the group can be identified by using its unique IP address.
-   [Custom ID](#): Customize an ID for the machine group and use this same custom ID for each server of the machine group.

**Note:** 

-   Before adding a server of other cloud vendors or your local IDC, or adding an ECS instance of other accounts to a machine group, you must set an AliUid for the server or instance. For more information, see [Collect logs from non-Alibaba Cloud ECS instances or ECS instances not in your account](reseller.en-US/User Guide/Logtail collection/Machine Group/Collect logs from non-Alibaba Cloud ECS instances or ECS instances not in your account.md).
-   You cannot add Windows servers and Linux servers to the same machine group.

## IP address-based machine group {#section_bcq_vkp_y2b .section}

You can add multiple servers to a machine group by adding their IP addresses to the machine group. Then you can configure the Logtail clients on all the servers at the same time.

-   If you use ECS servers that are not bound to hostnames, and the network types of these ECS servers remain unchanged, you can use their private IP addresses to define the machine group.
-   In other cases, use the server IP address obtained automatically by the Logtail client when you define a machine group. The IP address of each server is recorded in the IP address field of the app\_info.json server file on the server.

    **Note:** app\_info.json is the file that records the internal information of the Logtail client. The internal information includes the server IP addresses obtained by the Logtail client automatically. Manually modifying the IP address field of this file does not change the IP addresses obtained by the Logtail client.


A Logtail client automatically obtains a server IP address by using the following methods:

-   If the IP address of a server has been bound with its host name in the /etc/hosts server file, the Logtail client automatically obtains the IP address.
-   If the IP address of a server has not been bound with its host name, the Logtail automatically obtains the IP address of the first Network Interface \(NI\) on the server.

**Note:** **Whether the Alibaba Cloud intranet is used for data collection does not depend on whether you use a private IP address to define a machine group**. Your server log data can be collected to Log Service through the Alibaba Cloud intranet only when you use an ECS instance of Alibaba Cloud and you have selected **Alibaba Cloud intranet \(Classic Network and VPC\)** when installing a Logtail on the instance.

For more information, see [Create a machine group](reseller.en-US/User Guide/Logtail collection/Machine Group/Create a machine group.md).

## Custom ID-based machine group {#section_wyp_5kp_y2b .section}

In addition to IP addresses, custom IDs can also be used to define machine groups.

We recommend that you use a machine group defined by a custom ID in the following scenarios:

-   In a custom network, for example a VPC, different servers may have the same IP address. In that case, Log Service cannot manage the Logtail clients on the servers. Using a custom ID to define a machine group can eliminate such a problem.
-   Multiple servers in a machine group can use one custom ID to implement machine group auto scaling. If you set the same custom ID for a new server, the Log Service identifies the new server automatically and adds it to the machine group.

Typically, the system consists of multiple modules. Each module can be expanded horizontally. That is, multiple servers can be added to each module. By creating a machine group separately for each module, you can collect logs by module. Therefore, you need to create a custom ID for each module, and set the machine group ID for the servers of each module. For example, a common website consists of an HTTP request processing module, a cache module, a logic processing module, and a storage module. The custom IDs can be set as http\_module for the HTTP request processing module, cache\_module for the cache module, logic\_module for the logic processing module, and store\_module for the storage module.

For more information, see [Configure a user-defined identity for a machine group](reseller.en-US/User Guide/Logtail collection/Machine Group/Configure a user-defined identity for a machine group.md).

