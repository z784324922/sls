# JSON类型 {#concept_vv4_z3q_zdb .concept}

索引数据类型可设置为JSON类型，支持JSON格式日志的查询和分析功能。

JSON是由文本、布尔、数值、数组（Array）和图（Map）构成的组合类型数据。JSON数据作为一种通用类型的数据类型，其自解析、灵活的特性，使其能够很好满足复杂场景下数据的记录需求，在很多日志内容中格式不固定的部分往往都是以JSON的形式进行记录，例如将一次http请求的request参数和response内容以JSON的形式记录在一条日志中。

日志服务支持在索引中将字段设置为JSON类型，支持JSON格式日志的查询和分析。

## 配置说明 {#section_py3_q1v_tdb .section}

-   支持json格式解析，所有text、bool类型自动索引

    ```
    json_string.key_map.key_text : test_value
    json_string.key_map.key_bool : true
    ```

-   非json array中的double、long类型数据，可通过配置指定json路径后进行查询

    ```
    配置key_map.key_long这个字段的类型为long
    查询 : json_string.key_map.key_long > 50
    ```

-   非json array中的text、double、long类型字段，可开启“统计分析”功能，进行sql分析

    ```
    json_string.key_map.key_long > 10 | select count(*) as c , 
        "json_string.key_map.key_text" group by 
        "json_string.key_map.key_text"
    ```

    **说明：** 

    -   不支持json object、json array类型
    -   字段不能在json array中
    -   bool类型字段可以转成text类型
-   支持非完全合法json数据解析

    日志服务会尽可能解析有效内容，直到遇到非法部分结束。

    例如以下示例在key\_3之后的数据被截断丢失，对于这种缺失的日志，日志服务可正确解析到 json\_string.key\_map.key\_2 这个字段。

    ```
    "json_string": 
    {
         "key_1" :  "value_1",
         "key_map" : 
          {
                 "key_2" : "value_2",
                 "key_3" : "valu
    ```


## 查询语法 {#section_zb3_v1v_tdb .section}

指定Key查询需要加上JSON中父路径的前缀，文本、数值类查询语法与其他类型相同，详情请参见[查询语法](intl.zh-CN/用户指南/查询与分析/查询语法与功能/查询语法.md)。

## 查询示例 {#section_ats_jtd_5cb .section}

以下一条日志除时间外，还包含4个键值，其中"message"字段是json格式。

|序号|Key|类型|
|:-|:--|:-|
|0|time|-|
|1|class|text|
|2|status|long|
|3|latency|double|
|4|message|json|

```
0. time:2018-01-01 12:00:00
  1. class:central-log
  2. status:200
  3. latency:68.75
  4. message:
  {  
      "methodName": "getProjectInfo",
      "success": true,
      "remoteAddress": "1.1.1.1:11111",
      "usedTime": 48,
      "param": {
              "projectName": "ali-log-test-project",
              "requestId": "d3f0c96a-51b0-4166-a850-f4175dde7323"
      },
      "result": {
          "message": "successful",
          "code": "200",
          "data": {
              "clusterRegion": "ap-southeast-1",
              "ProjectName": "ali-log-test-project",
              "CreateTime": "2017-06-08 20:22:41"
          },
          "success": true
      }
  }
```

索引设置如下：

![](images/5522_zh-CN.png "索引设置")

其中：

-   ①表示可查询json字段中所有string和bool数据。
-   ②表示可查询long类型数据。
-   ③表示配置的字段可进行SQL分析。

**示例**：

1.  **查询string、bool类型**

    **说明：** 

    -   json内字段无需配置。
    -   json map、array自动展开，支持多层嵌套，每一层以”.”进行分割。
    ```
    message.traceInfo.requestId : 92.137_1518139699935_5599
    message.param.projectName : ali-log-test-project
    message.success : true
    message.result.data.ProjectStatus : Normal
    ```

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13098/154527937334240_zh-CN.png)

2.  **查询Double、Long类型**

    **说明：** 需要对json内字段独立配置，字段必须不在array。

    ```
    message.usedTime > 40
    ```

3.  **Sql 统计分析**

    **说明：** 

    -   需要对json内字段独立配置，字段必须不在array。
    -   查询字段需要使用引号，或者设置别名。
    ```
    * | select avg("message.usedTime") as avg_time ,
    "message.methodName"  group by "message.methodName"
    ```


