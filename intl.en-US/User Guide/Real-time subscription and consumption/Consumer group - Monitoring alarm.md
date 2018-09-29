# Consumer group - Monitoring alarm {#task_p3s_c3c_5db .task}

A consumer group is a group of consumers. Each consumer consumes some of the shards in a Logstore.

The data model of shards can be understood as a queue. The newly written data is added to the tail of the queue and each piece of data in the queue corresponds to a write time. The following shows the data model of shards.

![](images/5789_en-US.png "Shard Data Model")

Basic concepts in collaborative consumption latency alarm:

-   **Consumption process**: The process that a consumer reads data from the head of the queue in sequence.
-   **Consumption progress**: The corresponding write time of the data read by a consumer currently.
-   **Consumption lagging duration**: The difference between the current consumption progress and the latest data write time in the queue, which is measured in seconds.

The consumption lagging duration of a ConsumerGroup takes the maximum value among the consumption lagging durations of all contained shards. When it exceeds the preset threshold \(that is, data consumption lags far behind data production\), an alarm is triggered.

## Procedure {#section_hmf_k3c_5db .section}

1.   Log on to the Log Service console. On the Project List page, click the project name. 
2.   On the Logstore List page, click the Monitor icon at the right of the Logstore. 

    ![](images/5790_en-US.png "Click the Delay Time of Consumption chart name.")

3.   The figure shows the length, in seconds, of consumption, for all Java groups under logstore. which is measured in seconds. Click  **Create Alarm Rule** in the upper-right corner to enter the Create Alarm Rule page. 

    ![](images/5791_en-US.png "Create an alarm rule for consumer group spamdetector-report-c.")

4.   The alarm is triggered if the latency within five minutes is greater than or equal to  600 seconds. Configure the Effective Period and Notification Contact, and then save the rule. 

    ![](images/5792_en-US.png "Set an alarm rule")

    Then, an alarm rule is created.  If you have any questions about the configurations of alarm rules, open a ticket.


