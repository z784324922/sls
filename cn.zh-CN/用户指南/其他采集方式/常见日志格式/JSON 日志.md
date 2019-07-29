# JSON 日志 {#reference_dsq_3v5_vdb .reference}

Logtail 支持的 JSON 日志是 Object 类型，可以自动提取 Object 首层的键作为字段名称，Object 首层的值作为字段值。字段值可以是Object、Array或基本类型，如String、Number等。

JSON日志建构于两种结构：

-   Object：“键/值”对的集合（A collection of key/value pairs）。
-   Array：值的有序列表（An ordered list of values）。

JSON行与行之间用`\n`进行分割，每一行作为一条单独日志进行提取。

如果是 JSON Array 等非 Object 类型数据，logtail 不支持自动解析，请使用正则表达式提取字段，或者使用极简模式整行采集日志。

## 日志样例 {#section_ets_2bc_ry .section}

``` {#codeblock_090_0br_1yr}
{"url": "POST /PutData?Category=YunOsAccountOpLog&AccessKeyId=U0Ujpek********&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=pD12XYLmGxKQ%2Bmkd6x7hAgQ7b1c%3D HTTP/1.1", "ip": "10.200.98.220", "user-agent": "aliyun-sdk-java", "request": {"status": "200", "latency": "18204"}, "time": "05/May/2016:13:30:28"}
{"url": "POST /PutData?Category=YunOsAccountOpLog&AccessKeyId=U0Ujpek********&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=pD12XYLmGxKQ%2Bmkd6x7hAgQ7b1c%3D HTTP/1.1", "ip": "10.200.98.210", "user-agent": "aliyun-sdk-java", "request": {"status": "200", "latency": "10204"}, "time": "05/May/2016:13:30:29"}
```

## 配置Logtail收集JSON日志 {#section_dpf_3bc_ry .section}

通过Logtail收集JSON日志完整流程请参考[五分钟快速入门](../cn.zh-CN/快速入门/五分钟快速入门.md)，根据您的网络部署和实际情况选择对应配置。本文档仅展示配置Logtail的第二步**指定收集模式**步骤中的详细配置。

1.  登录[日志服务控制台](https://sls.console.aliyun.com)，单击Project名称。
2.  单击**接入数据**按钮，并在**接入数据**页面中选择**单行-文本日志**。
3.  选择日志空间。

    可以选择已有的Logstore，也可以新建Project或Logstore。

4.  创建并配置机器组。

    在创建机器组之前，您需要首先确认已经安装了Logtail。 安装完Logtail后单击**确认安装完毕**创建机器组。如果您之前已经创建好机器组 ，请直接单击**使用现有机器组**。

    选择一个机器组，将该机器组从**源机器组**移动到**应用机器组**。

5.  数据源设置。
    1.  填写**配置名称**、**日志路径**，并选择日志收集模式为**JSON模式**。
    2.  请根据您的需求确认是否使用系统时间作为日志时间。您可以选择开启或者关闭**使用系统时间**功能。
        -   开启 **使用系统时间** 

            使用系统时间表示不提取日志中的时间字段，将日志服务采集该日志的时间作为日志时间。

        -   关闭 **使用系统时间** 

            不使用系统时间表示从日志数据中提取时间字段，将其作为日志时间。

            如果选择关闭 **使用系统时间**，您需要定义被提取作为时间字段的Key名称，同时定义时间转换格式。例如JSON Object中的 `time` 字段（05/May/2016:13:30:29）可以提取为日志时间。配置日期格式请参考[配置时间格式](cn.zh-CN/用户指南/Logtail采集/文本日志/配置时间格式.md)。

             ![采集设置](images/2638_zh-CN.png "收集JSON日志")


