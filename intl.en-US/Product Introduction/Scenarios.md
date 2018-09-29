# Scenarios {#concept_epl_h4n_vdb .concept}

Typical scenarios of Log Service include data collection, real-time computing, data warehousing and offline analysis, product operation and analysis, and Operation &amp; Maintenance \(O&amp;M\) and management. This document introduces some typical scenarios. For more scenarios, see Best practices. 

## Data collection and consumption {#section_m11_bld_qy .section}

The LogHub function of Log Service enables access to massive real-time log data \(including Metric, Event, BinLog, TextLog, and Click data\) at the lower costs. 

Advantages of the solution:

-   Easy to use: Over 30 real-time data collection methods are provided for you to quickly build your platform. The powerful configuration and management capabilities can ease O&amp;M workload. Nodes are available across China and the rest of the world.
-   Auto scaling: It helps easily cope with traffic peaks and business growth. 

![](images/2369_en-US.png "Data collection and consumption ")

## ETL/Stream Processing  {#section_m4f_jld_qy .section}

LogHub can interconnect with various real-time computing and services, provides complete progress monitoring and alarm notification functions, and supports SDK/API-based custom consumption. 

-   Easy to operate: It provides various SDKs and programming frameworks and can interconnect with various stream computing engines seamlessly. 
-   Comprehensive functions: Rich monitoring data and delay alarm functions are provided.
-   Auto scaling: PB-grade elasticity and zero latency. 

![](images/2370_en-US.png "Data cleaning and Flow Calculation ")

## Data warehouse  {#section_c5b_mld_qy .section}

LogShipper ships LogHub data to storage services and supports various storage formats such as compression, user-defined partitions, row storage, and column storage. 

-   Massive data: No upper limit is configured for the amount of data.
-   Rich storage formats: Various storage formats are supported, such as row storage, column storage, and TextFile. 
-   Flexible configuration: Configurations such as user-defined partitions are supported. 

![](images/2371_en-US.png "Data Warehouse docking ")

## Real-time query and analysis of logs {#section_ybh_f2d_jbb .section}

LogAnalytics supports indexing LogHub data in real time and provides rich query methods such as keywords, fuzzy match, context, range, and SQL aggregation. 

-   Strong real-timeliness: Data can be queried after being written. 
-   Massive amount and low cost: Supports PB/day indexing capabilities, and the cost is 15% of the self-built solution. 
-   Strong analysis capabilities: Supports multiple query methods. Supports SQL aggregation and analysis. Visualization and alarm notification functions are provided. 

![](images/2372_en-US.png "Real-time query and analysis of logs ")

