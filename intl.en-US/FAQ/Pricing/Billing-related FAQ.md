# Billing-related FAQ {#concept_cwh_kvn_vdb .concept}

FAQ list:

1.  [What should I do if I have overdue payment in Log Service?](#)
2.  [I only created projects and Logstores. Why do I have a bill?](#)
3.  [How do I disable Log Service?](#)
4.  [Will any write or read traffic be generated on the Internet if I query and analyze logs in the console?](#)

## 1. What should I do if I have overdue payment in Log Service? {#section_ghh_svn_vdb .section}

Log Service charges resources in pay-as-you-go mode. It generates a bill every day and automatically deducts fees. The bill lists the resources that you used on the last day. If the bill is overdue for more than 24 hours, Log Service automatically stops to provide services for you. However, it still charges you for the storage space that you are using, and the overdue amount increases. We recommend that you pay off the overdue bill within 24 hours to avoid any business loss caused by service interruption. You can continue to use Log Service after paying off the overdue bill.

## 2. I only created projects and Logstores. Why do I have a bill? {#section_adq_svn_vdb .section}

If you have created a project and a Logstore, shards are created by default to reserve resources. As indicated on the page when you create a Logstore, Log Service charges a small amount of resource reservation fees for shards. Based on the current billing policy, you can use a shard free of charge for 31 days. If you create two shards, they are charged after 15 days. You can delete the project and Logstore if you no longer need the shards. If you delete the resources, Log Service sends you the bill of resource usage the next day. You will not receive any project bills from the third day.

## 3. How do I disable Log Service? {#section_acw_lvn_vdb .section}

If you no longer need Log Service, you can delete all the projects under your account. In this case, Log Service is disabled. You will not be charged from the next day. If you have overdue payment, pay off the overdue bill and delete the projects. If no Log Service services or resources exist under your account, you will not receive any Log Service bills from the third day.

## 4. Will any write or read traffic be generated on the Internet if I query and analyze logs in the console? {#section_h4d_xxh_mgb .section}

If you perform any operations in the console, for example, you query and analyze logs, you access Log Service on the intranet. Your intranet access does not generate write or read traffic on the Internet. Therefore, no such traffic is billed.

