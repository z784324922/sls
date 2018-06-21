# JSON 日志 {#reference_dsq_3v5_vdb .reference}

JSON日志 建构于两种结构：

-   Object：“键/值”对的集合（A collection of name/value pairs）。
-   Array：值的有序列表（An ordered list of values）。

logtail 支持的 JSON 日志是 Object 类型，可以自动提取 Object 首层的键作为字段名称，Object 首层的值作为字段值。字段值可以是Object、Array或基本类型，如String、Number等。JSON行与行之间用`\n`进行分割，每一行作为一条单独日志进行提取。

如果是 JSON Array 等非 Object 类型数据，logtail 不支持自动解析，请使用正则表达是提取字段，或者使用极简模式整行采集日志。

## 日志样例 {#section_ets_2bc_ry .section}

```
{"url": "POST /PutData?Category=YunOsAccountOpLog&AccessKeyId=U0Ujpek********&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=pD12XYLmGxKQ%2Bmkd6x7hAgQ7b1c%3D HTTP/1.1", "ip": "10.200.98.220", "user-agent": "aliyun-sdk-java", "request": {"status": "200", "latency": "18204"}, "time": "05/May/2016:13:30:28"}
{"url": "POST /PutData?Category=YunOsAccountOpLog&AccessKeyId=U0Ujpek********&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=pD12XYLmGxKQ%2Bmkd6x7hAgQ7b1c%3D HTTP/1.1", "ip": "10.200.98.210", "user-agent": "aliyun-sdk-java", "request": {"status": "200", "latency": "10204"}, "time": "05/May/2016:13:30:29"}
```

## 配置Logtail收集JSON日志 {#section_dpf_3bc_ry .section}

通过Logtail收集JSON日志完整流程请参考[五分钟快速入门](../../../../intl.zh-CN/快速入门/五分钟快速入门.md)，根据您的网络部署和实际情况选择对应配置。本文档仅展示配置Logtail的第二步**指定收集模式**步骤中的详细配置。

1.  在Logstore列表界面单击**数据接入向导**图表，进入数据接入向导。
2.  选择数据类型。

    选择**文本文件**并单击**下一步**。

3.  配置数据源。
    1.  填写配置名称、日志路径，并选择日志收集模式为**JSON模式**。
    2.  请根据您的需求确认是否使用系统时间作为日志时间。您可以选择开启或者关闭**使用系统时间**功能。
        -   开启 **使用系统时间**

            使用系统时间表示不提取日志中的时间字段，将日志服务采集该日志的时间作为日志时间。

        -   关闭 **使用系统时间**

            不使用系统时间表示从日志数据中提取时间字段，将其作为日志时间。

            如果选择关闭 **使用系统时间**，您需要定义被提取作为时间字段的Key名称，同时定义时间转换格式。例如JSON Object中的 `time` 字段（05/May/2016:13:30:29）可以提取为日志时间。配置日期格式请参考[文本-配置时间格式](intl.zh-CN/用户指南/Logtail 采集/数据源/文本-配置时间格式.md)。

             ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13047/2638_zh-CN.png "收集JSON日志") 


