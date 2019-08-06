# 查询OSS访问日志 {#concept_cw3_ghb_ygb .concept}

日志服务支持对采集到的OSS访问日志进行实时查询与分析统计、通过多种可视化图表对分析结果进行直观的展示。

## 配置查询分析 {#section_t3j_55s_zdb .section}

1.  **关联Bucket**配置完成后，根据页面提示，单击弹出对话框中的**设置索引**，跳转到日志服务控制台查询分析 & 可视化页面。

    ![配置查询分析](images/39791_zh-CN.png "配置查询分析")

2.  日志服务已预设了OSS日志所需的查询索引，字段说明请查看[日志字段](intl.zh-CN/数据采集/云产品采集/OSS访问日志/日志字段.md)。

    1.  选择目标项目后单击Project名称。
    2.  双击日志库名称，选择**查询分析属性** \> **设置**。
    ![配置索引](images/39792_zh-CN.png "配置索引")

    **说明：** 系统默认为您关联了Bucket的Logstore创建4个特定的仪表盘，配置完成后您可以在仪表盘页面查看。您也可以在OSS控制台的日志分析页面中单击对应日志库**分析日志**，并单击左侧的仪表盘名称，直接查看仪表盘。

    ![分析日志](images/39795_zh-CN.png "分析日志")


## 默认仪表盘 {#section_kd4_x5s_zdb .section}

日志服务提供了4个开箱即用的报表中心：

-   **oss\_performance\_center，展示针对性能的统计信息** 

    ![性能统计信息](images/39796_zh-CN.png "oss_operation_center")

-   **oss\_access\_center，展示针对访问日志的统计信息** 

    ![访问日志统计信息](images/39797_zh-CN.png "oss_access_center")

-   **oss\_performance\_center，展示针对性能的统计信息** 

    ![性能统计](images/39798_zh-CN.png "oss_performance_center")

-   **oss\_audit\_center，展示文件删除修改的统计信息** 

    ![文件删除修改](images/39799_zh-CN.png "oss_audit_center")


