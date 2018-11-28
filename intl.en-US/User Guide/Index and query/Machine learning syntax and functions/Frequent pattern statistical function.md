# Frequent pattern statistical function {#concept_gnb_1wq_kfb .concept}

The frequent pattern statistical function mines representative combinations of attributes from the given multi-attribute field samples to summarize the current logs.

## pattern\_stat {#section_n3p_qlq_kfb .section}

Function format:

```
select pattern_stat(array\[col1, col2, col3\], array\['col1\_name', 'col2\_name', 'col3\_name'\], array\[col5, col6\], array\['col5\_name', 'col6\_name'\], supportScore, sample\_ratio) 
```

The following table describes the parameters.

|Parameter|Description|Value|
|:--------|:----------|:----|
|array\[col1, col2, col3\]|Input column composed of character type values|Values in array format, for example, array\[clientIP, sourceIP, path, logstore\]|
|array\['col1\_name', 'col2\_name', 'col3\_name'\]|Name corresponding to the input column composed of character type values|Values in array format, for example, array\['clientIP', 'sourceIP', 'path', 'logstore'\]|
|array\[col5, col6\]|Input column composed of numeric values|Values in array format, for example, array\[Inflow, OutFlow\]|
|array\['col5\_name', 'col6\_name'\]|Name corresponding to the input column composed of numeric values|Values in array format, for example, array\['Inflow', 'OutFlow'\]|
|supportScore|Support level of positive and negative samples for pattern mining|Double type values. Range: \(0,1\].|
|sample\_ratio|Sampling ratio with the default value of 0.1, which indicates that only 10% of the total samples are used|Double type values. Range: \(0,1\].|

Example:

-   Statement for query and analysis:

```
* | select pattern_stat(array[ Category, ClientIP, ProjectName, LogStore, Method, Source, UserAgent ], array[ 'Category', 'ClientIP', 'ProjectName', 'LogStore', 'Method', 'Source', 'UserAgent' ], array[ InFlow, OutFlow ], array[ 'InFlow', 'OutFlow' ], 0.45, 0.3) limit 1000
```

-   Result:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23361/154338412013565_en-US.png)


The following table describes the display items.

|Display item|Description|
|:-----------|:----------|
|count|Number of samples for the current pattern|
|supportScore|Support level for the current pattern|
|pattern|Pattern content, which is organized in the format of conditional queries|

