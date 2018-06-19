# API网关访问日志 {#concept_lpx_53q_zdb .concept}

阿里云API网关提供API托管服务，在微服务聚合、前后端分离、系统集成上为用户提供诸多便利。访问日志（Acccess Log）是由web服务生成的日志，每一次API请求都对应一条访问记录，内容包括调用者IP、请求的URL、响应延迟、返回状态码、请求和响应字节数等重要信息，可以便于用户了解其Web服务的运行状况。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13090/5402_zh-CN.png "API网关")

日志服务支持通过**数据接入向导**采集API网关的访问日志。

## **功能简介** {#section_hnc_fqs_zdb .section}

1.  **日志在线查询。**您可以根据日志中任意关键字进行快速的精确或模糊检索，可用于问题定位或者统计查询。
2.  **详细调用日志。**您可以检索API调用的详细日志。
3.  **自定义分析图表。**您可以根据统计需求将任意日志项自定义统计图表，以满足您日常的业务需要。
4.  **预置分析报表。**API网关预定义了一些全局统计图表。包括：请求量大小、成功率、错误率、延时情况、调用API的APP数量，错误情况统计、TOP 分组、TOP API、Top 延迟等等。

## 字段说明 {#section_qjv_gqs_zdb .section}

|日志字段|描述|
|:---|:-|
|apiGroupUid|API的分组ID|
|apiGroupName|API分组名称|
|apiUid|API的ID|
|apiName|API名称|
|apiStageUid|API环境ID|
|apiStageName|API环境名称|
|httpMethod|调用的HTTP方法|
|path|请求的PATH|
|domain|调用的域名|
|statusCode|HttpStatusCode|
|errorMessage|错误信息|
|appId|调用者应用ID|
|appName|调用者应用名称|
|clientIp|调用者客户端IP|
|exception|后端返回的具体错信息|
|providerAliUid|API提供者帐户ID|
|region|区域，如：cn-hangzhou|
|requestHandleTime|请求时间，格林威治时间|
|requestId|请求ID，全局唯一|
|requestSize|请求大小，单位：字节|
|responseSize|返回数据大小，单位：字节|
|serviceLatency|后端延迟，单位：毫秒|

## 配置步骤 {#section_mmk_dqs_zdb .section}

1.  创建Project和Logstore。

    请参考[准备工作](intl.zh-CN/用户指南/准备工作.md)创建一个Project和Logstore。

    若Logstore已存在请跳过本步骤。

2.  进入数据接入向导。

    创建Logstore后，在Logstore列表界面单击数据接入向导图标，进入数据接入向导流程。

3.  选择数据类型

    在**云产品日志**部分单击**API网关**，并单击**下一步**，进入**数据源设置**。

4.  设置数据源。

    在**数据源设置**步骤中，检查您是否已完成以下配置：

    1.  **是否开通API网关服务。**

        API 网关为您提供完整的 API 托管服务，辅助用户将能力、服务、数据以 API 的形式开放给合作伙伴，也可以发布到 API 市场供更多的开发者采购使用。

        如您尚未开通**API网关**服务，请按照页面提示开通。

    2.  **是否完成RAM授权。**

        建立分发规则之前需要通过访问控制RAM为日志服务授权，允许日志服务采集您的API网关日志。

        单击右上角的**授权**，完成快捷授权。

    3.  **是否已建立分发规则。**

        如您是第一次操作此步骤，系统会自动完成API网关日志导入和分发规则；如您此前已配置过**API网关日志**采集，页面会提示**已经存在日志分发规则**，您可以选择删除旧的分发规则。

    单击**下一步**，进入**查询分析&可视化**配置。

5.  配置**查询分析&可视化**。

    请按照下图配置索引。索引的配置关系到您的日志检索分析效率，您在仪表盘中也会使用到索引配置，请谨慎修改。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13090/5403_zh-CN.png "配置索引")

    单击**下一步**，结束配置。日志投递可待有需要时另行配置。


至此，**数据接入向导**初始化工作完成，您可以选择刚才设置的Logstore api-gateway-access-log进行日志查询、分析，或者进入仪表盘查看报表。

