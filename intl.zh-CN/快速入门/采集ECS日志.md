# 采集ECS日志 {#task_1478513 .task}

您可以通过Logtail采集ECS日志，本文为您介绍控制台方式的采集流程。

-   已开通ECS和日志服务。
-   已创建了日志服务Project和Logstore。详细步骤请参见[准备流程](../intl.zh-CN/准备工作/准备流程.md)。

    **说明：** 如果您的ECS为经典网络或VPC专有网络，请保证日志服务Project和ECS位于同一地域。

-   如果您的日志服务和ECS不在同一账号名下，则不能自动获取ECS对应的owner信息，需要[配置主账号AliUid](../intl.zh-CN/数据采集/Logtail采集/机器组/配置主账号AliUid.md)。

## 配置流程 {#section_frm_tev_k7b .section}

选择文件类型后进入数据采集配置流程：

![配置流程](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1175224/156568924354375_zh-CN.png)

## 采集步骤 {#section_6dm_3x0_v52 .section}

1.  登录[日志服务控制台](https://sls.console.aliyun.com)，单击Project名称。 
2.  选择数据类型。 在**概览**页面，单击**接入数据**按钮，在接入数据页面中选择**单行-文本文件**。
3.  选择日志空间。 可以选择已有Logstore，也可以新建Project和Logstore。

    **说明：** 如果您的ECS为经典网络或VPC专有网络，请保证日志服务Project和ECS位于同一地域。

4.  创建机器组。 在创建机器组之前，您需要首先确认已经安装了Logtail。

    -   集团内部机器：默认自动安装，如果没有安装，请根据界面提示进行咨询。
    -   ECS机器： 勾选实例后单击**安装**进行一键式安装。Windows系统不支持一键式安装，请参见[安装Logtail（Windows系统）](../intl.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Windows系统）.md)手动安装。
    -   自建机器：请根据界面提示进行安装。或者参见[安装Logtail（Linux系统）](../intl.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Linux系统）.md#)或[安装Logtail（Windows系统）](../intl.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Windows系统）.md#)文档进行安装。
    安装完Logtail后单击**确认安装完毕**创建机器组。如果您之前已经创建好机器组 ，请直接单击**使用现有机器组**。

5.  配置机器组。 将**源机器组**中的机器组移动到**应用机器组**中。
6.  Logtail配置。 详细数据源配置请参见[文本日志](../intl.zh-CN/数据采集/Logtail采集/文本日志/采集文本日志.md)，本文档以**极简模式**为例说明。

    填写ECS日志所在的文件路径，并单击**下一步**。

    ![极简模式](images/3873_zh-CN.png "极简模式")

    **说明：** 

    -   Logtail配置推送生效时间最长需要3分钟，请耐心等待。
    -   Logtail收集错误可以参见[诊断采集错误](../intl.zh-CN/常见问题/日志采集/诊断采集错误.md)。
7.  查询分析配置。 

    -   **全文索引** 

        您可以选择开启**全文索引**，并确认是否启用**大小写敏感**、确认**分词符**内容。

    -   **字段索引属性** 

        单击**字段名称**下方的加号新增一行，填写**字段名称**、**类型**、**别名**、**大小写敏感**、**分词符**、选择是否**开启统计**。

    **说明：** 

    -   全文索引和字段索引属性必须至少启用一种。同时启用时，以字段索引属性为准。
    -   索引类型为long/double时，大小写敏感和分词符属性无效。
    -   如何设置索引请参见[开启并配置索引](../intl.zh-CN/查询与分析/开启并配置索引.md)。个别字段为日志服务[保留字段](../intl.zh-CN/产品简介/限制说明/保留字段.md)，请知悉。
    -   如需使用Nginx模板或消息服务模板，请在查询界面的**查询分析属性**中配置。

完成以上步骤后，您已成功配置Logtail方式采集ECS日志。如果您需要为采集到的日志配置日志投递，请根据页面提示，在后续步骤中完成。

## 查看日志 {#section_mo8_tb7_we5 .section}

成功配置后，您可以尝试重新登录ECS，或输入命令`echo "test message" >> /var/log/message`，本地`/var/log/message`文件会有新的日志产生，这部分日志会被Logtail采集到日志服务中。

请返回至概览页面，单击日志库名称后的![管理图标](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13018/156568924352166_zh-CN.png)图标，选择**查询分析**或**消费预览**，可以查看Logtail采集到的日志数据。

![预览日志](images/3876_zh-CN.png "日志查看方式")

![消费预览](images/3877_zh-CN.png "预览日志")

![检索日志](images/3878_zh-CN.png "检索日志")

