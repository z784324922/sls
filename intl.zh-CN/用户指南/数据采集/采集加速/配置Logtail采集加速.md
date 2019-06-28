# 配置Logtail采集加速 {#concept_opz_gml_dgb .concept}

开启全球加速功能之后，按照全球加速模式安装的Logtail将自动采用全球加速模式采集日志，开启加速功能前安装的Logtail需要参考本文档手动切换为全球加速模式。

## 前提条件 {#section_mw1_4ml_dgb .section}

1.  [开启HTTP加速](intl.zh-CN/用户指南/数据采集/采集加速/开启全球加速.md#section_sst_dsz_q2b)。
2.  （可选）[开启HTTPS加速](intl.zh-CN/用户指南/数据采集/采集加速/开启全球加速.md#section_ugs_nhm_q2b)。

    若您使用HTTPS访问日志服务，请确认已经开启HTTPS的加速功能，并参考[开启HTTPS加速](intl.zh-CN/用户指南/数据采集/采集加速/开启全球加速.md#section_ugs_nhm_q2b)配置HTTPS加速。

3.  确认加速功能已生效。

    详细步骤请参考[检查加速是否生效](intl.zh-CN/用户指南/数据采集/采集加速/开启全球加速.md#ul_acl_jx1_r2b)。


## 背景信息 {#section_ncb_xml_dgb .section}

配置Logtail采集加速前，请注意：

-   若开启了全球加速功能之后再安装Logtail，请参考[安装Logtail（Linux系统）](intl.zh-CN/用户指南/Logtail采集/安装/安装Logtail（Linux系统）.md)，选择安装模式为**全球加速**。之后您通过Logtail采集日志将自动获得全球加速效果。
-   若开启全球加速功能之前已安装了Logtail，请参考本文档，手动切换Logtail 采集模式为全球加速。

## 切换Logtail采集模式为全球加速 {#section_u53_qml_dgb .section}

1.  停止Logtail。
    -   Linux：以管理员身份执行命令`/etc/init.d/ilogtaild stop`。
    -   Windows：
        1.  打开控制面板中的**管理工具**。
        2.  打开**服务**，找到LogtailWorker。
        3.  右键单击**停止**。
2.  修改Logtail启动配置文件ilogtail\_config.json。

    请参考[启动配置文件（ilogtail\_config.json）](intl.zh-CN/用户指南/Logtail采集/简介/Logtail配置和记录文件.md#section_jh3_dpk_2fb)，将参数data\_server\_list中的endpoint一行改为`log-global.aliyuncs.com`。

3.  启动Logtail。
    -   Linux：以管理员身份执行命令`/etc/init.d/ilogtaild start`。
    -   Windows：
        1.  打开控制面板中的**管理工具**。
        2.  打开**服务**，找到LogtailWorker。
        3.  右键单击**启动**。

