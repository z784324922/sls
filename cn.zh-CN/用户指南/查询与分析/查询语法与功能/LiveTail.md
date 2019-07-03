# LiveTail {#concept_edp_dt1_mfb .concept}

LiveTail是日志服务在控制台提供了日志数据实时监控的交互功能，帮助您实时监控日志内容、提取关键日志信息。

## 背景信息 {#section_ik5_yt1_mfb .section}

在线上运维的场景中，往往需要对日志队列中进入的数据进行实时监控，从最新的日志数据中提取出关键的信息进而快速地分析出异常原因。在传统的运维方式中，如果需要对日志文件进行实时监控，需要到服务器上对日志文件执行命令`tail -f`，如果实时监控的日志信息不够直观，可以加上`grep`或者`grep -v`进行关键词过滤。日志服务在控制台提供了日志数据实时监控的交互功能LiveTail，针对线上日志进行实时监控分析，减轻运维压力。

## 功能优势 {#section_f3g_zt1_mfb .section}

-   监控日志的实时信息，标记并过滤关键词。
-   结合采集配置，对采集的日志进行索引区分。
-   日志字段做分词处理，以便查询包含分词的上下文日志。
-   根据单条日志信息追踪到对应日志文件进行实时监控，无需连接线上机器。

## 限制说明 {#section_djl_zw1_mfb .section}

-   LiveTail功能仅支持Logtail采集到的日志数据。
-   已成功采集到日志数据后，才能使用LiveTail功能。

## 使用LiveTail实时监控日志 {#section_ejk_c51_mfb .section}

1.  登录[日志服务控制台](https://sls.console.aliyun.com)，单击Project名称。
2.  在**查询分析**列下单击**查询**。
3.  您可以通过以下两种方式使用LiveTail功能。

    -   快捷开启LiveTail。

        1.  在**原始日志**页签中，单击指定原始日志的序号右侧图标![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23704/156214720213747_zh-CN.png)，并选择**LiveTail**。

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23704/156214720213762_zh-CN.png)

        2.  系统为您自动开启LiveTail，并开始计时。

            其中，**来源类型**、**机器名称**和**文件名称**已预设为指定原始日志的信息。

            开启LiveTail后，Logtail采集到的日志数据会实时显示排列在页面中。最新的日志数据始终在页面底部，且滚动条默认在最下方，即显示最新数据。页面最多显示1000条数据，满1000条后页面自动刷新并重新填充日志数据。

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23704/156214720313763_zh-CN.png)

        3.  （可选）在搜索框中输入关键词。

            包含关键词的日志才会显示在监控列表中。筛选包含关键词的日志，便于对特定日志进行实时内容监控。

        4.  在实时监控日志的时候，如果某些日志数据可能存在异常需要分析的时候，单击**停止LiveTail**。

            停止LiveTail后，LiveTail计时结束，不再实时更新日志数据。

            针对监控日志时发现的异常，日志服务提供多种分析方式，详细信息请参考[使用LiveTail分析日志](#)。

    -   自定义设置LiveTail。
        1.  单击LiveTail页签。

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23704/156214720313764_zh-CN.png)

        2.  配置LiveTail。

            |配置|是否必选|说明|
            |:-|:---|:-|
            |**来源类型**|必选|日志的来源，包括：             -   普通日志
            -   Kubernetes
            -   Docker
 |
            |**机器名称**|必选|日志来源服务器的名称。|
            |**文件名称**|必选|日志文件的完整路径及文件名。|
            |**过滤条件**|可选|关键词，设置关键词后，只有包含关键词的日志才会显示在实时监控窗口中。|

        3.  单击**开启LiveTail**。

            开启LiveTail后，Logtail采集到的日志数据会实时显示排列在页面中。最新的日志数据始终在页面底部，且滚动条默认在最下方，即显示最新数据。页面最多显示1000条数据，满1000条后页面自动刷新并重新填充日志数据。

        4.  在实时监控日志的时候，如果某些日志数据可能存在异常需要分析的时候，单击**停止LiveTail**。

            停止LiveTail后，LiveTail计时结束，不再实时更新日志数据。

            针对监控日志时发现的异常，日志服务提供多种分析方式，详细信息请参考[使用LiveTail分析日志](#)。


## 使用LiveTail分析日志 {#section_nt2_3cb_mfb .section}

停止LiveTail之后，实时监控窗口不再更新显示日志内容，可以对监控过程中发现的问题进行进一步分析排查。

-   查看包含指定字段的日志内容。

    所有的字段都进行过分词处理，单击指定异常字段内容，页面自动跳转到原始日志页签中，按照关键词筛选出该字段相关的所有日志内容。另外也可以对包含该关键字的日志进行上下文查询、查看统计图表等方式进行分析。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23704/156214720313765_zh-CN.png)

-   根据日志分布直方图（histogram）缩小查询的时间范围。

    LiveTail开启时，日志分布直方图也在进行同步更新。如果发现某个时段的日志分布有异常，例如日志数量显著增加时，可以单击该时段的绿色矩形缩小查询的时间范围。跳转后的原始日志内的时间轴与LivaTail内选择的点击的时间轴是相关联的，可以查看在这段时间内所有的原始日志内容及详细的时间分布。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23704/156214720313766_zh-CN.png)

-   通过列设置强调关键信息。

    LiveTail页签中，单击日志列表右上角的**列设置**可以将指定字段单独选设置为一列，使该列的数据更加醒目。可以将需要重点关注的数据设为一列，便于查看和判断异常。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23704/156214720313767_zh-CN.png)

-   对日志数据进行快速分析。

    LiveTail页签中，单击日志列表左上角的箭头，可以展开快速分析区域。快速分析的时间区间是LiveTail开启到停止的时间段，分析的功能与原始日志内提供的快速分析相同。详细说明请参考[快速分析](intl.zh-CN/用户指南/查询与分析/查询语法与功能/快速分析.md)。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23704/156214720313768_zh-CN.png)


