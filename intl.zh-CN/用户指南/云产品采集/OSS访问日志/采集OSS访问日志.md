# 采集OSS访问日志 {#task_895435 .task}

志服务支持采集OSS访问日志，并对采集到的OSS访问日志进行实时查询与分析统计、通过多种可视化图表对分析结果进行直观的展示。日志服务对OSS访问日志的专业日志采集分析手段，可以降低操作审计和事件回溯的复杂度，让您的工作更有效率。

1.  已开通日志服务。
2.  已开通OSS服务，并成功创建了Bucket。
3.  日志服务Project需要和OSS Bucket在同一个阿里云账号的同一Region下。

1.  日志采集授权 单击[快捷授权](https://ram.console.aliyun.com/?spm=a2c4g.11186623.2.5.jntiYI#/role/authorize?request=%7B%22Requests%22%3A%20%7B%22request1%22%3A%20%7B%22RoleName%22%3A%20%22AliyunLogArchiveRole%22%2C%20%22TemplateId%22%3A%20%22Archive%22%7D%7D%2C%20%22ReturnUrl%22%3A%20%22https%3A//sls.console.aliyun.com/%22%2C%20%22Service%22%3A%20%22Log%22%7D)为日志服务授权，授权后日志服务有权限将OSS访问日志分发到您的Logstore中。

    若您之前没有授权，也可以在OSS控制台首页单击**日志分析** \> **日志采集授权**，根据提示完成授权。

2.  关联Bucket 
    1.  在OSS控制台首页单击**日志分析** \> **管理日志服务**，进入日志分析页面。
    2.  单击**创建关联**，进入新建关联步骤。

        ![](images/5431_zh-CN.png "新建关联")

        1.  **选择/新建项目**。

            选择日志服务Project所属的区域、日志服务项目名称，并单击**下一步**。

            **说明：** 

            -   **您的日志服务Project与OSS Bucket须处于同一区域，多个Bucket日志可以采集至同一个Logstore中**。
            -   如您之前没有在当前区域中创建对应的日志服务Project，您可以单击**新建项目**新建一个Project。
        2.  **选择/新建日志库**。

            选择对应的Logstore名称，并单击**下一步**。

            **说明：** 如您之前没有在当前区域中创建对应的日志服务Logstore，您可以单击**新建日志库**新建一个Logstore。

        3.  **关联Bucket**。

            选择对应的Bucket名称，并单击**提交**。您可以选择将当前区域下的多个Bucket与该Logstore关联。

            ![](images/5432_zh-CN.png "关联Bucket")

        您已成功建立分发规则。请根据页面提示，前往日志服务控制台设置索引信息。


