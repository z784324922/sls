# Use a consumer library to consume logs in high reliability mode {#concept_43841_zh .concept}

Log processing has a wide scope, covering real-time computing, data warehouses, and offline computation. This topic describes how to process logs in order without data loss or repetition in real-time computing scenarios, even when upstream and downstream business systems are unreliable \(for example, have faults\) or the business traffic fluctuates.

For ease of understanding, this topic uses one day in a bank as an example to illustrate how to process logs. This topic also introduces the LogHub features of Log Service, based on which you can use Spark Streaming and Storm spouts to process logs.

## Definitions {#section_vzw_cgl_5fb .section}

## What is log data? {#section_ywl_ggl_5fb .section}

Jay Kreps, a former LinkedIn employee, defines a log as "an append-only, totally-ordered sequence of records ordered by time" in [The Log: What every software engineer should know about real-time data's unifying abstraction](https://engineering.linkedin.com/distributed-systems/log-what-every-software-engineer-should-know-about-real-time-datas-unifying).

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13205/156896552032423_en-US.png)

-   Append only: Log entries are appended to the end of the log. They cannot be modified after they are generated.
-   Totally ordered by time: Log entries are strictly ordered. Each log entry is assigned a unique sequential log entry number to indicate its timestamp. Different log entries may be generated at the same timestamp in seconds. For example, a GET operation and a SET operation are performed at the same timestamp. However, the two operations are still performed in order on a computer.

## What kind of data can be abstracted into logs? {#section_mqn_mxp_5fb .section}

Half a century ago, captains and operators kept logs in thick notebooks. Today, computers enable logs to be generated and consumed everywhere. Servers, routers, sensors, GPS, orders, and various devices reflect our lives from different perspectives. In addition to a timestamp used to record the time of a log, captains kept anything they wanted in logs, such as text, an image, weather conditions, and sailing directions. After half a century, logs are generated in a variety of scenarios, such as an order, a payment record, a user access record, and a database operation.

In the computer field, common logs include metrics, binary logs for relational and NoSQL databases, events, audit logs, and access logs.

In this topic, a user operation in the bank is regarded as a log entry. It contains the name, account, operation time, operation type, and transaction amount of the user.

For example:

``` {#codeblock_m6g_563_kin}
2016-06-28 08:00:00 Zhang San Deposit RMB 1,000
2016-06-27 09:00:00 Li Si Withdrawal RMB 20,000
```

## LogHub data model {#section_lfd_woz_kjz .section}

To help you understand abstract concepts, this section uses the LogHub data model of [Alibaba Cloud Log Service](https://cn.aliyun.com/product/sls) for demonstration. For more information, see [Basic concepts](../../../../reseller.en-US/Product Introduction/Basic concepts/Overview.md) in Log Service Product Introduction.

-   A log consists of time and a group of key-value pairs.
-   A log group is a collection of logs that have the same metadata such as the IP address and source.

The following figure shows the relationships between the logs and log group.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13205/156896552032424_en-US.png)

-   A shard is the basic read/write unit of a log group. It can be regarded as a 48-hour first in, first out \(FIFO\) queue. Each shard allows you to write data at 5 MB/s and read data at 10 MB/s. The logical range of a shard is specified by the BeginKey and EndKey. This range enables the shard to contain a type of data different from other shards.
-   A Logstore stores log data of the same type. Each Logstore is a carrier that consists of one or more shards whose range is \[0000, FFFF..\).
-   A project is a container for Logstores.

The following figure shows the relationships among the logs, log group, shards, Logstores, and project.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13205/156896552032425_en-US.png)

## One day in a bank {#section_e2q_91q_40k .section}

For example, in the nineteenth century, several users in a city went to a bank to deposit or withdraw their money. Several clerks were working in the bank. At that time, no computers were available to synchronize data in real time. Each clerk recorded data in an account book and used the account book to check the money every night in the bank. In this example, users are producers of data, money deposit and withdrawal are user operations, and clerks are consumers of data.

In a distributed log processing system, clerks are standalone servers that have fixed memory and computing capabilities, users are requests from various data sources, and the bank hall is a Logstore where users can read and write data.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13205/156896552132426_en-US.png)

-   Logs or log group: the user operations such as money deposit and withdrawal.
-   User: the producer of operations.
-   Clerk: the employee who handles user requests in the bank.
-   Bank hall \(Logstore\): the place where user requests are received and then assigned to clerks for handling.
-   Shard: the way in which the bank manager sorts user requests in the bank hall.

## Issue 1: Ordering {#section_8t1_xrm_wrq .section}

Two clerks A and B were working in the bank. Zhang San visited the bank and deposited RMB 1,000 at counter A. Clerk A recorded the transaction amount in account book A. In the afternoon, due to a shortage of money, Zhang San went to counter B to withdraw the money. At counter B, clerk B found no deposit record after checking account book B.

Based on this example, money deposit and withdrawal must be strictly ordered. Requests from the same user must be handled by the same clerk to ensure that the status of user operations is consistent.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13205/156896552132427_en-US.png)

To ensure ordering, users can queue up to submit requests. A shard can be created, where only clerk A is assigned to handle user requests based on the FIFO principle. However, this method leads to low efficiency, even when 10 clerks are assigned to handle requests from 1,000 users.

To improve efficiency in this scenario, you can use the following solution:

1.  Create 10 shards for 10 clerks. Assign a clerk to work in each shard.
2.  Ensure that operations for the same account are ordered: Map users by using consistent hashing. For example, map users to specific shards by bank account or user name. In this case, by using the formula hash\(Zhang\) = Z, requests from Zhang San are always mapped to the specific shard whose range contains Z. A clerk, for example, Clerk A, is assigned to handle requests in this shard.

