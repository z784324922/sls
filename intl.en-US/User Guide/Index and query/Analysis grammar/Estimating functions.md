# Estimating functions {#LogService_user_guide_0105 .reference}

The query and analysis function of Log Service supports analyzing logs by using estimating functions. The specific statements and meanings are as follows.

|Function|Description |Examples|
|:-------|:-----------|:-------|
|`approx_distinct(x)`|Estimates the number of unique values in column x.|-|
|`approx_percentile(x,percentage)`|Sorts the column x and returns the value approximately at the given percentage position.|Returns the value at the half position: `approx_percentile(x,0.5)`|
|`approx_percentile(x, percentages)`|Similar to the preceding statement, but you can specify multiple percentages to return the values at each specified percentage position.|`approx_percentile(x,array[0.1,0.2])`|
|`numeric_histogram(buckets, Value)`|Collects values in the numeric column by bucket. That is, you need to enter the Value column into buckets. The number of buckets is determined by buckets.The returned information is the key of each bucket and the corresponding count. This works similarly to `select count group by` for numbers.

**Note:** Returned results must be in JSON format.

|For POST requests, the delay is divided into 10 buckets. You can run `method:POST | select numeric_histogram(10,latency)` to check the size of each bucket.|
|`numeric_histogram(buckets, Value)`|Collects values in the numeric column by bucket. That is, you need to enter the Value column into buckets. The number of buckets is determined by buckets.The returned information is the key of each bucket and the corresponding count. This works similarly to `select count group by` for numbers.

**Note:** Returned results must be in multi-row multi-column format.

|For POST requests, the delay is divided into 10 buckets. You can run `method:POST | select numeric_histogram(10,latency)` to check the size of each bucket.|

