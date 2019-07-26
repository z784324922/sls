# API网关访问日志 {#concept_lpx_53q_zdb .concept}

阿里云API网关提供API托管服务，在微服务聚合、前后端分离、系统集成上为用户提供诸多便利。每一次API请求都对应一条访问记录，内容包括调用者IP、请求的URL、响应延迟、返回状态码、请求和响应字节数等重要信息，可以便于用户了解其Web服务的运行状况。

![API网关](images/5402_zh-CN.png "API网关")

日志服务支持通过**数据接入向导**采集API网关的访问日志。

## 功能简介 {#section_hnc_fqs_zdb .section}

1.  **日志在线查询**：您可以根据日志中任意关键字进行快速的精确或模糊检索，可用于问题定位或者统计查询。
2.  **详细调用日志**：您可以检索API调用的详细日志。
3.  **自定义分析图表**：您可以根据统计需求将任意日志项自定义统计图表，以满足您日常的业务需要。
4.  **预置分析报表**：API网关预定义了一些全局统计图表。包括：请求量大小、成功率、错误率、延时情况、调用API的APP数量，错误情况统计、TOP 分组、TOP API、Top 延迟等等。

## 字段说明 {#section_qjv_gqs_zdb .section}

|日志字段|描述|
|:---|:-|
|apiGroupUid|API的分组ID。|
|apiGroupName|API分组名称。|
|apiUid|API的ID。|
|apiName|API名称。|
|apiStageUid|API环境ID。|
|apiStageName|API环境名称。|
|httpMethod|调用的HTTP方法。|
|path|请求的PATH。|
|domain|调用的域名。|
|statusCode|Http的状态码。|
|errorMessage|错误信息。|
|appId|调用者应用ID。|
|appName|调用者应用名称|
|clientIp|调用者客户端IP。|
|exception|后端返回的具体错误信息。|
|providerAliUid|API提供者帐户ID。|
|region|区域，如：cn-hangzhou。|
|requestHandleTime|请求时间，格林威治时间。|
|requestId|请求ID，全局唯一。|
|requestSize|请求大小，单位：字节。|
|responseSize|返回数据大小，单位：字节。|
|serviceLatency|后端延迟，单位：毫秒。|

## 配置步骤 {#section_mmk_dqs_zdb .section}

1.  创建Project和Logstore。

    请参考[创建Project](cn.zh-CN/用户指南/准备工作/操作Project.md#)和[创建Logstore](cn.zh-CN/用户指南/准备工作/操作Logstore.md#)。

    若Logstore已存在请跳过本步骤。

2.  展开对应日志库，单击**数据接入**后的加号。

    您也直接单击页面右侧的**接入数据**按钮，在接入流程中再选择日志库。

    ![接入数据](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13062/156410346152859_zh-CN.png)

3.  选择数据类型。

    单击**接入数据**中的**API网关**。

4.  选择日志空间。

    如果您是通过日志库下的**数据接入**后的加号进入采集配置流程，系统会直接跳过该步骤。

5.  数据源设置。

    在**数据源设置**步骤中，检查您是否已完成以下配置：

    1.  **是否开通API网关服务。** 

        API 网关为您提供完整的 API 托管服务，辅助用户将服务、数据以 API 的形式开放给合作伙伴，也可以发布到 API 市场供更多的开发者采购使用。

        如您尚未开通**API网关**服务，请按照页面提示开通。

    2.  **是否完成RAM授权。** 

        建立分发规则之前需要通过访问控制RAM为日志服务授权，允许日志服务采集您的API网关日志。

        单击右上角的**授权**，完成快捷授权。

    3.  **是否已建立分发规则。** 

        如您是第一次操作此步骤，系统会自动完成API网关日志导入和分发规则；如您此前已配置过**API网关日志**采集，页面会提示**已经存在日志分发规则**，您可以选择删除旧的分发规则。

6.  查询分析配置。

    默认已经设置好索引，如果您需要重新设置索引，请在查询分析页面选择**查询分析属性** \> **设置进行修改**。


至此，数据接入初始化工作完成，您可以选择刚才设置的Logstore api-gateway-access-log进行日志查询、分析或者进入仪表盘查看报表。

如您需要修改或删除配置，请参考[API网关文档](https://help.aliyun.com/document_detail/64818.html)，在[API网关控制台](https://apigateway.console.aliyun.com/)操作。

