# 采集ECS日志 {#concept_tr2_2yr_xdb .concept}

您可以通过Logtail采集ECS日志，本文为您介绍控制台方式的采集流程。

## 入门教程 {#section_zsl_lws_xdb .section}

采集ECS日志的视频教程请单击[视频教程](http://cloud.video.taobao.com/play/u/2450842572/p/1/e/6/t/1/56329486.mp4)查看。

## 配置流程 {#section_jq2_pzr_xdb .section}

1.  选择日志空间。
2.  创建机器组
3.  配置机器组。
4.  Logtail配置。
5.  查询分析配置。

## 前提条件 {#section_f34_lys_xdb .section}

-   已开通ECS和日志服务。
-   已创建了日志服务Project和Logstore。详细步骤请参考[准备流程](../../../../cn.zh-CN/用户指南/准备工作/准备流程.md)。

    **说明：** 如果您的ECS为经典网络或VPC专有网络，请保证日志服务Project和ECS位于同一地域。

-   如果您的日志服务和ECS不在同一账号名下，则不能自动获取ECS对应的owner信息，需要[为非本账号ECS、自建IDC配置主账号AliUid](../../../../cn.zh-CN/用户指南/Logtail采集/机器组/为非本账号ECS、自建IDC配置主账号AliUid.md)。

## 步骤2 配置机器组 {#section_uy3_vxs_xdb .section}

1.  在日志服务控制台首页单击Project名称，进入日志库概览界面。
2.  单击左侧导航栏中的![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13802/156401940652194_zh-CN.png)图标，切换到机器组列表导航栏。
3.  单击机器组右侧的![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13802/156401940652195_zh-CN.png)图标，选择**创建机器组**，填写您的ECS内网IP或自定义标识，单击**确认**。

    **说明：** 

    -   只支持当前Project所在地域的云服务器。
    -   同一机器组中不允许同时存在Windows与Linux云服务器。

![](images/3874_zh-CN.png "配置机器组")

## 采集步骤 {#section_wt2_vzs_xdb .section}

1.  在日志服务控制台首页单击Project名称。
2.  进入数据接入向导。

    在**概览**页面，单击**接入数据**按钮。

3.  选择数据类型。

    在接入数据页面中单击**单行-文本文件**。

4.  选择日志空间。

    可以选择已有Logstore，也可以新建Project和Logstore。

    **说明：** 如果您的ECS为经典网络或VPC专有网络，请保证日志服务Project和ECS位于同一地域。

5.  **创建机器组** 
    1.  在ECS上安装Logtail客户端

        在**ECS机器**页面勾选实例后单击**安装**。Windows系统请参考[安装Logtail（Windows系统）](../../../../cn.zh-CN/用户指南/Logtail采集/安装/安装Logtail（Windows系统）.md)。

    2.  创建机器组

        如果您之前没有创建过机器组，请在安装完Logtail后单击**确认安装完毕**创建机器组。

6.  机器组配置。

    将**源机器组**中的机器组移动到**应用机器组**中。

7.  Logtail配置。

    详细数据源配置请参考[文本日志](../../../../cn.zh-CN/用户指南/Logtail采集/文本日志/采集文本日志.md)，本文档以**极简模式**为例说明。

    填写ECS日志所在的文件路径，并单击**下一步**。

    ![](images/3873_zh-CN.png "极简模式")

    **说明：** 

    -   Logtail配置推送生效时间最长需要3分钟，请耐心等待。
    -   Logtail收集错误可以参见[诊断采集错误](../../../../cn.zh-CN/常见问题/日志采集/诊断采集错误.md)。
8.  查询分析配置。

    -   **全文索引** 

        您可以选择开启**全文索引**，**中文分词**，并确认是否启用**大小写敏感**、确认**分词符**内容。

    -   **字段索引属性** 

        单击**字段名称**下方的加号新增一行，填写**字段名称**、**类型**、**别名**、**大小写敏感**、**分词符**、选择是否**中文分词**，**开启统计**。

    **说明：** 

    1.  全文索引和字段索引属性必须至少启用一种。同时启用时，以字段索引属性为准。
    2.  索引类型为long/double时，大小写敏感和分词符属性无效。
    3.  如何设置索引请参考[开启并配置索引](../../../../cn.zh-CN/用户指南/查询与分析/开启并配置索引.md)。个别字段为日志服务[保留字段](../../../../cn.zh-CN/产品简介/限制说明/保留字段.md)，请知悉。
    4.  如需使用Nginx模板或消息服务模板，请在查询界面的**查询分析属性**中配置。

完成以上步骤后，您已成功配置Logtail方式采集ECS日志。如果您需要为采集到的日志配置日志投递，请根据页面提示，在后续步骤中完成。

## 查看日志 {#section_e43_vbt_xdb .section}

成功配置后，您可以尝试重新登录ECS，或输入命令`echo "test message" >> /var/log/message`，本地`/var/log/message`文件会有新的日志产生，这部分日志会被Logtail采集到日志服务中。

请返回至概览页面，单击日志库名称后的![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13018/156401940752166_zh-CN.png)图标，选择**查询分析**或**消费预览**，可以查看Logtail采集到的日志数据。

![](images/3876_zh-CN.png "日志查看方式")

![](images/3877_zh-CN.png "预览日志")

![](images/3878_zh-CN.png "检索日志")

