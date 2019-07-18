# Consume logs {#concept_303715 .concept}

Log Service provides the SDK in various languages, such as Java, Python, and Go. You can use the SDK to call Log Service operations and consume logs.

## Use the SDK to consume logs {#section_q6d_weg_zlz .section}

The following example shows how to use the [Java SDK](https://github.com/aliyun/aliyun-log-java-sdk) to consume data in ShardId. For more information, see [SDK Reference](../reseller.en-US/SDK Reference/Basic Descriptions /Overview.md#).

``` {#codeblock_pqo_3m3_cxh}
Client client = new Client(host, accessId, accessKey);

    String cursor = client.GetCursor(project, logStore, shardId, CursorMode.END).GetCursor();
    System.out.println("cursor = " +cursor);
    try {
      while (true) {
        PullLogsRequest request = new PullLogsRequest(project, logStore, shardId, 1000, cursor);
        PullLogsResponse response = client.pullLogs(request);
        System.out.println(response.getCount());
        System.out.println("cursor = " + cursor + " next_cursor = " + response.getNextCursor());
        if (cursor.equals(response.getNextCursor())) {
            break;
                }
        cursor = response.getNextCursor();
        Thread.sleep(200);
      }
    }
    catch(LogException e) {
      System.out.println(e.GetRequestId() + e.GetErrorMessage());
    }
```

## Preview logs in the console {#section_uzx_4o9_uyc .section}

Log preview also consumes logs. You can use a browser to log on to the Log Service console and preview some logs in a Logstore on the dedicated preview page.

1.  Log on to the [Log Service console](https://partners-intl.console.aliyun.com/#/sls), and then click the target project name.
2.  On the **Logstores** page, find the target Logstore and click **Preview** in the Log Consumption column.
3.  On the log preview page, select the shard and the log time range, and then click **Preview**.

    The log preview page displays the log data of the first 10 packets in the specified time range.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13163/15634441315786_en-US.png)