If many users whose surname is Zhang, the solution can be adjusted. For example, use the hash function to map users to shards by account ID or zip code so that user requests can be more evenly distributed to each shard.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13205/156896552132428_en-US.png)

## Issue 2: At least once {#section_01s_cle_y50 .section}

Zhang San deposited money at counter A. When handling this deposit request, clerk A received a call. After the call ended, clerk A incorrectly considered that the deposit request of Zhang San was handled and started to handle the request from the next user. However, the deposit request of Zhang San was lost in this case.

Computers do not make mistakes like clerks and can work more reliably for a longer time. However, computers may still fail to process data due to failure or overload. Deposit loss for such reasons is not allowed.

To avoid data loss in this scenario, you can use the following solution:

Clerk A records the position of the current request in the shard as the progress of the request in a notebook \(not account book A\). In this case, clerk A calls the next user only after the deposit request of Zhang San is handled.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13205/156896552132429_en-US.png)

However, this solution may lead to repetition. For example, after handling the deposit request of Zhang San and updating data in account book A, clerk A was called away but did not record the position of the current request in the shard in the notebook. When clerk A came back and did not find the progress of the request from Zhang San in the notebook, clerk A may handle the request again.

## Issue 3: Exactly once {#section_fy5_o0k_k3f .section}

Will repetition certainly cause problems? The answer is no.

If you perform an idempotent operation more than once, you may waste time and energy. However, such repetition does not affect the result. For example, balance inquiry is a read-only operation performed by a user. The repetition of this operation does not affect the inquiry result. Some non-read-only operations, such as user logoff, can also be performed twice consecutively.

In reality, most operations, such as money deposit and withdrawal, are not idempotent. If you perform these operations repeatedly, the impact on the results can be fatal. What is the solution to repetition? After handling a user request, clerk A needs to update data in account book A, record the position of the current request in the shard in the notebook, and then combine two records into a checkpoint.

If clerk A leaves temporarily or permanently, other clerks can continue as follows: If a checkpoint exists for the current user request, proceed to the next user request. If no checkpoint exists for the current user request, handle this request. Guarantee the atomicity of operations.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13205/156896552132430_en-US.png)

A checkpoint is a persistent object in which you can record the position or time of an element in a shard as the key to indicate that the element is processed.

## Business challenges {#section_dap_u7e_39f .section}

The principles are not complex. However, in the real world, changes and uncertainty make the three issues more complex. For example:

1.  The number of users soars on the pay day.
2.  Unlike computers, clerks need a break and lunch time.
3.  To improve service experience, the bank manager needs to request clerks to work faster at the right time. Can the bank manager determine the right time based on the speed of request processing in a shard?
4.  Clerks need to easily and properly transfer account books and checkpoints during the handover.

## One day in reality {#section_d2l_pun_u0k .section}

## Bank opening at 08:00 {#section_dbl_gag_cf7 .section}

All user requests are assigned to the only shard, shard 0. Clerk A is responsible for handling such requests.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13205/156896552132431_en-US.png)

## Peak hours after 10:00 {#section_ab0_gwt_u89 .section}

The bank manager decides to split shard 0 into shard 1 and shard 2 after 10:00. In addition, the bank manager assigns user requests to the two shards based on the following rules: If the first letter of the surname of a user falls in the range of A to W, the user request is assigned to shard 1. If the first letter of the surname of a user is X, Y, or Z, the user request is assigned to shard 2. The reason why the ranges of the two shards are different is that most surnames start with X, Y, or Z. This mapping method can ensure a balanced workload.

The following figure shows the status of user requests in shards from 10:00 to 12:00.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13205/156896552132432_en-US.png)

When clerk A has difficulty in handling requests in two shards, the bank manager dispatches clerk B and clerk C. Clerk B takes over one of the shards, whereas clerk C is currently idle.

## More and more users at 12:00 {#section_2up_7jy_khj .section}

The bank manager thinks that clerk A is under much pressure for handling requests in shard 1 and splits shard 1 into shard 3 and shard 4. Then, clerk A handles requests in shard 3, and clerk C handles requests in shard 4. After 12:00, the bank manager assigns user requests originally assigned to shard 1 to shard 3 and shard 4.

The following figure shows the status of user requests in shards after 12:00.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13205/156896552132433_en-US.png)

## Fewer and fewer users after 16:00 {#section_nqk_vkz_jou .section}

The bank manager asks clerk A and clerk B to have a break, and arranges for clerk C to handle requests in shard 2, shard 3, and shard 4. Later, the bank manager combines shard 2 and shard 3 to form shard 5, and then combines shard 5 and shard 4 to form shard 6. After all user requests in shard 6 are handled, the bank is closed.

## Actual log processing {#section_k6y_27s_gss .section}

The preceding process can be abstracted into a typical log processing scenario. To meet the business requirements of banks, an auto scaling and flexible log framework can be used to provide the following features:

1.  Automatically scales in or out shards.
2.  Automatically adapts shards to the consumers of a consumer group when consumers are added to or removed from the consumer group. In this process, guarantees data integrity and processes logs in order.
3.  Processes logs only once, which requires cooperation with consumers.
4.  Monitors the consumption progress to properly allocate computing resources.
5.  Supports logs from more sources. For banks, users can send requests from various channels such as online banking, mobile banking, and checks.

You can use LogHub and the LogHub consumer library to process logs in real time in typical scenarios. Using a consumer library, you only need to focus on the business logic and do not need to worry about traffic scaling or failover.

