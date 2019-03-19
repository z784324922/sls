# Overview {#concept_ijf_qs3_ffb .concept}

Log Service allows you to set indexes for the full text or some fields of the collected logs. If you set an index for the full text of a log, the value used to query this log is the content of the entire log. If you set indexes for some fields of a log, you can set the data type of each key used in queries.

## Data type {#section_s4c_wt5_zdb .section}

The following table describes the supported index types.

|Query type|Index type|Description|Example|
|:---------|:---------|:----------|:------|
|Basic|[text](reseller.en-US/User Guide/Index and query/Data type of index/Text type.md)|Indicates the text type that supports keywords and fuzzy matching in queries.| `uri:"login*" method:"post"` |
|[long](reseller.en-US/User Guide/Index and query/Data type of index/Value type.md)|Indicates the numeric type that supports interval queries.| `status>200, status in [200, 500]` |
|[double](reseller.en-US/User Guide/Index and query/Data type of index/Value type.md)|Indicates the numeric type that supports floating-point numbers.| `price>28.95, t in [20.0, 37]` |
|Combination|[json](reseller.en-US/User Guide/Index and query/Data type of index/JSON type.md)|Indicates that the index is a JSON field that supports nested queries. The field type is Text by default. You can set an index of the Text, Long, or Double type for element 'b' under element 'a' by using a path format such as 'a.b'. The field type is determined by the index type you set.| `level0.key>29.95 level0.key2:"action"` |
|[Full text](reseller.en-US/User Guide/Index and query/Data type of index/Text type.md) |Indicates that the full content of the log is queried as text.| `error and "login fail"` |

## Example {#section_ats_jtd_5cb .section}

The following log includes time and other four keys.

|No.|Key|Type|
|:--|:--|:---|
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

You can set indexes for a log as follows:

![](images/5522_en-US.png "Index setting")

In the preceding figure:

-   Mark ① indicates that the index type for this field is json and all data of the string type and bool type in the field can be queried.
-   Mark ② indicates that the index type for this field is long and data of the long type in the field can be queried.
-   Mark ③ indicates that the fields can be analyzed by using SQL statements.

**Example**:

1.  **Query data of the string type and bool type.**

    -   You do not need to configure the fields in the json field.
    -   JSON maps and arrays are automatically expanded. You can query fields that are multi-level nested with each level separated by a period \(.\).
    ```
    class : cental*
    message.traceInfo.requestId : 92.137_1518139699935_5599
    message.param.projectName : ali-log-test-project
    message.success : true
    ```

2.  **Query data of the Double type and Long type.**

    The fields in a JSON field must be configured separately and must not be contained in an array.

    ```
    latency>40
    message.usedTime > 40
    ```

3.  **Query data with combined data types.**

    ```
    class : cental* and message.usedTime > 40 not message.param.projectName:ali-log-test-project
    ```


