# Java SDK {#reference_cxs_zdm_12b .reference}

## Download address {#section_xlg_r1r_12b .section}

Log Service Java SDK allows Java developers to conveniently use Alibaba Cloud Log Service  by using the Java programs. You can directly use Maven dependencies to add the SDK or download the package to your local machine. Currently, Log Service Java SDK supports J2SE 6.0 or later versions. Click [here](https://github.com/aliyun/aliyun-log-java-sdk) to download the latest SDK.

## Procedure {#section_ylg_r1r_12b .section}

Follow these steps to start using Log Service Java SDK quickly.

## Step 1. Create an Alibaba Cloud account {#section_zlg_r1r_12b .section}

For more information, see[Sign up with Alibaba Cloud](https://www.alibabacloud.com/help/zh/doc-detail/50482.htm) .

## Step 2. Obtain an Alibaba Cloud AccessKey {#section_amg_r1r_12b .section}

Before using Log Service Java SDK, you must apply for an Access Key.

Log on to the Access Key Management page. Select an AccessKey for  for SDK. If you do not have any, create one and make sure the AccessKey is **enabled**. The AccessKey is used in the following steps and must be kept confidential. For more information about how to use the AccessKey in SDK, see [Preparation](../../../../reseller.en-US/User Guide/Preparation/Preparation.md) SDK configuration.

This access key will be used in the following steps. It must be kept confidential. See [Configurations](reseller.en-US/SDK Reference/Basic Descriptions /Configurations.md) for more information about how to use the AccessKey in SDK.

## Step 3. Create a Log Service project and a Logstore {#section_zrq_dbr_12b .section}

Before using Log Service Java SDK, you must create a Log Service project and Logstore in the console.

For how to create a project and Logstore, see [Preparation](../../../../reseller.en-US/User Guide/Preparation/Preparation.md).

**Note:** 

-   Make sure that you use the same Alibaba Cloud account to obtain the Alibaba Cloud AccessKey and create the Log Service project and Logstore.
-   For more information about the concepts of Log Service such as project and Logstore, see Core concept.
-   A project name must be globally unique in Log Service, and a Logstore name must be unique in the same project.
-   After the project is created, you cannot modify the region  or migrate the project across regions.

## Step 4. Install the Java development environment {#section_csq_dbr_12b .section}

Currently, Log Service Java SDK supports the Java runtime environment of J2SE 6.0 or later versions. You can download the installation package at the [Java  official website](http://developers.sun.com/downloads/) and follow the instructions to install the Java development environment.

## Step 5. Install Log Service Java SDK {#section_dsq_dbr_12b .section}

Install the Log Service Java SDK after you build the Java development environment. Currently, two installation methods are available.

1.  We recommend that you use  [Apache Maven](http://maven.apache.org/)  to obtain the latest SDK version. You can add the following configurations to your Maven  project.

    ```
    <dependency>
             <groupId>com.google.protobuf</groupId>
             <artifactId>protobuf-java</artifactId>
             <version>2.5.0</version>
    </dependency>
    <dependency>
    <groupId>com.aliyun.openservices</groupId>
    <artifactId>aliyun-log</artifactId>
    <version>0.6.7</version>
    <exclusions>
            <exclusion>
                <groupId>com.google.protobuf</groupId>
                <artifactId>protobuf-java</artifactId>
           </exclusion>
    </exclusions>
    </dependency>
    ```

2.  You can download the Java SDK package and then directly reference the local package in your Java project.
    1.  Click [here](https://github.com/aliyun/aliyun-log-java-sdk) to clone the Java SDK  package. Version updates are provided periodically. Use Maven to obtain the latest version.
    2.  Extract the downloaded package to a specified directory. The Java SDK does not require installation.
    3.  Add all .jar packages \(including third-party dependent packages\) in the SDK package to your Java project. For detailed instructions, see the corresponding IDE document.

## Step 6. Start a new Java project {#section_ekk_sbj_sy .section}

Now you can start using the Java SDK. To interact with Log Service and obtain the relevant output,  run the following sample code in a text editor or Java IDE. For more information about using Java SDK, see Instructions in this document.

```
package sdksample;
import java.util.ArrayList;
import java.util.List;
import java.util.Vector;
import java.util.Date;
import com.aliyun.openservices.log.Client;
import com.aliyun.openservices.log.common.*;
import com.aliyun.openservices.log.exception.*;
import com.aliyun.openservices.log.request.*;
import com.aliyun.openservices.log.response.*;
import com.aliyun.openservices.log.common.LogGroupData;
import com.aliyun.openservices.log.common.LogItem;
import com.aliyun.openservices.log.common.Logs.Log;
import com.aliyun.openservices.log.common.Logs.Log.Content;
import com.aliyun.openservices.log.common.Logs.LogGroup;
import com.aliyun.openservices.log.common.Consts.CursorMode;
public class sdksample {
    public static void main(String args[]) throws LogException, InterruptedException {
        String endpoint = "<log_service_endpoint>"; // Select the endpoint that matches the region of the project created in the preceding step.
        // Endpoint
        String accessKeyId = "<your_access_key_id>"; // Use your Alibaba Cloud AccessKey ID.
        String accessKeySecret = "<your_access_key_secret>"; // Use your Alibaba Cloud AccessKey Secret.
        // AccessKeySecret
        String project = "<project_name>"; // The name of the project created in the preceding step.
        String logstore = ""<logstore_name>"; // The name of the Logstore created in the preceding steps.
        //Build a client instance.
        Client client = new Client(endpoint, accessKeyId, accessKeySecret);
        //List the names of all LogStores under the current project
        int offset = 0;
        int size = 100;
        String logStoreSubName = "";
        ListLogStoresRequest req1 = new ListLogStoresRequest(project, offset, size, logStoreSubName);
        ArrayList<String> logStores = client.ListLogStores(req1). GetLogStores();
        System.out.println("ListLogs:" + logStores.toString() + "\n");
        //Write logs
        String topic = "";
        String source = "";
        // Send 10 packages consecutively, with each package containing 10 logs
        for (int i = 0; i < 10; i++) {
            Vector<LogItem> logGroup = new Vector<LogItem>();
            for (int j = 0; j < 10; j++) {
                LogItem logItem = new LogItem((int) (new Date().getTime() / 1000));
                logItem.PushBack("index"+String.valueOf(j), String.valueOf(i * 10 + j));
                logGroup.add(logItem);
            }
            PutLogsRequest req2 = new PutLogsRequest(project, logstore, topic, source, logGroup);
            client.PutLogs(req2);
            /*
             * You can specify the shard to which data is sent by setting the shard HashKey. 
              * Data is written to the shard whose range includes the HashKey. For more information about the API, see the following interface: public PutLogsResponse
             * PutLogs( String project, String logStore, String topic,
             * List <logitem> logitems, string source, string shardhash // 
             * Write data to the shard based on the hashkey, which may be MD5(ip) or MD5(id).) throws
             * LogException;
             */
        }
        // Read the data written to Shard 0 during the past minute.
        int shard_id = 0;
        long curTimeInSec = System.currentTimeMillis() / 1000;
        GetCursorResponse cursorRes = client.GetCursor(project, logstore, shard_id, curTimeInSec - 60);
        String beginCursor = cursorRes.GetCursor();
        cursorRes = client.GetCursor(project, logstore, shard_id, CursorMode.END);
        String endCursor = cursorRes.GetCursor();
        String curCursor = beginCursor;
        while (curCursor.equals(endCursor) == false) {
            int loggroup_count = 2; // Read two log groups at a time.
            BatchGetLogResponse logDataRes = client.BatchGetLog(project, logstore, shard_id, loggroup_count, curCursor,
                    endCursor);
            // Read the log group list.
            List<LogGroupData> logGroups = logDataRes.GetLogGroups();
            for(LogGroupData logGroup: logGroups){
                FastLogGroup flg = logGroup.GetFastLogGroup();
                System.out.println(String.format("\tcategory\t:\t%s\n\tsource\t:\t%s\n\ttopic\t:\t%s\n\tmachineUUID\t:\t%s",
                        flg.getCategory(), flg.getSource(), flg.getTopic(), flg.getMachineUUID()));
                System.out.println("Tags");
                for (int tagIdx = 0; tagIdx < flg.getLogTagsCount(); ++tagIdx) {
                    FastLogTag logtag = flg.getLogTags(tagIdx);
                    System.out.println(String.format("\t%s\t:\t%s", logtag.getKey(), logtag.getValue()));
                }
                for (int lIdx = 0; lIdx < flg.getLogsCount(); ++lIdx) {
                    FastLog log = flg.getLogs(lIdx);
                    System.out.println("--------\nLog: " + lIdx + ", time: " + log.getTime() + ", GetContentCount: " + log.getContentsCount());
                    for (int cIdx = 0; cIdx < log.getContentsCount(); ++cIdx) {
                        FastLogContent content = log.getContents(cIdx);
                        System.out.println(content.getKey() + "\t:\t" + content.getValue());
                    }
                }
            }
            String next_cursor = logDataRes.GetNextCursor();
            System.out.println("The Next cursor:" + next_cursor);
            curCursor = next_cursor;
        }
        // ！！！ // Note: You can call the following interface only after the index function is enabled.
        //Wait 1 minute until logs are queryable
        try {
            Thread.sleep(60 * 1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        //Query log distribution
        String query = "<The query keyword. To query all the contents, use an empty string here.>";
        int from = (int) (new Date().getTime() / 1000 - 300);
        int to = (int) (new Date().getTime() / 1000);
        GetHistogramsResponse res3 = null;
        while (true) {
            GetHistogramsRequest req3 = new GetHistogramsRequest(project, logstore, topic, query, from, to);
            res3 = client.GetHistograms(req3);
            if (res3 ! = null && res3. IsCompleted()) // IsCompleted()
            // If IsCompleted() returns "true", the query results are accurate. 
            //If "false" is returned, query the results again.
            {
                break;
            }
            Thread.sleep(200);
        }
        System.out.println("Total count of logs is " + res3. GetTotalCount());
        for (Histogram ht : res3. GetHistograms()) {
            System.out.printf("from %d, to %d, count %d.\n", ht.GetFrom(), ht.GetTo(), ht.GetCount());
        }
        //Query log data
        long total_log_lines = res3. GetTotalCount();
        int log_offset = 0;
        int log_line = 10; //log_line the maximum value is 100 and 100 rows of data are obtained each time. If you want to read more data, use offset to flip the page. Offset and lines are only valid for keyword queries, and if SQL queries are used, they are not valid. To return more data in a SQL query, use the limit syntax.
        while (log_offset <= total_log_lines) {
            GetLogsResponse res4 = null;
            // Read 10 lines of logs at a time for each log offset. If the read operation fails, it is retried three times at most.
            for (int retry_time = 0; retry_time < 3; retry_time++) {
                GetLogsRequest req4 = new GetLogsRequest(project, logstore, from, to, topic, query, log_offset,
                        log_line, false);
                res4 = client.GetLogs(req4);
                if (res4 ! = null && res4. IsCompleted()) {
                    break;
                }
                Thread.sleep(200);
            }
            System.out.println("Read log count:" + String.valueOf(res4. GetCount()));
            log_offset += log_line;
        }
        // Enable the analysis function. You can use the SQL function only after enabling the analysis function. You can enable the analysis function in the console or by using SDKs. // Use the analysis function.
        IndexKeys indexKeys = new IndexKeys();
        ArrayList<String> tokens = new ArrayList<String>();
        tokens.add(",");
        tokens.add(".");
        tokens.add("#");
        IndexKey keyContent = new IndexKey(tokens,false,"text");
        indexKeys.AddKey("index0",keyContent);
        keyContent = new IndexKey(new ArrayList<String>(),false,"long");
        indexKeys.AddKey("index1",keyContent);
        keyContent = new IndexKey(new ArrayList<String>(),false,"double");
        indexKeys.AddKey("index2",keyContent);
        IndexLine indexLine = new IndexLine(new ArrayList<String>(),false);
        Index index = new Index(7,indexKeys,indexLine);
        CreateIndexRequest createIndexRequest = new CreateIndexRequest(project,logstore,index);
        client.CreateIndex(createIndexRequest);
         // Use the analysis function.
        GetLogsRequest req4 = new GetLogsRequest(project, logstore, from, to, "", " index0:value | select avg(index1) as v1,sum(index2) as v2, index0 group by index0");
        GetLogsResponse res4 = client.GetLogs(req4);
        if(res4 ! = null && res4. IsCompleted()){
            for (QueriedLog log : res4. GetLogs()){
                LogItem item = log.GetLogItem();
                for(LogContent content : item.GetLogContents()){
                    System.out.print(content.GetKey()+":"+content.GetValue());
                }
                System.out.println();
            }
        }
    }
}
```

## Precautions {#section_jqj_rrf_jdb .section}

1.  To improve the I/O efficiency of your system, try not to directly use SDKs to write data to Log Service. For more information about the standard way to write data, see [Producer Library](../../../../reseller.en-US/User Guide/Data Collection/SDK collection/Producer Library.md).
2.  To consume data in Log Service, try not to directly use SDKs to pull data interfaces. An advanced consumer library [Consumer group - Usage](../../../../reseller.en-US/User Guide/Real-time subscription and consumption/Consumer group - Usage.md) is provided, which shields the implementation details of Log Service and provides the advanced functions such as load balancing and consumption in order.

