# Java SDK {#reference_cxs_zdm_12b .reference}

## 下载地址 {#section_xlg_r1r_12b .section}

Log Service 的 Java SDK 让 Java 开发人员可以非常方便地使用 Java 程序操作阿里云日志服务。开发者可以直接使用Maven依赖添加SDK，也可以下载包到本地。目前，SDK 支持 J2SE 6.0 及以上版本，单击[此处](https://github.com/aliyun/aliyun-log-java-sdk)下载最新版完整SDK。

## 操作步骤 {#section_ylg_r1r_12b .section}

为快速开始使用 Log Service Java SDK，请按照如下步骤进行。

## 步骤 1 创建阿里云账号 {#section_zlg_r1r_12b .section}

具体方法请参考 [阿里云账号注册流程](https://www.alibabacloud.com/help/zh/doc-detail/50482.htm)。

## 步骤 2 获取阿里云访问密钥 {#section_amg_r1r_12b .section}

为了使用 Log Service Java SDK，您必须申请阿里云的[访问秘钥](../../../../intl.zh-CN/API 参考/访问秘钥.md)。

登录阿里云[秘钥管理页面](https://ak-console.aliyun.com/#/accesskey)。选择一对用于 SDK 的访问密钥对。如果没有，请创建一对新访问密钥，且保证它处于**启用**状态。有关如何创建访问密钥，参见 [准备流程](../../../../intl.zh-CN/用户指南/准备工作/准备流程.md)。

该密钥对会在下面的步骤使用，且需要保管好，不能对外泄露。另外，您可以参考 [配置](intl.zh-CN/SDK 参考/基本介绍/配置.md) 了解更多 SDK 如何使用访问密钥的信息。

## 步骤 3 创建日志服务项目和日志库 {#section_zrq_dbr_12b .section}

在使用日志服务Java SDK之前，请先在控制台上创建好项目（Project）和日志库（Logstore）。

有关如何创建Project和Logstore，参见[准备流程](../../../../intl.zh-CN/用户指南/准备工作/准备流程.md)。

**说明：** 

-   请确保使用同一阿里云账号获取阿里云访问密钥和创建日志项目及日志库。
-   关于日志的项目、日志库等概念请参考 Log [基本概念](../../../../intl.zh-CN/产品简介/基本概念/简介.md#)。
-   Log 的 Project 名称为日志服务全局唯一，而 Logstore 名称在一个 Project 下面唯一。
-   Log 的 Project 一旦创建则无法更改它的所属区域。目前也不支持在不同阿里云 Region 间迁移 Log Project。

## 步骤 4 安装 Java 开发环境 {#section_csq_dbr_12b .section}

目前，Log Java SDK 支持 J2SE 6.0 及以上的 Java 运行环境，您可以从 [Java 官方网站](http://developers.sun.com/downloads/) 下载并按说明安装 Java 开发环境。

## 步骤 5 安装 Log Service Java SDK {#section_dsq_dbr_12b .section}

在安装完 Java 开发环境后，您需要安装 Log Service Java SDK。目前，我们提供两种方式安装日志服务的 Java SDK：

1.  建议使用 [Apache Maven](http://maven.apache.org/) 获取最新版本的 SDK，您可以添加如下配置到您的 Maven 项目。

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

2.  您也可以完整下载 Java SDK 软件包，然后在自己的 Java 项目中直接引用本地软件包。
    1.  从 [这里](https://github.com/aliyun/aliyun-log-java-sdk) 克隆 Java SDK 包（版本会定期更新，如需使用最新版本请使用 Maven）。
    2.  解压完整下载的包到指定的目录即可。Java SDK 是一个软件开发包，不需要额外的安装操作。
    3.  把 SDK 包中的所有 Jar 包（包括依赖的第三方包）添加到您的 Java 工程（具体操作请参照不同的 IDE 文档）。

## 步骤 6 开始一个新的 Java 项目 {#section_ekk_sbj_sy .section}

现在，您可以开始使用 SDK Java SDK。使用任何文本编辑器或者 Java IDE，运行如下示例代码即可与 Log Service 服务端交互并得到相应输出。Java SDK使用上的一些注意事项请参考注意事项章节。

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
        String endpoint = "<log_service_endpoint>"; // 选择与上面步骤创建 project 所属区域匹配的
        // Endpoint
        String accessKeyId = "<your_access_key_id>"; // 使用您的阿里云访问密钥 AccessKeyId
        String accessKeySecret = "<your_access_key_secret>"; // 使用您的阿里云访问密钥
        // AccessKeySecret
        String project = "<project_name>"; // 上面步骤创建的项目名称
        String logstore = "<logstore_name>"; // 上面步骤创建的日志库名称
        // 构建一个客户端实例
        Client client = new Client(endpoint, accessKeyId, accessKeySecret);
        // 列出当前 project 下的所有日志库名称
        int offset = 0;
        int size = 100;
        String logStoreSubName = "";
        ListLogStoresRequest req1 = new ListLogStoresRequest(project, offset, size, logStoreSubName);
        ArrayList<String> logStores = client.ListLogStores(req1).GetLogStores();
        System.out.println("ListLogs:" + logStores.toString() + "\n");
        // 写入日志
        String topic = "";
        String source = "";
        // 连续发送 10 个数据包，每个数据包有 10 条日志
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
             * 发送的时候也可以指定将数据发送至有一个特定的 shard，只要设置 shard 的 hashkey，则数据会写入包含该
             * hashkey 的 range 所对应的 shard，具体 API 参考以下接口： public PutLogsResponse
             * PutLogs( String project, String logStore, String topic,
             * List<LogItem> logItems, String source, String shardHash // 根据
             * hashkey 确定写入 shard，hashkey 可以是 MD5(ip) 或 MD5(id) 等 );
             */
        }
        // 把 0 号 shard 中，最近 1 分钟写入的数据都读取出来。
        int shard_id = 0;
        long curTimeInSec = System.currentTimeMillis() / 1000;
        GetCursorResponse cursorRes = client.GetCursor(project, logstore, shard_id, curTimeInSec - 60);
        String beginCursor = cursorRes.GetCursor();
        cursorRes = client.GetCursor(project, logstore, shard_id, CursorMode.END);
        String endCursor = cursorRes.GetCursor();
        String curCursor = beginCursor;
        while (curCursor.equals(endCursor) == false) {
            int loggroup_count = 2; // 每次读取两个 loggroup
            BatchGetLogResponse logDataRes = client.BatchGetLog(project, logstore, shard_id, loggroup_count, curCursor,
                    endCursor);
            // 读取LogGroup的List
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
        // ！！！重要提示 : 只有打开索引功能，才能调用以下接口 ！！！
        // 等待 1 分钟让日志可查询
        try {
            Thread.sleep(60 * 1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        // 查询日志分布情况
        String query = "<此处为需要查询的关键词，如果查询全部内容设置为空字符串即可>";
        int from = (int) (new Date().getTime() / 1000 - 300);
        int to = (int) (new Date().getTime() / 1000);
        GetHistogramsResponse res3 = null;
        while (true) {
            GetHistogramsRequest req3 = new GetHistogramsRequest(project, logstore, topic, query, from, to);
            res3 = client.GetHistograms(req3);
            if (res3 != null && res3.IsCompleted()) // IsCompleted() 返回
            // true，表示查询结果是准确的，如果返回
            // false，则重复查询
            {
                break;
            }
            Thread.sleep(200);
        }
        System.out.println("Total count of logs is " + res3.GetTotalCount());
        for (Histogram ht : res3.GetHistograms()) {
            System.out.printf("from %d, to %d, count %d.\n", ht.GetFrom(), ht.GetTo(), ht.GetCount());
        }
        // 查询日志数据
        long total_log_lines = res3.GetTotalCount();
        int log_offset = 0;
        int log_line = 10;//log_line 最大值为100，每次获取100行数据。若需要读取更多数据，请使用offset翻页。offset和lines只对关键字查询有效，若使用SQL查询，则无效。在SQL查询中返回更多数据，请使用limit语法。
        while (log_offset <= total_log_lines) {
            GetLogsResponse res4 = null;
            // 对于每个 log offset,一次读取 10 行 log，如果读取失败，最多重复读取 3 次。
            for (int retry_time = 0; retry_time < 3; retry_time++) {
                GetLogsRequest req4 = new GetLogsRequest(project, logstore, from, to, topic, query, log_offset,
                        log_line, false);
                res4 = client.GetLogs(req4);
                if (res4 != null && res4.IsCompleted()) {
                    break;
                }
                Thread.sleep(200);
            }
            System.out.println("Read log count:" + String.valueOf(res4.GetCount()));
            log_offset += log_line;
        }
        //打开分析功能,只有打开分析功能，才能使用SQL 功能。 可以在控制台开通分析功能，也可以使用SDK开启分析功能
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
        //使用分析功能
        GetLogsRequest req4 = new GetLogsRequest(project, logstore, from, to, "", " index0:value | select avg(index1) as v1,sum(index2) as v2, index0 group by index0");
        GetLogsResponse res4 = client.GetLogs(req4);
        if(res4 != null && res4.IsCompleted()){
            for (QueriedLog log : res4.GetLogs()){
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

## 注意事项 {#section_jqj_rrf_jdb .section}

1.  为了提高您的系统的IO效率，请尽量不要直接使用SDK往日志服务中写数据，写数据标准做法参考文章[Producer Library](../../../../intl.zh-CN/用户指南/其他采集方式/SDK采集/Producer Library.md)。
2.  要消费日志服务中的数据，请尽量不要直接使用SDK的拉数据接口，我们提供了一个高级消费库[通过消费组消费日志](../../../../intl.zh-CN/用户指南/实时消费/消费组消费/通过消费组消费日志.md)，该库屏蔽了日志服务的实现细节，并且提供了负载均衡、按序消费等高级功能。

