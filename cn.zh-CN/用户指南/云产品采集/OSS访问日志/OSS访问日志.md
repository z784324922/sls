# OSS访问日志 {#concept_v4b_w3q_zdb .concept}

日志服务支持采集OSS访问日志，并对采集到的OSS访问日志进行实时查询与分析统计、通过多种可视化方式进行分析结果的直观展示。

日志服务对OSS访问日志的专业日志采集分析手段，可以简化您的操作审计和事件回溯，让您的工作更有效率。详细日志字段说明请查看[日志字段](cn.zh-CN/用户指南/云产品采集/OSS访问日志/日志字段.md)。

## 前提条件 {#section_wp1_h5s_zdb .section}

1.  已开通日志服务。
2.  已开通OSS服务，并成功创建了Bucket。
3.  日志服务Project需要和OSS Bucket在同一个阿里云账号的同一Region下。

## 配置步骤 {#section_kt5_l5s_zdb .section}

## 1 日志采集授权 {#section_kmp_m5s_zdb .section}

单击[快捷授权](https://ram.console.aliyun.com/?spm=a2c4g.11186623.2.5.jntiYI#/role/authorize?request=%7B%22Requests%22%3A%20%7B%22request1%22%3A%20%7B%22RoleName%22%3A%20%22AliyunLogArchiveRole%22%2C%20%22TemplateId%22%3A%20%22Archive%22%7D%7D%2C%20%22ReturnUrl%22%3A%20%22https%3A//sls.console.aliyun.com/%22%2C%20%22Service%22%3A%20%22Log%22%7D)为日志服务授权，授权后日志服务有权限将OSS访问日志分发到您的Logstore中。

若您之前没有授权，也可以在OSS控制台首页单击**日志分析** \> **日志采集授权**，根据提示完成授权。

## 2 关联Bucket {#section_vdd_n5s_zdb .section}

1.  在OSS控制台首页单击**日志分析** \> **管理日志服务**，进入日志分析页面。
2.  单击**新建关联**，进入新建关联步骤。

    ![](images/5431_zh-CN.png "新建关联")

    1.  **选择/新建项目**。

        选择日志服务Project所属的区域、日志服务项目名称，并单击**下一步**。

        **说明：** 

        -   **您的日志服务Project与OSS Bucket须处于同一区域，多个Bucket日志可以采集至同一个Logstore中**。
        -   如您之前没有在当前区域中创建对应的日志服务Project，您需要输入Project名称新建一个。
    2.  **选择日志库**。

        选择对应的Logstore名称，并单击**下一步**。

        **说明：** 如您之前没有在当前区域中创建对应的日志服务Logstore，您需要输入Logstore名称新建一个。

    3.  **关联Bucket**。

        选择对应的Bucket名称，并单击**提交**。您可以选择将当前区域下的多个Bucket与该Logstore关联。

        ![](images/5432_zh-CN.png "关联Bucket")

    您已成功建立分发规则。请根据页面提示，前往日志服务控制台设置索引信息。


## 3 配置查询分析 {#section_t3j_55s_zdb .section}

1.  **关联Bucket**配置完成后，根据页面提示，单击弹出对话框中的**设置索引**，跳转到日志服务控制台查询分析 & 可视化页面。

    ![](images/5433_zh-CN.png "配置查询分析")

2.  日志服务已预设了OSS日志所需的查询索引，字段说明请查看[日志字段](cn.zh-CN/用户指南/云产品采集/OSS访问日志/日志字段.md)。。确认后单击**下一步**。

    ![](images/5434_zh-CN.png "配置索引")

    **说明：** 系统默认为您关联了Bucekt的Logstore创建4个特定的仪表盘，配置完成后您可以在仪表盘页面查看。您也可以在OSS控制台的日志分析页面中单击对应日志库**分析日志**，并单击左侧的仪表盘名称，直接查看仪表盘。

    ![](images/5435_zh-CN.png "分析日志")

3.  您可以按需要配置日志投递、ETL，或者直接单击**确认**，完成配置。

## 默认仪表盘 {#section_kd4_x5s_zdb .section}

日志服务提供了4个开箱即用的报表中心：

-   **oss\_operation\_center，展示总体运营状况的信息**

    ![](images/5436_zh-CN.png "oss_operation_center")

-   **oss\_access\_center，展示针对访问日志的统计信息**

    ![](images/5437_zh-CN.png "oss_access_center")

-   **oss\_performance\_center，展示针对性能的统计信息**

    ![](images/5438_zh-CN.png "oss_performance_center")

-   **oss\_audit\_center，展示文件删除修改的统计信息**

    ![](images/5439_zh-CN.png "oss_audit_center")


更多信息请参考[视频](https://yq.aliyun.com/articles/435682?spm=a2c4g.11186623.2.7.jntiYI)。

