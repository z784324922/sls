# Mapping function {#LogService_user_guide_0104 .reference}

The Log Service query analysis function supports log analysis by using mapping functions with detailed statements and implications described in the following table:

|Function|Description|Example|
|:-------|:----------|:------|
|Subscript operator \[\]|Gets the result of a key in the map.|-|
|histogram\(x\)|Performs GROUP BY according to each value of column x and calculates the count. The syntax is equivalent to `select count group by x`.**Note:** Returned information must be in JSON format.

|`latency > 10 | select histogram(status)`, which is equivalent to`latency > 10 | select count(1) group by status`|
|histogram\_u\(x\)|Performs GROUP BY according to each value of column x and calculates the count.**Note:** Returned information must be in multi-row multi-column format.

|`latency > 10 | select histogram(status)`, which is equivalent to `latency > 10 | select count(1) group by status`|
|map\_agg\(Key,Value\)|Returns a map of key, value, and shows the random latency of each method.| `latency > 100 | select map_agg(method,latency)` |
|multimap\_agg\(Key,Value\)|Returns a multi-value map of key, value, and returns all the latency for each method.| `latency > 100 | select multimap_agg(method,latency)` |
|cardinality\(x\) → bigint|Gets the size of the map.|-|
|element\_at\(map`<K, V>`, key\) → V|Gets the value corresponding to the key.|-|
|map\(\) → map`<unknown, unknown>`|Returns an empty map.|-|
|map\(array`<K>`, array`<V>`\) → map`<K,V>`|Converts two arrays into 1-to-1 maps.| `SELECT map(ARRAY[1,3], ARRAY[2,4]); — {1 -> 2, 3 -> 4}` |
|map\_from\_entries\(array`<row<K, V>>`\) → map`<K,V>`|Converts a multidimensional array into a map.| `SELECT map_from_entries(ARRAY[(1, ‘x’), (2, ‘y’)]); — {1 -> ‘x’, 2 -> ‘y’}` |
|map\_entries\(map`<K, V>`\) → array`<row<K,V>>`|Converts an element in a map into an array.| `SELECT map_entries(MAP(ARRAY[1, 2], ARRAY[‘x’, ‘y’])); — [ROW(1, ‘x’), ROW(2, ‘y’)]` |
|map\_concat\(map1`<K, V>`, map2`<K, V>`, …, mapN`<K, V>`\) → map`<K,V>`|The Union of multiple maps is required, if a key exists in multiple maps, take the first one.|-|
|map\_filter\(map`<K, V>`, function\) → map`<K,V>`|Refer to the lambda map\_filter function.|-|
|transform\_keys\(map`<K1, V>`, function\) → MAP`<K2,V>`|Refer to the lambda transform\_keys function.|-|
|transform\_values\(map`<K, V1>`, function\) → MAP`<K,V2>`|Refer to the lambda transform\_values function.|-|
|map\_keys\(x`<K, V>`\) → array`<K>`|Gets all the keys in the map and returns an array.|-|
|map\_values\(x`<K, V>`\) → array`<V>`|Gets all values in the map and returns an array.|-|
|map\_zip\_with\(map`<K, V1>`, map`<K, V2>`, function`<K, V1, V2, V3>`\) → map`<K,V3>`|Refer to power functions in Lambda.|-|

