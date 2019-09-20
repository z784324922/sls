# Display query and analysis results on multiple pages {#concept_ghh_2yt_1fb .concept}

Returning too much data for a log query may affect the result displaying speed and query experience. You can use API operations to perform paged queries so that the query result is displayed on multiple pages. This helps limit the volume of data returned each time.

The paged query feature that Log Service provides not only allows you to read raw logs by page but also allows you to write the execution result of SQL statements to local devices by page. Developers can read logs by page through the SDK or Command-Line Interface \(CLI\) provided by Log Service.

## Paging methods {#section_b5z_ggv_1fb .section}

The query and analysis features of Log Service allow you to query logs based on keywords and analyze the query result by running the corresponding SQL statements. The GetLogstoreLogs operation is provided by Log Service to query logs. With this operation, you can query raw logs by keyword, compute data by running SQL statements, and obtain the analysis result. The query feature and the analysis feature implemented in query and analysis statements use different paging methods.

-   **[Query statements](reseller.en-US/Index and query/Query/Query syntax.md)**: Search for records in raw logs by keyword. You can use the offset and lines parameters of the operation to obtain all required records and display them on multiple pages.
-   **[Analysis statements](reseller.en-US/Index and query/Syntax description.md)**: Analyze the query result by running the corresponding SQL statements and obtain the analysis result. You can get the paged result by running the SQL [LIMIT statement](reseller.en-US/Index and query/Analysis grammar/LIMIT syntax.md).

## Display the query result by page {#section_o3d_lhv_1fb .section}

Use the offset and lines parameters of the GetLogStoreLogs operation to get the paged query result.

-   offset: the line from which the logs are to be read.
-   lines: the number of lines to be read for the current request. The maximum value of this parameter is 100. If you set a value greater than 100, only 100 lines are returned.

The value of the offset parameter increases for reading logs by page. When the value reaches a certain number, zero lines are returned and the progress becomes complete. In this case, all data has been read.

## Sample code of querying logs by page {#section_gtw_r4v_1fb .section}

-   **Pseudocode:** 

    ```
        offset = 0// Indicates that logs are read from the zeroth line.
        lines = 100// Indicates that 100 lines are read at a time.
        query = "status:200"// Indicates that the logs where the value of the status field contains 200 are queried. while True:
        response = get_logstore_logs(query, offset, lines) // The response to the read request.
        process (response)                                 // Use the custom logic to process the query result.
        If response.get_count() == 0 && response.is_complete()   
             Stop reading logs and exit the current loop.
        else
            offset += 100                                 // The value of the offset parameter increases by 100. The next 100 lines are to be read.
    ```

-   **Python code**:

    For more information about the sample code, see [Python SDK](../../../../reseller.en-US/SDK Reference/Python SDK.md).

    ```
        endpoint = ''# The endpoint that matches the region of the project created in the preceding step.
        accessKeyId = ''# Your Alibaba Cloud AccessKey ID.
        accessKey = ''# Your Alibaba Cloud AccessKey secret.
        project = ''# The name of the project created in the preceding step.
        logstore = ''# The name of the Logstore created in the preceding step.
        client = LogClient(endpoint, accessKeyId, accessKey)
        topic = ""
        query = "index"
        From = int(time.time()) - 600
        To = int(time.time())
        log_line = 100
        offset = 0
        while True:
            res4 = Nonefor retry_time in range(0, 3):
                req4 = GetLogsRequest(project, logstore, From, To, topic, query, log_line, offset, False)
                res4 = client.get_logs(req4)
                if res4 isnotNoneand res4.is_completed():
                    break
                time.sleep(1)
            offset += 100if res4.is_completed() && res4.get_count() == 0:
                  break;
            if res4 isnotNone:
                res4.log_print()  # Display the execution result.
    ```

-   **Java code:** 

    For more information about the sample code, see [Java SDK](../../../../reseller.en-US/SDK Reference/Java SDK.md).

    ```
            int log_offset = 0;
            int log_line = 100;// Indicates that 100 lines are read at a time. The maximum value of this parameter is 100. If you need to read more than 100 lines at a time, use the offset parameter. Note that the offset and lines parameters are available to keyword-based queries instead of SQL-based queries. To read more than 100 lines at a time during a SQL-based query, use the LIMIT statement.
            while (true) {
                GetLogsResponse res4 = null;
                // For each log offset, 100 lines are read at a time. If a read operation fails, a maximum of 3 retries are allowed.
                for (int retry_time = 0; retry_time < 3; retry_time++) {
                    GetLogsRequest req4 = new GetLogsRequest(project, logstore, from, to, topic, query, log_offset,
                            log_line, false);
                    res4 = client.GetLogs(req4);
    
                    if (res4 ! = null && res4.IsCompleted()) {
                        break;
                    }
                    Thread.sleep(200);
                }
                System.out.println("Read log count:" + String.valueOf(res4.GetCount()));
                log_offset += log_line;
                if (res4.IsCompleted() && res4.GetCount() == 0) {
                            break;
                }
    
            }
    						
    ```


