# 操作Project {#concept_mxk_414_vdb .concept}

您可以通过日志服务管理控制台对项目（Project）进行创建和删除操作。

## 创建Project {#section_ahq_ggx_ndb .section}

**说明：** 

-   目前，日志服务仅提供控制台方式创建Project。
-   Project的名字需要全局唯一（在所有阿里云Region内）。如果您选择的Project名称已经被别人使用，您会收到页面提醒信息“**Project名称全局唯一，已被占用，请更换名称重新创建**”，请您更换一个Project名称重试。
-   Project创建时需要指定所在的阿里云Region。请根据需要收集的日志来源和其他实际情况选择合适的阿里云Region。如果您需要收集来自阿里云ECS的日志，建议在ECS相同的Region创建Project。这样可以加快日志收集速度，还可以使用阿里云内网收集日志，不占用ECS虚拟机公网带宽。
-   Project一旦创建完成则无法改变其所属地域，且日志服务不支持Project的迁移，所以请谨慎选择Project的所属Region。
-   一个阿里云账户在所有阿里云Region最多可创建50个Project。

 **操作步骤** 

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。
2.  单击**Project列表**区域中的**创建Project**。
3.  填写**Project名称**、**属性**和**所属地域**，确认是否**开通服务日志**功能，并单击 **确认**。

    |配置项|说明|
    |---|--|
    |Project名称|项目名称只能包含小写字母、数字和连字符（-），且必须以小写字母或数字开头和结尾，长度为3~63个字符。 **说明：** 项目名称创建后不能修改。

 |
    |注释|Project的简单注释，配置后将显示在Project列表页面。创建Project完成后可以在Project列表页面单击编辑来修改注释。 **说明：** 注释内容长度为0~64个字节，不支持字符`<>"'\`。

 |
    |所属地域|您需要为每个项目指定阿里云的一个地域（Region），且创建后就不能修改地域，也不能在多个地域间迁移项目。|
    |开通服务日志|勾选后即开通对应日志采集功能。开通后1-2分钟内生效。     -   详细日志：完整的操作日志。
    -   重要日志：一些重要日志，如报警，计量，Logtail心跳等。
 |
    |日志存储位置|如果选择开启服务日志功能，您需要选择日志的存储位置。可以设置为：     -   自动创建（推荐）。
    -   当前Project。
    -   同一地域下的其他Project。
 |


![新建Project](images/7306_zh-CN.png "创建Project")

## 修改Project {#section_l24_nfj_n2b .section}

如果您需要修改Project的注释信息，可以通过修改Project来操作。

1.  找到需要修改配置的Project。您可以在Project列表的搜索框内输入Project名称进行查询。
2.  单击**操作**列的**编辑**。
3.  在弹出对话框中修改Project配置。

    **说明：** 不支持修改Project名称和所属地域。


![修改Project](images/7305_zh-CN.png "修改Project")

## 删除Project {#section_on2_3gx_ndb .section}

在某些情况下（例如关闭日志服务，销毁Project的所有日志等），您可能需要删除整个Project。

**说明：** 当您的Project被删除后，其管理的所有日志数据及配置信息都会永久释放，不可恢复。所以，在删除Project 前请慎重确认，避免数据丢失。

1.  在Project列表中，选择需要删除的项目。
2.  单击右侧的 **删除**。
3.  在弹出的对话框中，单击选择删除的原因。

    如果选择**其他原因**，请在下方文本框中详细说明。

4.  单击**确认**。

    ![删除Project](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13023/15640196442574_zh-CN.png)


