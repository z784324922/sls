# JSON格式 {#concept_y4l_34q_zdb .concept}

本文档主要介绍日志服务投递OSS使用JSON存储的相关配置，关于投递日志到OSS的其它内容请参考[投递流程](intl.zh-CN/用户指南/数据投递/投递日志到OSS/投递流程.md)。

OSS文件压缩类型及文件地址见下表。

|压缩类型|文件后缀|OSS文件地址举例|
|:---|:---|:--------|
|不压缩|无|oss://oss-shipper-shenzhen/ecs\_test/2016/01/26/20/54\_1453812893059571256\_937|
|snappy|.snappy|oss://oss-shipper-shenzhen/ecs\_test/2016/01/26/20/54\_1453812893059571256\_937.snappy|

## 不压缩 {#section_yxz_y4z_xgb .section}

Object由多条日志拼接而成，文件的每一行是一条JSON格式的日志，样例如下：

```
{"__time__":1453809242,"__topic__":"","__source__":"10.170.***.***","ip":"10.200.**.***","time":"26/Jan/2016:19:54:02 +0800","url":"POST
              /PutData?Category=YunOsAccountOpLog&AccessKeyId=<yourAccessKeyId>&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=<yourSignature>
              HTTP/1.1","status":"200","user-agent":"aliyun-sdk-java"}
```

## snappy压缩 {#section_hnk_1pz_xgb .section}

当前支持通过以下方式解压缩，解压后为原格式文件。

详细说明请查看[Snappy压缩文件](intl.zh-CN/用户指南/数据投递/投递日志到OSS/Snappy压缩文件.md)：

-   **使用 C++ Lib 解压缩**
-   **使用 Java Lib解压缩**
-   **使用Linux 环境解压工具**
-   **Python解压方案**

