# Map映射函数 {#LogService_user_guide_0104 .reference}

日志服务查询分析功能支持通过映射函数进行日志分析，详细语句及含义如下：

|函数|含义|示例|
|:-|:-|:-|
|下标运算符\[\]|获取map中某个key对应的结果。|-|
|histogram\(x\)|按照x的每个值GROUP BY，计算count。语法相当于`select count group by x`。**说明：** 返回结果为JSON格式。

|`latency > 10 | histogram(status)`，等同于`latency > 10 | select count(1) group by status`。|
|histogram\_u\(x\)|按照x的每个值GROUP BY，计算count。**说明：** 返回结果为多行多列。

|`latency > 10 | histogram(status)`，等同于`latency > 10 | select count(1) group by status`。|
|map\_agg\(Key,Value\)|返回Key、Value组成的map，并展示每个method的随机的latency。| `latency > 100 | select map_agg(method,latency)` |
|multimap\_agg\(Key,Value\)|返回Key、Value组成的多Value map，并返回每个method的所有的latency。| `latency > 100 | select multimap_agg(method,latency)` |
|cardinality\(x\) → bigint|获取map的大小。|-|
|element\_at\(map`<K, V>`, key\) → V|获取key对应的value。|-|
|map\(\) → map`<unknown, unknown>`|返回一个空的map。|-|
|map\(array`<K>`, array`<V>`\) → map`<K,V>`|把两个数组，转换成1对1的Map。| `SELECT map(ARRAY[1,3], ARRAY[2,4]); — {1 -> 2, 3 -> 4}` |
|map\_from\_entries\(array`<row<K, V>>`\) → map`<K,V>`|把一个多维数组转化成map。| `SELECT map_from_entries(ARRAY[(1, ‘x’), (2, ‘y’)]); — {1 -> ‘x’, 2 -> ‘y’}` |
|map\_entries\(map`<K, V>`\) → array`<row<K,V>>`|把map中的元素转化成array形式。| `SELECT map_entries(MAP(ARRAY[1, 2], ARRAY[‘x’, ‘y’])); — [ROW(1, ‘x’), ROW(2, ‘y’)]` |
|map\_concat\(map1`<K, V>`, map2`<K, V>`, …, mapN`<K, V>`\) → map`<K,V>`|求多个map的并集，如果某个key在多个map中存在，则取第一个。|-|
|map\_filter\(map`<K, V>`, function\) → map`<K,V>`|请参考lambda map\_filter函数。|-|
|transform\_keys\(map`<K1, V>`, function\) → MAP`<K2,V>`|请参考lambda transform\_keys函数。|-|
|transform\_values\(map`<K, V1>`, function\) → MAP`<K,V2>`|请参考lambda transform\_values函数。|-|
|map\_keys\(x`<K, V>`\) → array`<K>`|获取map中所有的key，返回array。|-|
|map\_values\(x`<K, V>`\) → array`<V>`|获取map中所有的value，返回array。|-|
|map\_zip\_with\(map`<K, V1>`, map`<K, V2>`, function`<K, V1, V2, V3>`\) → map`<K,V3>`|请参考lambda中 map\_zip\_with函数。|-|

