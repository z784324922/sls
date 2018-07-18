# Estimating functions {#LogService_user_guide_0105 .reference}

The query and analysis function of Log Service supports analyzing logs by using estimating functions. The specific statements and meanings are as follows.

|Statement|Meaning|Example |
|:--------|:------|:-------|
|`approx_distinct(x)`|Estimates the number of unique values in column x.|-|
|`approx_percentile(x,percentage)`|Sorts the column x and returns the value approximately at the given percentage position.|Returns the value at the half position: `approx_percentile(x,0.5)`|
|`approx_percentile(x, percentages)`|Similar to the preceding statement, but you can specify multiple percentages to return the values at each specified percentage position.|`approx_percentile(x,array[0.1,0.2])`|
|`numeric_histogram(buckets, Value)`|Makes statistics on the value column in different buckets.  Divides the value column into buckets number of buckets and returns the key and count of each bucket, which is equivalent to `select  count group by` for values.|For post requests, divide the delay into 10 barrels, returns the size of each bucket: method: `method:POST | select numeric_histogram(10,latency)`|

