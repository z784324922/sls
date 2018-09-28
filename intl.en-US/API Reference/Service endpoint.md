# Service endpoint {#reference_wgx_pwq_zdb .reference}

## Internet service endpoint {#section_rxx_n4w_12b .section}

The Log Service endpoint is a URL used to access a project and logs within the project,  and is associated with the Alibaba Cloud region where the project  resides and the project name.  Currently, Log Service has been activated in multiple Alibaba Cloud regions. The Internet service endpoints for each region are as follows.

|Region |Service endpoint|
|:------|:---------------|
|China East 1 \(Hangzhou\) |cn-hangzhou.log.aliyuncs.com|
|China East 2 \(Shanghai\) |cn-shanghai.log.aliyuncs.com|
|China North 1 \(Qingdao\) |cn-qingdao.log.aliyuncs.com|
|China North 2 \(Beijing\) |cn-beijing.log.aliyuncs.com|
|China North 3 \(Zhangjiakou\) |cn-zhangjiakou.log.aliyuncs.com|
|China North 5 \(Huhehaote\) |cn-huhehaote.log.aliyuncs.com|
|China South 1 \(Shenzhen\) |cn-shenzhen.log.aliyuncs.com|
|Chengdu \(China\) |cn-chengdu.log.aliyuncs.com|
|Hong Kong \(China\) |cn-hongkong.log.aliyuncs.com|
|Asia Pacific NE 1 \(Tokyo\) |ap-northeast-1.log.aliyuncs.com|
|Asia Pacific SE 1 \(Singapore\) |ap-southeast-1.log.aliyuncs.com|
|Asia Pacific SE 2 \(Sydney\) |ap-southeast-2.log.aliyuncs.com|
|Asia Pacific SE 3 \(Kuala Lumpur\)|ap-southeast-3.log.aliyuncs.com|
|Asia Pacific SE 5 \(Jakarta\) |ap-southeast-5.log.aliyuncs.com|
|Middle East 1 \(Dubai\) |me-east-1.log.aliyuncs.com |
|US West 1 \(Silicon Valley\) |us-west-1.log.aliyuncs.com|
|EU Central 1 \(Frankfurt\) |eu-central-1.log.aliyuncs.com|
|US East 1 \(Virginia\) |us-east-1.log.aliyuncs.com|
|Asia Pacific SOU 1 \(Mumbai\) |ap-south-1.log.aliyuncs.com|
|UK \(London\)|eu-west-1.log.aliyuncs.com|

When accessing a specific project, you must give a final access address composed of the project name and the region where the project resides.  The specific format is as follows:

```
<project_name>.<region_endpoint>
```

For example, if the project name is big-game and it is in the China East 1 \(Hangzhou\) region, then the access address is as follows:

```
big-game.cn-hangzhou.log.aliyuncs.com
```

**Note:** 

You must specify a region when creating a Log Service project.  After the project is created, you cannot modify the region or migrate the project across regions, and you must select a root service endpoint address that matches the region to compose the access address for this project. The service endpoint is used for API requests. and you must  select a root service endpoint address that matches the region to compose the access address for this project. The service endpoint is used for API requests.

## Classic network/VPC service endpoint {#section_p5r_r4w_12b .section}

To use Log Service APIs on an Alibaba Cloud Elastic Compute Service \(ECS\) instance \(including the Virtual Private Cloud \(VPC\) environment\), you can also use the intranet service endpoints. Using intranet service endpoints to access Log Service does not consume ECS Internet traffic and saves the valuable ECS public network bandwidth\). The intranet root service endpoints for each  region are as follows.

|Region|Root service endpoint|
|:-----|:--------------------|
|China East 1 \(Hangzhou\)|cn-hangzhou-intranet.log.aliyuncs.com|
|China East 2 \(Shanghai\)|cn-shanghai-intranet.log.aliyuncs.com|
|China North 1 \(Qingdao\)|cn-qingdao-intranet.log.aliyuncs.com|
|China North 2 \(Beijing\)|cn-beijing-intranet.log.aliyuncs.com|
|China South 1 \(Shenzhen\)|cn-shenzhen-intranet.log.aliyuncs.com|
|China North 3 \(Zhangjiakou\)|cn-zhangjiakou-intranet.log.aliyuncs.com|
|China North 5 \(Huhehaote\) |cn-huhehaote-intranet.log.aliyuncs.com|
|Chengdu \(China\) |cn-chengdu-intranet.log.aliyuncs.com|
|Hong Kong \(China\) |cn-hongkong-intranet.log.aliyuncs.com|
|US West 1 \(Silicon Valley\)|us-west-1-intranet.log.aliyuncs.com|
|Asia Pacific NE 1 \(Tokyo\)|ap-northeast-1-intranet.log.aliyuncs.com|
|Asia Pacific SE 1 \(Singapore\) |ap-southeast-1-intranet.log.aliyuncs.com|
|Asia Pacific SE 2 \(Sydney\)|ap-southeast-2-intranet.log.aliyuncs.com|
|Asia Pacific SE 3 \(Kuala Lumpur\) |ap-southeast-3-intranet.log.aliyuncs.com|
|Asia Pacific SE 5 \(Jakarta\) |ap-southeast-5-intranet.log.aliyuncs.com|
|Middle East 1 \(Dubai\)|me-east-1-intranet.log.aliyuncs.com|
|EU Central 1 \(Frankfurt\) |eu-central-1-intranet.log.aliyuncs.com|
|US East 1 \(Virginia\)|us-east-1-intranet.log.aliyuncs.com|
|Asia Pacific SOU 1 \(Mumbai\) |ap-south-1-intranet.log.aliyuncs.com|
|UK \(London\)|eu-west-1-intranet.log.aliyuncs.com|

For example, then the intranet access address is as follows:

```
big-game.cn-hangzhou-intranet.log.aliyuncs.com
```

**Note:** Currently, Log Service APIs in the preceding service endpoints only support the HTTP or HTTPS protocol.

