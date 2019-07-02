# JSON type {#concept_vv4_z3q_zdb .concept}

Log Service supports the query and analysis of JSON-formatted logs. You can set the data type of indexes to JSON.

JSON-formatted data is a combination of multiple data types, including text, Boolean, numeric, array, and map. JSON-formatted data, as a common type of data, is self-parsed and flexible. It can be used to record data in complex scenarios. In many logs, the content without a fixed format is recorded in JSON format. For example, the request parameters and response of an HTTP request are recorded in a log in JSON format.

Log Service allows you to set the data type of index fields to JSON so that you can query and analyze logs in JSON format.

## Configuration {#section_py3_q1v_tdb .section}

-   Log Service can parse JSON-formatted fields and automatically generate indexes for all the fields of the text and Boolean types.

    ```
    json_string.key_map.key_text : test_value
    json_string.key_map.key_bool : true
    ```

-   To query the fields of the double or long type that is not in a JSON array, you can specify the JSON path.

    ```
    Set the data type of the key_map.key_long field to long.
    Query: json_string.key_map.key_long > 50
    ```

-   To query the fields of the text, double, or long type that is not in a JSON array, you can enable the statistical analysis feature and use SQL statements to analyze these fields.

    ```
    json_string.key_map.key_long > 10 | select count(*) as c , 
        "json_string.key_map.key_text" group by 
        "json_string.key_map.key_text"
    ```

    **Note:** 

    -   JSON object and JSON array types are not supported.
    -   Fields cannot be contained in a JSON array.
    -   Fields of the Boolean type can be converted into the text type.
    -   JSON-formatted fields must be enclosed in double quotation marks \(" "\) during log query and analysis.
-   Log Service can parse JSON-formatted data that contains both valid and invalid content.

    Log Service attempts to parse all the valid content until the invalid content appears.

    For example, the data after the key\_3 field is truncated and lost in the following code. Log Service can correctly parse the json\_string.key\_map.key\_2 field and the content before this field.

    ```
    "json_string": 
    {
         "key_1" :  "value_1",
         "key_map" : 
          {
                 "key_2" : "value_2",
                 "key_3" : "valu
    ```


## Query syntax {#section_zb3_v1v_tdb .section}

To query a specific key, you must add the JSON parent path as the prefix of the key in the query statement. The query syntax for the fields of the text and numeric types is the same for both JSON-formatted data and other data. For more information, see [Query syntax](reseller.en-US/User Guide/Index and query/Query/Query syntax.md).

## Query example {#section_ats_jtd_5cb .section}

The following log contains the time field and four keys, among which the message field is in JSON format.

|No.|Key|Type|
|:--|:--|:---|
|0|time|N/A|
|1|class|Text|
|2|status|Long|
|3|latency|Double|
|4|message|JSON|

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

The following figure shows how to set indexes for a log.

![](images/5522_en-US.png "Index settings")

where:

-   \(1\) indicates that you can query all the data of the string and Boolean types in JSON-formatted fields.
-   \(2\) indicates that you can query the data of the long type.
-   \(3\) indicates that you can use SQL statements to analyze the configured fields.

 **Examples**:

1.  **Query the fields of the string and Boolean types** 

    **Note:** 

    -   You do not need to configure JSON-formatted fields.
    -   JSON maps and JSON arrays are automatically expanded. You can query fields that are multi-level nested with each level separated by a period \(.\).
    ```
    message.traceInfo.requestId : 92.137_1518139699935_5599
    message.param.projectName : ali-log-test-project
    message.success : true
    message.result.data.ProjectStatus : Normal
    ```

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13098/156205060434240_en-US.png)

2.  **Query the fields of the double and long types** 

    **Note:** You need to configure each JSON-formatted field separately. Fields cannot be contained in a JSON array.

    ```
    message.usedTime > 40
    ```

3.  **Use SQL statements to analyze fields** 

    **Note:** 

    -   You need to configure each JSON-formatted field separately. Fields cannot be contained in a JSON array.
    -   Fields to be queried must be enclosed in double quotation marks \(" "\) or be configured with an alias.
    ```
    * | select avg("message.usedTime") as avg_time ,
    "message.methodName"  group by "message.methodName"
    ```


