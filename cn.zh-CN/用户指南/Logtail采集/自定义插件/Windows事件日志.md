# Windows事件日志 {#concept_crk_h1k_ggb .concept}

Windows版本Logtail 支持以自定义插件的形式采集 Windows 事件日志。

通过 Windows API，Windows版本Logtail的Windows事件日志插件支持从数据源不断地拉取到符合配置的日志，解析日志并发送到日志服务。此插件可以采集Windows 机器上的任意事件日志，例如，可以通过此插件实现对应用、安全、系统、硬件等事件日志的采集。

## 实现原理 {#section_cyl_n1k_ggb .section}

对于事件日志，Windows 目前提供了 [Windows Event Log](https://docs.microsoft.com/en-us/windows/desktop/wes/windows-event-log) 和 [Event Logging](https://docs.microsoft.com/en-us/windows/desktop/EventLog/event-logging)（以下分别简称为 wineventlog 和 eventlogging）两套 API，前者是后者的升级，仅在 Windows Vista 及以上的版本中提供。此插件会根据所运行的系统，自动地选择相应的 API（优先选择 wineventlog）来获取 Windows 事件日志。

如下图所示，Windows 事件日志在设计中采用发布订阅的模式，应用程序或者内核将事件日志发布到指定的通道（Channel，如图中的 Application、Security、System），Logtail 借助此插件调用 Windows API，实现对这些通道的订阅，从而能够不断地获取相关的事件日志、并发送到日志服务。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/83253/156410466135294_zh-CN.png)

## 注意事项 {#section_qcs_v1k_ggb .section}

-   该功能仅支持 Windows 版本的 Logtail，且版本号为1.0.0.0 及以上。

    如何查看Window版Logtail的版本号，请参考[安装Logtail（Windows系统）](cn.zh-CN/用户指南/Logtail采集/安装/安装Logtail（Windows系统）.md)。

-   可以在配置的 inputs 中填写多项以同时采集多个通道的事件（比如同时采集**应用程序**和**系统**日志）。

## 配置项 {#section_gsq_bbk_ggb .section}

该插件的输入类型为：`service_wineventlog`。

|配置项|类型|是否必选|说明|
|:--|:-|:---|:-|
|Name|string|是|指定所采集事件日志所属的通道名，只能指定一个。默认值为 Application，表示采集**应用程序**的事件日志。|
|IgnoreOlder|uint|否|根据事件时间进行过滤，此配置是相对于采集开始时间偏移量，单位为秒，早于此设置的日志会被忽略。比如： -   设置为 3600 表示相对于采集开始时间一小时前的日志都会被忽略。
-   设置为 14400 表示相对于采集开始时间四小时前的日志都会被忽略。

 默认值为空，表示不根据时间进行过滤。这种情况下会采集到该机器上所有的历史事件日志。 **说明：** 该选项仅在首次配置采集时生效，后续Logtail记录事件采集的Checkpoint，保证事件不会重复采集。

 |
|Level|string|否|根据事件等级进行过滤，仅采集指定等级的日志，可选等级包括：`verbose`、`information`、`warning`、`error`和`critical`。该项可以使用英文逗号（,）来同时指定多个等级，比如： -   设置为`information`表示只采集这一个级别的日志。
-   设置为 `warning, error`表示只采集这两个等级的日志。

 默认值为`information, warning, error, critical`，表示采集除了 verbose 以外其他等级的所有日志。 **说明：** 该参数仅支持 wineventlog API，即只能在 Windows Vista 及以上的操作系统上使用。

 |
|EventID|string|否|根据事件 ID 进行过滤，可以指定正向过滤（单个或范围）或者反向过滤（仅支持单个）。例如： -   `1-200` 表示只采集事件 ID 在 1-200 范围内的日志。
-   `20`表示只采集事件 ID 为 20 的日志。
-   `-100`表示采集除了事件 ID 为 100 以外的所有日志。

 默认值为空，表示采集所有事件。

 该项也可以使用英文逗号（,）来同时指定多个范围，比如设置为`1-200,-100`表示采集 1-200 范围内除了 100 以外的事件日志。 **说明：** 该参数仅支持 wineventlog API，即只能在 Windows Vista 及以上的操作系统上使用。

 |
|Provider|string 数组|否|根据事件来源进行过滤，只采集此参数中指定的来源的事件日志。例如设置为 \["App1", "App2"\] 表示只采集来源名字为 App1 和 App2 的事件日志，其他事件日志都会被忽略。 默认值为空，表示采集所有来源的事件。

 **说明：** 该参数仅支持 wineventlog API，即只能在 Windows Vista 及以上的操作系统上使用。

 |
|IgnoreZeroValue|boolean|否|并非每条事件日志都拥有所有的字段，可以使用此参数来过滤掉那些为空的字段，对空的定义视类型而定，比如整数类型使用 0 表示空。 默认为 false，表示不过滤空字段。

 |

## 查看配置 {#section_nnm_pck_ggb .section}

在 Windows 事件查看器中可以查看上述配置项的当前配置。

1.  单击**开始**菜单（或者按 Win 键）。
2.  搜索并打开**事件查看器**。
3.  在左侧导航栏中展开**Windows日志**目录。

    左侧导航栏中以目录结构呈现了所有的通道，选择通道名称，鼠标右键菜单中单击**属性**，**全名**一栏为该通道的名字，即配置项中的**Name**。

    目录**Windows 日志**下有几个常用通道，全名分别为：

    -   应用程序：Application
    -   安全：Security
    -   Setup：Setup
    -   系统：System
