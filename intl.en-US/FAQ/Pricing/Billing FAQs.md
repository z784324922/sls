# Billing FAQs {#concept_cwh_kvn_vdb .concept}

**FAQ list**

1.  [What should I do if my account is in arrears due to Log Service?](#section_ybw_lvn_vdb)
2.  [I only created a project and a LogStore, why is there a bill?](#section_zbw_lvn_vdb)
3.  [How do I disable Log Service?](#section_acw_lvn_vdb)

## 1. What should I do if my account is in arrears due to Log Service? {#section_ghh_svn_vdb .section}

Log Service charges resources after you use them. It outputs bills every day and automatically deducts fees. The bill lists resources you used on the last day. If the overdue bill is not paid off within 24 hours, your Log Service stops automatically. However, you are still charged for the storage space you are using, and the overdue amount increases. We recommend that you pay off the overdue bill within 24 hours to avoid any business loss caused by service stop. You can continue to use Log Service after paid off the arrears.

## 2. I only created projects and Logstores. Why do I have a bill? {#section_adq_svn_vdb .section}

If you have created a project and a Logstore, a shard is created by default to reserve resources.  As indicated by the page appeared when you create a Logstore, Log Service charges a small amount of resource reservation fees for the shard. Based on the current billing policy, the free quota for each shard is 31 days. If you create two shards, they will be charged after 15 days. You can delete the project and Logstore if you no longer need the shard.  If you delete the resources, Log Service sends you the bill of resource usage the next day, and you do not receive the project bill from the third day. 

**For more information about the billing items, see [EN-US\_TP\_13014.md](reseller.en-US/Pricing/Billing method.md).**

## 3. How to disable Log Service? {#section_acw_lvn_vdb .section}

If you no longer need Log Service, you can delete all projects under the account. In this case, Log Service is disabled and you will not be charged from the next day. If you account has been in arrears, pay off the arrears and delete the projects. If no Log Service services or resources exist under your account, you will not receive Log Service bills from the next day.

