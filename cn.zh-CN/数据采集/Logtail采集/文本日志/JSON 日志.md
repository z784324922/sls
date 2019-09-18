# JSON 日志 {#reference_dsq_3v5_vdb .task}

日志服务支持通过控制台数据接入向导一站式配置JSON模式采集日志与设置索引。

Logtail支持的JSON日志是Object类型，可以自动提取Object首层的键作为字段名称，Object首层的值作为字段值。字段值可以是Object、Array或基本类型，如String、Number等。

JSON日志建构于两种结构：

-   Object：“键/值”对的集合（A collection of key/value pairs）。
-   Array：值的有序列表（An ordered list of values）。

JSON行与行之间用`\n`进行分割，每一行作为一条单独日志进行提取。

如果是JSON Array等非Object类型数据，logtail不支持自动解析，请使用正则表达式提取字段，或者使用极简模式整行采集日志。

本章节主要介绍JSON模式的Logtail配置部分，关于其他采集步骤的详细说明请参见[文本日志采集流程](cn.zh-CN/数据采集/Logtail采集/文本日志/文本日志采集流程.md#)。

## 日志样例 {#section_adx_7fp_vhg .section}

``` {#codeblock_cry_pw6_xsq}
{"url": "POST /PutData?Category=YunOsAccountOpLog&AccessKeyId=U0Ujpek********&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=pD12XYLmGxKQ%2Bmkd6x7hAgQ7b1c%3D HTTP/1.1", "ip": "10.200.98.220", "user-agent": "aliyun-sdk-java", "request": {"status": "200", "latency": "18204"}, "time": "05/May/2016:13:30:28"}
{"url": "POST /PutData?Category=YunOsAccountOpLog&AccessKeyId=U0Ujpek********&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=pD12XYLmGxKQ%2Bmkd6x7hAgQ7b1c%3D HTTP/1.1", "ip": "10.200.98.210", "user-agent": "aliyun-sdk-java", "request": {"status": "200", "latency": "10204"}, "time": "05/May/2016:13:30:29"}
```

## 采集步骤 {#section_xmz_djl_327 .section}

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。
2.  选择数据源类型。 通过JSON模式采集请选择**JSON-文本日志**。
3.  选择日志空间。 请选择Project和Logstore，您也可以直接单击**立即创建**新建Project和Logstore。具体步骤请参见[准备流程](../../../../cn.zh-CN/准备工作/准备流程.md#)。

    如果您是通过日志库下的**数据接入**后的加号进入采集配置流程，系统会直接跳过该步骤。

4.  创建机器组。 在创建机器组之前，您需要首先确认已经安装了Logtail。

    -   ECS机器： Linux系统勾选实例后单击**安装**进行一键式安装。Windows系统不支持一键式安装，请参见[安装Logtail（Windows系统）](../../../../cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Windows系统）.md#)手动安装。
    -   自建（非ECS）机器：请根据界面提示进行安装。或者请参见[安装Logtail（Linux系统）](cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Linux系统）.md#)或[安装Logtail（Windows系统）](cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Windows系统）.md#)进行安装。
    安装完Logtail后单击**确认安装完毕**创建机器组，具体请参见[简介](../../../../cn.zh-CN/数据采集/Logtail采集/机器组/简介.md#)。如果您之前已经创建好机器组 ，请直接单击**使用现有机器组**。

5.  机器组配置。 选择一个机器组，将该机器组从**源机器组**移动到**应用机器组**。

    ![机器组配置](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13062/156878545760275_zh-CN.png)

6.  Logtail配置。 JSON模式的Logtail配置项说明如下。

    |配置项|详情|
    |---|--|
    |配置名称|配置名称只能包含小写字母、数字、连字符（-）和下划线（\_），且必须以小写字母和数字开头和结尾，长度为3~128字符。 **说明：** 配置名称设置后不可修改。

 |
    |日志路径|指定日志的目录和文件名。     -   日志文件名支持完整文件名和通配符两种模式，文件名规则请参见[Wildcard matching](http://man7.org/linux/man-pages/man7/glob.7.html)。
    -   日志文件查找模式为多层目录匹配，即指定文件夹下所有符合文件名模式的文件都会被监控到，包含所有层次的目录。

        -   例如`/apsara/nuwa/ … /*.log`表示`/apsara/nuwa`目录中（包含该目录的递归子目录）后缀名为`.log`的文件。
        -   例如`/var/logs/app_* … /*.log*`表示`/var/logs`目录下所有符合`app_*`模式的目录中（包含该目录的递归子目录）文件名包含`.log`的文件。
**说明：** 

        -   一个文件只能被一个配置收集。
        -   目录通配符只支持 `*`和`?` 两种。
 |
    |是否为Docker文件|如果是Docker容器内部文件，可以直接配置内部路径与容器Tag，Logtail会自动监测容器创建和销毁，并根据Tag进行过滤采集指定容器的日志。|
    |模式|之前步骤我们选择的数据源是**JSON-文本日志**，所以这里默认是**JSON模式**，您也可以手动修改。|
    |使用系统时间|当打开提取字段开关时，需要设置。 如果关闭，您需要在提取字段时指定某一字段（Value）为时间字段，并命名为 `time`。在选取 `time` 字段后，您可以单击**时间转换格式**中的**自动生成** 生成解析该时间字段的方式。关于日志时间格式的更多信息请参见[配置时间格式](../../../../cn.zh-CN/数据采集/Logtail采集/文本日志/配置时间格式.md#)。

 |
    |丢弃解析失败日志|请选择解析失败的日志是否上传到日志服务。     -   开启后，解析失败的日志不会上传到日志服务。
    -   关闭后，日志解析失败时上传原始日志。
 |
    |最大监控目录深度|指定从日志源采集日志时，监控目录的最大深度，即最多监控几层日志。最大目录监控深度范围0-1000，0代表只监控本层目录。|

    ![JSON日志采集配置](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13047/156878545760714_zh-CN.png)

7.  配置**高级选项**，设置完成后，单击**下一步**。 请根据您的需求选择高级配置。如没有特殊需求，建议保持默认配置。

    |配置项|详情|
    |:--|:-|
    |上传原始日志|请选择是否需要上传原始日志。开启该功能后，原始日志内容会作为\_\_raw\_\_字段与解析过的日志一并上传。|
    |Topic生成方式|     -   **空-不生成Topic**：默认选项，表示设置Topic为空字符串，在查询日志时不需要输入Topic即可查询。
    -   **机器组Topic属性**：设置Topic生成方式为机器组Topic属性，可以用于明确区分不同前端服务器产生的日志数据。
    -   **文件路径正则**：选择此项之后，您需要填写下方的**自定义正则**，用正则式从路径里提取一部分内容作为Topic。可以用于区分具体用户或实例产生的日志数据。
 |
    |自定义正则|如您选择了**文件路径正则**方式生成Topic，需要在此处填写您的自定义正则式。|
    |日志文件编码|     -   utf8：指定使用UTF-8编码。
    -   gbk：指定使用GBK编码。
 |
    |时区属性|设置采集日志时，日志时间的时区属性。     -   机器时区：默认为机器所在时区。
    -   自定义时区：手动选择时区。
 |
    |超时属性|如果一个日志文件在指定时间内没有任何更新，则认为该文件已超时。您可以对**超时属性**指定以下配置。     -   永不超时：指定持续监控所有日志文件，永不超时。
    -   30分钟超时：如日志文件在30分钟内没有更新，则认为已超时，并不再监控该文件。
 |
    |过滤器配置|日志只有**完全符合**过滤器中的条件才会被收集。 例如：

    -   **满足条件即收集**：配置`Key:level Regex:WARNING|ERROR`，表示只收集level为WARNING或ERROR类型的日志。
    -   [过滤不符合条件的数据](http://www.regular-expressions.info/lookaround.html)：
        -   配置`Key:level Regex:^(?!.*(INFO|DEBUG)).*`，表示不收集level为INFO或DEBUG类型的日志。
        -   配置`Key:url Regex:.*^(?!.*(healthcheck)).*`，表示不采集url中带有healthcheck的日志，例如key为url，value为`/inner/healthcheck/jiankong.html`的日志将不会被采集。
更多示例可参见[regex-exclude-word](https://stackoverflow.com/questions/2404010/match-everything-except-for-specified-strings)、[regex-exclude-pattern](https://stackoverflow.com/questions/2078915/a-regular-expression-to-exclude-a-word-string)。

 |

8.  查询分析配置。 默认已经设置好索引，您也可以根据业务需求，重新设置索引。具体请参见[开启并配置索引](../../../../cn.zh-CN/查询与分析/开启并配置索引.md#)。

配置完成后您就可以开始通过JSON模式采集日志。

