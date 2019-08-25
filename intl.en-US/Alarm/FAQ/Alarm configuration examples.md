# Alarm configuration examples {#concept_ltq_n1l_wgb .concept}

This topic describes typical examples of alarm configurations.

## Set the alarm notification to contain the error logs for which an alarm is set {#section_tvr_r1l_wgb .section}

**Scenario**: If the number of error logs exceed 5 within five minutes, an alarm is triggered and the alarm notification contains the error logs.

**Configuration solution** 

-   Statements associated with the alarm.
    -   Sequence number 0: indicates `level：ERROR`.
    -   Sequence number 1: indicates `level：ERROR | select COUNT(*) as count`.
-   The condition for triggering the alarm is `$1.count > 5`.
-   The notification content is `${results[0].rawresults}`.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/130326/156669665839406_en-US.png)

