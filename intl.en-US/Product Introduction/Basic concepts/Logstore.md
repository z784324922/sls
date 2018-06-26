# Logstore {#concept_btb_4qn_vdb .concept}

The Logstore is a unit in Log Service to collect, store, and query the log data.  Each Logstore belongs to a project, and each project can create multiple Logstores. You can create multiple Logstores for a project according to your actual needs. Typically, an independent Logstore is created for each type of logs in an application. For example, you have a game application “big-game”, and three types of logs are on the server: operation\_log, application\_log, and access\_log. You can first create a project named “big-game”, and then create three Logstores under this project for these three types of logs to collect, store, and query logs respectively.

You must specify the Logstore for writing and querying logs. If you want to deliver log data to maxcompute for offline analysis, its data delivery is also based on the logstore as a unit for data synchronization, that is, The log data in the logstore is delivered to a maxcompute table.

Specifically, Logstores provide the following functions:

-   Logstores can collect logs and support writing logs in real time.
-   Logstores can store logs and support using logs in real time.
-   Logstores can create indexes and support querying logs in real time. 
-   Provides data channels delivered to maxcompute