## Display the analysis result by page {#section_srx_m4v_1fb .section}

The offset and lines parameters of the GetLogStoreLogs operation are not available to SQL-based analysis. If you traverse the offset parameter based on the preceding method to display the analysis result by page, the SQL-based analysis result is the same for each execution. If you try to obtain all the computing result at a time, the following issues may occur because of the large-sized data in the result:

-   The latency of transmitting large-sized data is high.
-   The memory usage is high. Storing large-sized data in the analysis result occupies the storage space of the client memory.

To resolve these issues in displaying the SQL-based analysis result by page, Log Service provides the standard SQL [LIMIT statement](reseller.en-US/Index and query/Analysis grammar/LIMIT syntax.md) that uses the following syntax:

```
limit Offset, Line
```

-   Offset: the line from which the result is to be read.
-   Line: the number of lines to be read. The value of Line does not have an upper limit. However, reading too many lines at a time may result in a high network latency and an adverse impact on the processing speed of the client.

Assume that 2,000 lines are returned for the analysis statement `* | selectcount(1) , url group by url`. You can specify that all data is to be read at four intervals, with 500 lines being read at a time.

```
* | selectcount(1) , url  group by url  limit 0, 500
* | selectcount(1) , url  group by url  limit 500, 500
* | selectcount(1) , url  group by url  limit 1000, 500
* | selectcount(1) , url  group by url  limit 1500, 500
```

## Sample code of displaying the SQL-based analysis result by page {#section_lf1_fpv_1fb .section}

-   **Pseudocode**:

    ```
        offset = 0// Indicates that logs are read from the zeroth line.
        lines = 500// Indicates that 500 lines are read at a time.
        query = "* | select count(1) , url  group by url  limit "whileTrue:
        real_query = query + offset + "," +  lines
        response = get_logstore_logs(real_query) // The response to the read request.
         process (response)                       // Use the custom logic to process the analysis result.
        If response.get_count() == 0   
             Stop reading logs and exit the current loop.
        else
            offset += 500                        // The value of the offset parameter increases by 500. The next 500 lines are to be read.
    ```

-   **Python code:** 

    ```
        endpoint = ''# The endpoint that matches the region of the project created in the preceding step.
        accessKeyId = ''# Your Alibaba Cloud AccessKey ID.
        accessKey = ''# Your Alibaba Cloud AccessKey secret.
        project = ''# The name of the project created in the preceding step.
        logstore = ''# The name of the Logstore created in the preceding step.
        client = LogClient(endpoint, accessKeyId, accessKey)
        topic = ""
        origin_query = "* | select count(1) , url  group by url  limit "
        From = int(time.time()) - 600
        To = int(time.time())
        log_line = 100
        offset = 0
        while True:
            res4 = None
            query = origin_query + str(offset) + " , " + str(log_line)
            for retry_time in range(0, 3):
                req4 = GetLogsRequest(project, logstore, From, To, topic, query)
                res4 = client.get_logs(req4)
                if res4 isnotNoneand res4.is_completed():
                    break
                time.sleep(1)
            offset += 100if res4.is_completed() && res4.get_count() == 0:
                  break;
            if res4 isnotNone:
                res4.log_print()  # Display the execution result.
    ```

-   **Java code:** 

    ```
            int log_offset = 0;
            int log_line = 500;
            String origin_query = "* | select count(1) , url  group by url  limit "while (true) {
                GetLogsResponse res4 = null;
                // For each log offset, 500 lines are read at a time. If a read operation fails, a maximum of 3 retries are allowed.
                query = origin_query + log_offset + "," + log_line;
                for (int retry_time = 0; retry_time < 3; retry_time++) {
                    GetLogsRequest req4 = new GetLogsRequest(project, logstore, from, to, topic, query);
                    res4 = client.GetLogs(req4);
    
                    if (res4 ! = null && res4.IsCompleted()) {
                        break;
                    }
                    Thread.sleep(200);
                }
                System.out.println("Read log count:" + String.valueOf(res4.GetCount()));
                log_offset += log_line;
                if (res4.GetCount() == 0) {
                            break;
                }
    
            }
    						
    ```


