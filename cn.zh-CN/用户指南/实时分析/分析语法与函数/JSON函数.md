# JSON函数 {#concept_afg_4lq_zdb .concept}

JSON函数，可以解析一段字符串为JSON类型，并且提取JSON中的字段。JSON主要有两种结构：map和array。如果一个字符串解析成JSON失败，那么返回的是null。

如果需要把json展开成多行，请参考[unnest语法](intl.zh-CN/用户指南/实时分析/分析语法与函数/unnest语法.md)。

日志服务支持以下常见的JSON函数：

|函数名|含义|样例|
|:--|:-|:-|
|`json_parse(string)`|把字符串转化成JSON类型。|`SELECT json_parse('[1, 2, 3]')` 结果为JSON类型数组|
|`json_format(json)`|把JSON类型转化成字符串。|`SELECT json_format(json_parse('[1, 2, 3]'))`结果为字符串|
|`json_array_contains(json, value)`|判断一个JSON类型数值，或者一个字符串（内容是一个JSON数组）是否包含某个值。|`SELECT json_array_contains(json_parse('[1, 2, 3]'), 2)或 SELECT json_array_contains('[1, 2, 3]', 2)`|
|`json_array_get(json_array, index)`|同`json_array_contains`，是获取一个JSON数组的某个下标对应的元素。|`SELECT json_array_get('["a", "b", "c"]', 0)结果为'a'`|
|`json_array_length(json)`|返回JSON数组的大小。|`SELECT json_array_length('[1, 2, 3]') 返回结果3`|
|`json_extract(json, json_path)`|从一个JSON对象中提取值，JSON路径的语法类似`$.store.book[0].title`，返回结果是一个JSON对象。|`SELECT json_extract(json, '$.store.book');`|
|`json_extract_scalar(json, json_path)`|类似`json_extract`，但是返回结果是字符串类型。|-|
|`json_size(json,json_path)`|获取JSON对象或数组的大小。|`SELECT json_size('[1, 2, 3]') 返回结果3`|