4.  选中指定通道，查看中间的**级别**一栏。

    此处以列表结构呈现了指定通道中的所有事件日志，其中：

    -   级别：对应于配置项中的 `Level`，`信息`即 information。
    -   日期和时间：对应于配置项中的 `IgnoreOlder`，根据时间的过滤即基于此列的值。
    -   来源：对应于配置项中的 `Provider`。
    -   事件 ID：对应于配置项中的 `EventID`。
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/83253/156410466135295_zh-CN.png)


## 日志字段 {#section_std_s5k_ggb .section}

本插件会将采集到的事件日志格式化为以下字段进行输出。

|字段名|字段类型|是否会被过滤|字段含义|
|:--|:---|:-----|:---|
|activity\_id|string|是|表示当前事件所属活动的 GUID，同一个活动的事件会具有相同的活动 GUID。|
|computer\_name|string|否|产生当前事件的节点名。|
|event\_data|JSON object|否|和当前事件相关的数据。|
|event\_id|int|否|当前事件的 ID。|
|kernel\_time|int|是|记录当前事件消耗的内核态时间，一般事件此字段为 0。|
|keywords|JSON array|是|当前事件关联的关键字，用于分类事件。|
|level|string|是|当前事件的等级。|
|log\_name|string|否|获取当前事件的通道名，即配置项中的 Name。|
|message|string|是|当前事件关联的消息。|
|message\_error|string|是|在解析当前事件关联消息时发生的错误信息。|
|opcode|string|是|当前事件关联的操作码。|
|process\_id|int|是|记录当前事件的进程 ID。|
|processor\_id|int|是|记录当前事件对应的处理器 ID，一般事件此字段为 0。|
|processor\_time|int|是|记录当前事件消耗的处理器时间，一般事件此字段为 0。|
|provider\_guid|string|是|当前事件来源的 GUID。|
|record\_number|int|否|当前事件关联的记录编号。一般来说，事件的记录编号会随着每条事件的写入递增，当超过 2 32 （eventlogging）或 2 64 （wineventlog）后会重新从 0 开始。|
|related\_activity\_id|string|是|当前事件所属活动关联的其他活动的 GUID。|
|session\_id|int|是|记录当前事件的会话 ID，一般事件此字段为 0。|
|source\_name|string|否|当前事件的来源，即配置项中的 Provider。|
|task|string|是|当前事件关联的任务。|
|thread\_id|int|是|记录当前事件的线程 ID。|
|type|string|否|获取当前事件使用的 API，wineventlog 或者 eventlogging。|
|user\_data|JSON object|否|当前事件关联的数据。|
|user\_domain|string|是|当前事件关联的用户的域。|
|user\_identifier|string|是|当前事件关联的用户的 Windows 安全标识（Security Identifier，SID）。|
|user\_name|string|是|当前事件关联的用户名。|
|user\_time|int|是|记录当前事件消耗的用户态时间，一般事件此字段为 0。|
|user\_type|string|是|当前事件关联的用户的类型。|
|version|int|是|当前事件的版本号。|
|xml|string|是|当前事件最原始的信息，XML 格式。|

## 采集步骤 {#section_bvn_wxk_ggb .section}

**前提条件**

-   已开通日志服务。
-   已安装Windows Logtail，并且 Logtail 版本在 1.0.0.0 及以上。

    安装Windows Logtail、查看Logtail版本号请参考[查看Logtail版本](cn.zh-CN/用户指南/Logtail采集/安装/安装Logtail（Windows系统）.md#section_dxc_1gf_ggb)。

-   已创建Project 和Logstore。

**操作步骤** 

1.  登录[日志服务控制台](https://sls.console.aliyun.com)，单击Project名称。
2.  单击**接入数据**按钮，并在**接入数据**页面中选择**自定义数据插件**。
3.  选择日志空间。

    可以选择已有的Logstore，也可以新建Project或Logstore。

4.  创建并配置机器组。

    在创建机器组之前，您需要首先确认已经安装了Logtail。 安装完Logtail后单击**确认安装完毕**创建机器组。如果您之前已经创建好机器组 ，请直接单击**使用现有机器组**。

    **说明：** 接收syslog的服务器必须在机器组中，且安装了0.16.13及以上版本的Logtail。

    选择一个机器组，将该机器组从**源机器组**移动到**应用机器组**。

5.  数据源设置。

    请填写**配置名称**和**插件配置**。配置内容（JSON 格式）如下：

    `inputs`部分为采集配置，是必选项；`processors`部分为处理配置，是可选项。采集配置部分需要按照您的数据源配置对应的采集语句，处理配置部分请参考[处理采集数据](cn.zh-CN/用户指南/Logtail采集/自定义插件/处理采集数据.md)配置一种或多种采集方式。

    同时采集**应用程序**和**系统**两个通道的配置如下：

    其中， IgnoreOlder 均设置为了 3 天以避免采集过多的事件日志，您可以打开机器上的事件查看器确认有数据的时间段，适当调整该参数。

    ``` {#codeblock_c6t_n7k_bsh}
    {
        "inputs": [
            {
                "type": "service_wineventlog",
                "detail": {
                    "Name": "Application",
                    "IgnoreOlder": 259200
                }
            },
            {
                "type": "service_wineventlog",
                "detail": {
                    "Name": "System",
                    "IgnoreOlder": 259200
                }
            }
        ]
    }
    ```

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/83253/156410466235296_zh-CN.png)

6.  查询分析配置。

    默认已经设置好索引，您也可以手动进行修改。

    至此，您的事件日志已经被采集到Logstore 中，您可以到Logstore列表页面单击**查询**来查看您的日志。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/83253/156410466235298_zh-CN.png)


