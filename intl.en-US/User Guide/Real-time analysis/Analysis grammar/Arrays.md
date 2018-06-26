# Arrays {#concept_ymd_dmq_zdb .concept}

|Statement|Meaning|Example|
|:--------|:------|:------|
|Subscript operator \[\]|\[\] is used to obtain a certain element in the array.|-|
|Connection operator ||||| is used to connect two arrays into one.| `SELECT ARRAY [1] || ARRAY [2]; — [1, 2]`  `SELECT ARRAY [1] || 2; — [1, 2]` 

  `SELECT 2 || ARRAY [1]; — [2, 1]` 

 |
|array\_distinct|Obtain the distinct elements in the array by means of array deduplication.|-|
|array\_intersect\(x, y\)|Obtain the intersection of arrays x and y.|-|
|array\_union\(x, y\) → array|Obtain the union of arrays x and y.|-|
|array\_except\(x, y\) → array|Obtain the subtraction of arrays x and y.|-|
|array\_join\(x, delimiter, null\_replacement\) → varchar|Join string arrays with the delimiter into a string and replace null values with null\_replacement.|-|
|array\_max\(x\) → x|Obtain the maximum value in array x.| |
|array\_min\(x\) → x|Obtain the minimum value in array x.|-|
|array\_position\(x, element\) → bigint|Obtain the subscript of the element in array x.  The subscript starts from 1. 0 is returned if no subscript is found.|-|
|Array\_remove \(x, element\)-array|Remove the element from the array.|-|
|array\_sort\(x\) → array|Sort the array and move null values to the end.|-|
|cardinality\(x\) → bigint|Obtain the array size.|-|
|concat\(array1, array2, …, arrayN\) → array|Concatenate arrays.|-|
|contains\(x, element\) → boolean|Returns TRUE if array x contains the element.|-|
|This is a Lambda function. See filter\(\) in Lambda.|Concatenate a two-dimensional array into a one-dimensional array.|-|
|flatten\(x\) → array|Concatenate a two-dimensional array into a one-dimensional array.|-|
|reduce\(array, initialState, inputFunction, outputFunction\) → x|See function reduce\(\) in [Lambda functions](intl.en-US/User Guide/Real-time analysis/Analysis grammar/Lambda functions.md).|-|
|reverse\(x\) → array|Sort array x in reverse order.|-|
|sequence\(start, stop\) → array|Generate a sequence from start to stop and increment each step by 1.|-|
|sequence\(start, stop, step\) → array|Generate a sequence from start to stop and increment each step by the specified step value.|-|
|sequence\(start, stop, step\) → array|Generate a timestamp array from start to stop. Start and stop are of the timestamp type.  Step is of the interval type, which can be from DAY to SECOND, and can also be YEAR or MONTH.|-|
|shuffle\(x\) → array|Shuffle the array.|-|
|slice\(x, start, length\) → array|Create a new array with length elements from start in array x.|-|
|transform\(array, function\) → array|See transform\(\) in [Lambda functions](intl.en-US/User Guide/Real-time analysis/Analysis grammar/Lambda functions.md).|-|
|zip\(array1, array2\[, …\]\) → array|Merge multiple arrays.  In the result, the Nth parameter in the Mth element is the Mth element in the Nth original array, which is equivalent to transposing multiple arrays.| `SELECT zip(ARRAY[1, 2], ARRAY[‘1b’, null, ‘3b’]); — [ROW(1, ‘1b’), ROW(2, null), ROW(null, ‘3b’)]` |
|zip\_with\(array1, array2, function\) → array|See zip\_with\(\) in Lambda.|-|

