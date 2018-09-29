# JSON type {#concept_vv4_z3q_zdb .concept}

JSON contains multiple data types, including text, boolean, value, array, and map.

## Instructions {#section_py3_q1v_tdb .section}

## Text type {#section_r5s_q1v_tdb .section}

For JSON fields, fields of text type and boolean type are automatically recognized.

For example, the following jsonkey can be queried by using the conditions such as `jsonkey.key1:"text_value"` .

```
jsonkey: {
   key1:text_value,
   key2:true,
   key3:3.14
}
```

## Value type {#section_z2b_s1v_tdb .section}

You can query the double or long type data that is not in the JSON array by setting the type and specifying the path.

For example, the type of the jsonkey.key3 field is double.Then, the query statement is as follows:

```
 jsonkey.key3 > 3
```

## JSON field including invalid content {#section_tbr_s1v_tdb .section}

Log Service attempts to parse the valid contents until the invalid content appears.

For example:

```
"json_string": 
{
     "key_1" : "value_1",
     "key_map" : 
      {
             "key_2" : "value_2",
             "key_3" : "valu
```

Data after key\_3 is truncated and lost. The field `json_string.key_map.key_2` and contents before this field are successfully parsed.

## Instructions {#section_nmq_51v_tdb .section}

-   JSON object type and JSON array type are not supported.
-   The field cannot be in a JSON array.
-   Boolean fields can be converted to the text type.

## Query syntax {#section_zb3_v1v_tdb .section}

To query a specific key, you must add the parent path prefix of JSON in the query statement. The text type and value type of JSON have the same query syntax as those of non-JSON. For more information, see [Query syntax](reseller.en-US/User Guide/Index and query/Query syntax.md).

