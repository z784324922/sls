# 投递MaxCompute时的参数时间 {#concept_38888_zh .concept}

本文档主要介绍日志服务投递日志至MaxCompute时需要设置的时间参数及含义。

日志服务投递日志至MaxCompute中有多个时间参数，如下图所示：

![参数截图](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13306/156576310813016_zh-CN.png)

其中涉及到的时间参数及其含义如下：

|时间参数|含义|
|:---|:-|
|sls\_time|日志服务收到日志的时间。|
|sls\_extract\_others|用户真实的时间。|
|sls\_partition\_time|MaxCompute归档的时间。|

**说明：** sls\_time一般都是略晚于sls\_extract\_others的时间。

如您的问题仍未解决，请联系售后技术支持。

