# Architecture {#concept_wdz_3nn_vdb .concept}

The Log Service system architecture is as follows.

![](images/2368_en-US.png "Architecture")

## Logtail {#section_ltk_1k2_qy .section}

Logtail is an agent that helps you quickly collect logs  and has the following features:

-   Non-invasive log collection based on log files
    -   Only read files.
    -   Non-invasion during the reading process.
-   Secure and reliable
    -   Supports file rotation, so no loss of data.
    -   Supports local caching.
    -   Provides network exception retry mechanism.
-   Convenient management
    -   Management on Web.
    -   Supports visualization configuration.
-   Comprehensive self-protection
    -   Monitors the CPU and memory consumed by the process in real time.
    -   Restricts the upper limit of memory usage.

## Frontend servers {#section_u1w_3k2_qy .section}

Frontend servers are the frontend machines built with LVS + Nginx  and have the following features:

-   HTTP and REST protocols
-   Horizontal scaling
    -   Supports horizontal scaling when traffic increases.
    -   Frontend servers can be added to quickly improve processing capabilities.
-   High throughput and low latency
    -   Pure asynchronous processing. A single request exception does not affect other requests.
    -   Adopts the Lz4 compression, which is specially for logs, to increase the processing capabilities of individual machines and reduce network bandwidth.

## Backend servers {#section_fwf_pk2_qy .section}

The backend is a distributed process deployed on multiple machines. It provides real-time Logstore data persistence, index, query, and shipping to MaxCompute. The features of the overall backend service are as follows:

-   High data security
    -   Each log you write is saved in triplicate.
    -   Data is automatically replicated and repaired if a disk is damaged or the machine hardware/software has a system error.
-   Stable service
    -   Logstores are automatically migrated if the process is crashed or the machine does not have a response for a long time.
    -   Automatic Server Load Balancer makes sure that traffic is distributed evenly among different machines.
    -   Strict quota limits that prevent abnormal behavior of a single user from affecting other users.
-   Horizontal scaling 
    -   Horizontal scaling is performed by using shards as the unit.
    -   You can dynamically add shards as needed to increase throughput.

