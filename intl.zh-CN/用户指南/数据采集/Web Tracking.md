# Web Tracking {#concept_ixm_xl5_vdb .concept}

日志服务支持通过Web Tracking功能进行HTML、H5、iOS和 Android平台日志数据的采集，支持自定义维度和指标。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13028/2604_zh-CN.png)

如上图所示，使用Web Tracking功能可以采集各种浏览器以及iOS、Android APP的用户信息（除[iOS/Android SDK](../../../../intl.zh-CN/SDK 参考/基本介绍/概述.md)外\)，例如：

-   用户使用的浏览器、操作系统、分辨率等。
-   用户浏览行为记录，比如用户网站上的点击行为、购买行为等。
-   用户在APP中停留时间、是否活跃等。

**说明：** 使用Web Tracking意味着该Logstore打开互联网匿名写入的权限，可能会产生脏数据，请留意。

## 配置步骤 {#section_kt1_2m5_vdb .section}

## 步骤1 开通Web Tracking {#section_ofw_2m5_vdb .section}

您可以通过控制台或SDK方式开通Web Tracking。

-   **通过控制台开通Web Tracking**
    1.  在Logstore列表页面，选中需要开通Web Tracking功能的Logstore，单击右侧的**修改**。
    2.  打开 Web Tracking 开关。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13028/2605_zh-CN.png)

-   **通过 Java SDK 开通Web Tracking**

    使用 [JAVA SDK](../../../../intl.zh-CN/SDK 参考/基本介绍/概述.md)：

    ```
    import com.aliyun.openservices.log.Client;
    import com.aliyun.openservices.log.common.LogStore;
    import com.aliyun.openservices.log.exception.LogException;
    public class WebTracking {
      static private String accessId = "your accesskey id";
      static private String accessKey = "your accesskey";
      static private String project = "your project";
      static private String host = "log service data address";
      static private String logStore = "your logstore";
      static private Client client = new Client(host, accessId, accessKey);
      public static void main(String[] args) {
          try {
              //在已经创建的Logstore 上开通Web Tracking功能。
              LogStore logSt = client.GetLogStore(project, logStore).GetLogStore();
              client.UpdateLogStore(project, new LogStore(logStore, logSt.GetTtl(), logSt.GetShardCount(), true));
              //关闭Web Tracking功能。
              //client.UpdateLogStore(project, new LogStore(logStore, logSt.GetTtl(), logSt.GetShardCount(), false));
              //新建支持Web Tracking功能的Logstore。
              //client.UpdateLogStore(project, new LogStore(logStore, 1, 1, true));
          }
          catch (LogException e){
              e.printStackTrace();
          }
      }
    }
    ```


## 步骤2 收集日志数据 {#section_ktl_vm5_vdb .section}

Logstore开通Web Tracking功能后，可以使用以下三种方法上传数据到Logstore中。

-   **使用HTTP GET请求**

    ```
    curl --request GET 'http://${project}.${host}/logstores/${logstore}/track?APIVersion=0.6.0&key1=val1&key2=val2'
    ```

    其中各个参数的含义如下：

    |字段|含义|
    |:-|:-|
    |`${project}`|您在日志服务中开通的Project名称。|
    |`${host}`|您日志服务所在地区的域名。|
    |`${logstore}`|`${project}` 下面开通Web Tracking功能的某一个Logstore的名称。|
    |`APIVersion=0.6.0`|保留字段，必选。|
    |`__topic__=yourtopic`|指定日志的topic，保留字段，可选|
    |`key1=val1`、`key2=val2`|您要上传到日志服务的Key-Value对，可以有多个，但是要保证URL的长度小于16KB。|

-   **使用 HTML img 标签**

    ```
    <img src='http://${project}.${host}/logstores/${logstore}/track.gif?APIVersion=0.6.0&key1=val1&key2=val2'/>
    <img src='http://${project}.${host}/logstores/${logstore}/track_ua.gif?APIVersion=0.6.0&key1=val1&key2=val2'/>
    ```

    各个参数的含义同上，track\_ua.gif除了将自定义的参数上传外，在服务端还会将http头中的UserAgent、referer也作为日志中的字段。

-   **使用 js SDK**
    1.  将 loghub-tracking.js 复制到 web 目录，并在页面中引入如下脚本：

        [单击下载](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/31752/cn_zh/1462870126706/loghub-tracking.js?spm=5176.doc31752.2.3.SOoim2&file=loghub-tracking.js)

        ```
        <script type="text/javascript" src="loghub-tracking.js" async></script>
        ```

        **说明：** 为了不阻塞页面加载，脚本会异步发送 HTTP 请求，如果页面加载过程中需要多次发送数据，后面的请求会覆盖前面的 HTTP 请求，看到的现象是浏览器中会显示 Web Tracking 请求退出。使用同步发送可以避免该问题，同步发送请在脚本中执行如下语句替换：

        原始语句：

        ```
        this.httpRequest_.open("GET", url, true)
        ```

        替换最后一个参数变成同步发送：

        ```
        this.httpRequest_.open("GET", url, false)
        ```

    2.  创建 Tracker 对象。

        ```
        var logger = new window.Tracker('${host}','${project}','${logstore}');
        logger.push('customer', 'zhangsan');
        logger.push('product', 'iphone 6s');
        logger.push('price', 5500);
        logger.logger();
        logger.push('customer', 'lisi');
        logger.push('product', 'ipod');
        logger.push('price', 3000);
        logger.logger();
        ```

        其中各个参数的含义如下：

        |字段|含义|
        |--|--|
        |`${host}`|您日志服务所在Region的endpoint。|
        |`${project}`|您在日志服务中开通的Project名称。|
        |`${logstore}`|`${project}`中的Logstore的名称。|

        执行以上命令后，可以在日志服务看到如下两条日志：

        ```
        customer:zhangsan
        product:iphone 6s
        price:5500
        ```

        ```
        customer:lisi
        product:ipod
        price:3000
        ```


数据上传到日志服务之后，可以使用日志服务[查询分析功能](intl.zh-CN/用户指南/索引与查询/简介.md)实时检索、分析日志数据，并通过多样的可视化方案展示实时分析结果。也可以使用日志服务提供的 [Loghub client library](intl.zh-CN/用户指南/实时订阅与消费/消费组消费.md) 消费数据。

