# 操作Logstore {#concept_xkb_zh5_vdb .concept}

日志库（Logstore）是创建在项目（Project）下的资源集合，Logstore中的所有数据都来自于同一个数据源。收集到的日志数据的查询、分析、投递均以Logstore为单位。您可以对Logstore进行以下操作：

-   [创建Logstore](#section_v52_2jx_ndb)
-   [修改Logstore配置](#section_evc_rjx_ndb)
-   [删除Logstore](#section_ezq_vjx_ndb)

## 创建Logstore {#section_v52_2jx_ndb .section}

**说明：** 

-   任何一个 Logstore 必须在某一个 Project 下创建。
-   每个日志服务项目可创建最多 10个日志库。
-   Logstore 名称在其所属Project内必须唯一。
-   数据保存时间创建后还可以进行修改。您可以在 Logstore 页面，在**操作**列下单击**操作** \> **修改** ，修改 **数据保存时间** 并单击**修改** ，然后关闭对话框即可。

1.  在 Project列表页面，单击项目的名称，然后单击**创建**创建日志库。

    或者在创建完项目后，根据系统提示创建日志库，单击 **创建**。

2.  填写日志库的配置信息并单击 **确定**。

    |配置项|说明|
    |---|--|
    |Logstore名称|日志库名称须由小写字母、数字、连字符（-）和下划线（\_）组成，且以小写字母或者数字开头和结尾，长度为3-63字节。Logstore 名称在其所属项目内必须唯一。**说明：** 日志库名称创建后不能修改。

|
    |WebTracking|确认是否开启WebTracking功能。WebTracking功能支持从HTML、H5、iOS、或Android平台收集日志数据到日志服务。默认关闭。|
    |数据保存时间|采集到日志服务中的日志在日志库中的保存时间，单位为天。可以设置为1~3650天，即10年。超过该时间后，日志会被删除。|
    |Shard数量|日志库的分区数量，每个Logstore可以创建1~10个分区。每个Project中可以创建最多200个分区。|

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13024/2585_zh-CN.png)


## 修改Logstore配置 {#section_evc_rjx_ndb .section}

创建日志库以后，您还可以在需要的时候修改日志库的配置。

1.  登录日志服务控制台。
2.  选择所需的项目，单击项目名称。
3.  在 Logstore列表 页面，选择所需的日志库并单击操作列下的 **修改**。
4.  在弹出的对话框中修改日志库的配置并关闭对话框。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13024/2586_zh-CN.png)


## 删除Logstore {#section_ezq_vjx_ndb .section}

在某些情况下（如希望废弃某个 Logstore），您可能需要删除指定的 Logstore。日志服务允许您在控制台上删除 Logstore。

**说明：** 

-   一旦 Logstore 删除，其存储的日志数据将会被永久丢失，不可恢复，请谨慎操作。
-   删除指定 Logstore 前必须删除其对应的所有 Logtail 配置。

1.  登录日志服务控制台。
2.  选择所需的项目，单击项目名称。
3.  在 Logstore列表 页面，选择要删除的日志库并单击右侧的 **删除**。
4.  在弹出的确认对话框中，单击 **确定**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13024/2587_zh-CN.png)


