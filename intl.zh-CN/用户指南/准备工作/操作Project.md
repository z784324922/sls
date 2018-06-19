# 操作Project {#concept_mxk_414_vdb .concept}

您可以通过日志服务管理控制台对项目（Project）进行创建和删除操作。

## 创建Project {#section_ahq_ggx_ndb .section}

**说明：** 

-   目前，日志服务仅提供控制台方式创建Project。
-   Project的名字需要全局唯一（在所有阿里云Region内）。如果您选择的Project名称已经被别人使用，您会收到页面提醒信息“**Project XXX already exists**”，请您更换一个Project名称重试。
-   Project创建时需要指定所在的阿里云Region。您需要根据需要收集的日志来源和其他实际情况选择合适的阿里云Region。如果您需要收集来自阿里云ECS虚拟机的日志，建议在ECS虚拟机相同的Region 创建Project。这样既可以加快日志收集速度，还可以使用阿里云内网（不占用 ECS 虚拟机公网带宽）收集日志。
-   Project一旦创建完成则无法改变其所属地域，且日志服务目前也不支持Project的迁移，所以请谨慎选择Project的所属Region。
-   一个用户在所有阿里云Region总计最多可创建10个Project。

**操作步骤**

1.  登录日志服务管理控制台。
2.  单击右上角的 **创建Project**。
3.  填写 **Project名称** 和 **所属地域**，单击 **确认**。

    |配置项|说明|
    |---|--|
    |Project名称|项目名称只能包含小写字母、数字和连字符（-），且必须以小写字母和数字开头和结尾，长度不能超过 3~63 个字节。**说明：** 项目名称创建后不能修改。

|
    |注释|Project的简单注释，配置后将显示在Project列表页面。创建Project完成后可以在Project列表页面单击修改注释修改。|
    |所属地域|您需要为每个项目指定阿里云的一个地域（Region），且创建后就不能修改地域，也不能在多个地域间迁移项目。|


## 删除Project {#section_on2_3gx_ndb .section}

在某些情况下（例如关闭日志服务，销毁Project 的所有日志等），您可能需要删除整个Project。日志服务允许您在控制台上方便地删除整个Project。

**说明：** 当您的Project被删除后，其管理的所有日志数据及配置信息都会永久释放，不可恢复。所以，在删除Project 前请仔细确认，避免数据丢失。

1.  在Project列表中，选择需要删除的项目。
2.  单击右侧的 **删除**。

