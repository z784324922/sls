# 采集ECS日志 {#concept_tr2_2yr_xdb .concept}

您可以通过Logtail采集ECS日志，本文为您介绍控制台方式的采集流程。

## 配置流程 {#section_jq2_pzr_xdb .section}

1.  在服务器上安装Logtail。
2.  配置Logtail机器组。
3.  创建采集配置并应用到机器组。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13802/3871_zh-CN.png "配置流程")

## 前提条件 {#section_f34_lys_xdb .section}

-   已开通ECS和日志服务。
-   已创建了日志服务Project和Logstore。详细步骤请参考[准备流程](../../../../intl.zh-CN/用户指南/准备工作/准备流程.md)。

    **说明：** 如果您的ECS为经典网络或VPC专有网络，请保证日志服务Project和ECS位于同一地域。

-   如果您的日志服务和ECS不在同一账号名下，则不能自动获取ECS对应的owner信息，需要[非本人ECS（或线下机器）](../../../../intl.zh-CN/用户指南/Logtail 采集/机器组/非本人ECS（或线下机器）.md)。

## 步骤1 安装Logtail {#section_xqh_bxs_xdb .section}

1.  运行安装命令。

    根据ECS所在区域选择Logtail安装脚本。详细内容请参考[Linux](../../../../intl.zh-CN/用户指南/Logtail 采集/安装/Linux.md)和[Windows](../../../../intl.zh-CN/用户指南/Logtail 采集/安装/Windows.md)。

    例如，华东1Linux系统的经典网络用户，请执行以下命令安装Logtail。

    ```
    wget http://logtail-release.oss-cn-hangzhou-internal.aliyuncs.com/linux64/logtail.sh; chmod 755 logtail.sh; sh logtail.sh install cn_hangzhou
    ```

2.  检查Logtail运行状态。

    执行以下命令检查Logtail运行状态，如果回显信息为`ilogtail is running`表示安装成功。

    ```
    /etc/init.d/ilogtaild status
    ```


![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13802/3872_zh-CN.png "安装Logtail")

## 步骤2 配置机器组 {#section_uy3_vxs_xdb .section}

1.  在日志服务控制台首页单击Project名称，进入Logstore列表界面。
2.  单击左侧导航栏中的**Logtail机器组**，进入机器组列表。
3.  单击右上角的**创建机器组**，填写您的ECS内网IP或自定义标识，单击**确认**。

    **说明：** 

    -   只支持当前Project所在地域的云服务器。
    -   同一机器组中不允许同时存在Windows与Linux云服务器。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13802/3874_zh-CN.png "配置机器组")

## 步骤3 创建采集配置 {#section_wt2_vzs_xdb .section}

1.  进入数据接入向导。

    在**Logstore列表**页面，单击**数据接入向导**图标。

2.  选择数据类型。

    在选择数据接入类型步骤中，单击**文本文件**并单击下一步。

3.  数据源设置。

    详细数据源配置请参考[文本日志](../../../../intl.zh-CN/用户指南/Logtail 采集/数据源/文本日志.md)，本文档以**极简模式**为例说明。

    填写ECS日志所在的文件路径，并单击**下一步**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13802/3873_zh-CN.png "极简模式")

4.  应用到机器组。

    勾选您在**步骤2**中创建的机器组，并单击**应用到机器组**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13802/3875_zh-CN.png "应用到机器组")


完成以上步骤后，您已成功配置Logtail方式采集ECS日志。如果您需要为采集到的日志数据建立索引、配置日志投递，请根据页面提示，在后续步骤中完成。

## 查看日志 {#section_e43_vbt_xdb .section}

成功配置后，您可以尝试重新登录ECS，或输入命令`echo "test message" >> /var/log/message`，本地`/var/log/message`文件会有新的日志产生，这部分日志会被Logtail采集到日志服务中。

在Logstore列表单击**查询**或**预览**，可以查看Logtail采集到的日志数据。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13802/3876_zh-CN.png "日志查看方式")

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13802/3877_zh-CN.png "预览日志")

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13802/3878_zh-CN.png "检索日志")

