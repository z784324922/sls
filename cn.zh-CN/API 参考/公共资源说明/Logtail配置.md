# Logtail配置 {#concept_f3n_s5q_12b .concept}

logtail配置叫做config，每个project默认可以创建100个配置（config）。Config名称在project下具备唯一性。

您可以通过config指定日志收集的位置、方式和参数。

**config命名规范：**

-   只能包括小写字母、数字、连字符（-）和下划线（\_）
-   必须以小写字母或者数字开头和结尾
-   长度必须在2~128字节以内

完整资源示例：

```
{
    "configName": "testcategory1",
    "inputType": "file",
    "inputDetail": {
        "logType": "common_reg_log",
        "logPath": "/var/log/httpd/",
        "filePattern": "access.log",
        "localStorage": true,
        "timeFormat": "%Y/%m/%d %H:%M:%S",
        "logBeginRegex": ".*",
        "regex": "(\w+)(\s+)",
        "key" :["key1", "key2"],
        "filterKey":["key1"],
        "filterRegex":["regex1"],
        "topicFormat": "none"
    },
    "outputType": "sls",
    "outputDetail": 
    {
        "logstoreName": "perfcounter"
    },
    "createTime"": 1400123456,
    "lastModifyTime": 1400123456
}
```

|属性名称|类型|是否必须|描述|
|:---|:-|:---|:-|
|configName|string|是|日志配置名称，Project下唯一|
|inputType|string|是|输入类型，默认为file|
|inputDetail|json|是|见下表格说明|
|outputType|string|是|输出类型，目前只支持LogService|
|outputDetail|string|是|见下表格说明|
|createTime\(output-only\)|integer|否|配置创建时间|
|lastModifyTime\(output-only\)|integer|否|该资源服务端更新时间|

inputDetail内容：

|属性名称|类型|必须|描述|
|:---|:-|:-|:-|
|logType|string|是|日志类型，现在只支持common\_reg\_log。|
|logPath|string|是|日志所在的父目录，例如`/var/logs/`。|
|filePattern|string|是|日志文件的Pattern，例如`access*.log`。|
|localStorage|boolean|是|是否打开本地缓存，在服务端之间链路断开的情况下，本地可以缓存1GB日志。|
|timeFormat|string|是|日志时间格式，如`%Y/%m/%d%H:%M:%S`。|
|logBeginRegex|string|是|日志首行特征（正则表达式），由于匹配多行日志组成一条log的情况。|
|regex|string|是|日志对提取正则表达式。|
|key|array|是|日志提取后所生成的Key。|
|filterKey|array|是|用于过滤日志所用到的Key，只有Key的值满足对应filterRegex列中设定的正则表达式日志才是符合要求的。|
|filterRegex|array|是|和每个filterKey对应的正正则表达式，filterRegex的长度和filterKey的长度必须相同。|
|topicFormat|string|否|用于将日志文件路径的某部分作为topic，如`/var/log/(.*).log`，默认为none，表示topic为空。|
|preserve|boolean|否|true代表监控目录永不超时，false代表监控目录30分钟超时，默认值为true。|
|preserveDepth|integer|否|当设置preserve为false时，指定监控不超时目录的深度，最大深度支持3。|

outputDetail内容：

|属性名称|类型|必须|描述|
|:---|:-|:-|:-|
|logstoreName|string|是|对应Logstore名称|

