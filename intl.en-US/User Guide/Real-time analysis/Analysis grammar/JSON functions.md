# JSON functions {#concept_afg_4lq_zdb .concept}

JSON functions can parse a string as the JSON type and extract the fields in JSON. JSON mainly has the following two structures: map and array. If a string fails to be parsed as the JSON type, the returned value is null.

To split JSON into multiple lines, see [UNNEST function](reseller.en-US/User Guide/Real-time analysis/Analysis grammar/UNNEST function.md).

Log Service supports the following common JSON functions.

|Function|Description|Example|
|:-------|:----------|:------|
|`json_parse(string)`|Converts a string to the JSON type.|`SELECT json_parse('[1, 2, 3]')` returns a JSON array|
|`json_format(json)`|Converts the JSON type to a string.|`SELECT json_format(json_parse('[1, 2, 3]'))`returns a string|
|`json_array_contains(json, value)`|Determines whether a JSON type value or string \(whose content is a JSON array\) contains a value or not.|`SELECT json_array_contains(json_parse('[1, 2, 3]'), 2) or SELECT json_array_contains('[1, 2, 3]', 2)`|
|`json_array_get(json_array, index)`|The same as `json_array_contains`, which is used to obtain the element of a subscript of a JSON array.|`SELECT json_array_get('["a", "b", "c"]',  0) returns 'a'`|
|`json_array_length(json)`|Returns the size of the JSON array.|`SELECT json_array_length('[1, 2, 3]') Returns 3`|
|`json_extract(json, json_path)`|Extracts the value from a JSON object. The JSON path syntax is similar to `$.store.book[0].title`. The returned result is a JSON object.|`SELECT json_extract(json, '$.store.book');`|
|`json_extract_scalar(json, json_path)`|Similar to `json_extract`, but returns a string.|-|
|`json_size(json,json_path)`|Obtains the size of the JSON object or array.|`Select json_size ('[1, 2, 3]') returns 3`|

