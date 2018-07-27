# General aggregate functions {#LogService_user_guide_0103 .reference}

The query and analysis function of Log Service supports analyzing logs by using general aggregate functions. The specific statements and meanings are as follows.

|Statement|Meaning|Example|
|:--------|:------|:------|
|`arbitrary(x)`|Returns a value in column x randomly.|`latency > 100 | select arbitrary(method)`|
|`avg(x)`|Calculates the arithmetic mean of all the values in column x.|`latency > 100 | select avg(latency)`|
|`checksum(x)`|Calculates the checksum of all the values in a column and returns the base64-encoded value.|`latency > 100 | select checksum(method)`|
|`count(*)`|Calculates the number of rows in a column.|-|
|`count(x)`|Calculates the number of non-null values in a column.|`latency > 100 | count(method)`|
|`count_if (X)`|Calculates the number of X = true.|`latency > 100 | count(url like ‘%abc’)`|
|`geometric_mean(x)`|Calculates the geometric mean of all the values in a column.|`latency > 100 | select geometric_mean(latency)`|
|`max_by(x,y)`|Returns the value of column x when column y has the maximum value.|The method for the maximum latency: `latency>100 | select max_by(method,latency)` |
|`max_by(x,y,n)`|Returns the values of column x corresponding to the maximum n rows of column y.|The method for the top 3 rows with maximum latency: `latency > 100 | select max_by(method,latency,3)` |
|`min_by(x,y)`|Returns the value of column x when column y has the minimum value.|The method for the minimum latency:  `* | select min_by(x,y)`|
|`min_by (X, Y, n)`|Returns the value of column x when column y has the minimum value.|The method for the minimum latency: `* | select min_by(method,latency,3)`|
|`max(x)`|Returns the maximum value.|`latency > 100| select max(inflow)`|
|`min(x)`|Returns the minimum value|`latency > 100| select min(inflow)`|
|`sum(x)`|Returns the sum of all the values in column x.|`latency > 10 | select sum(inflow)`|
|`bitwise_and_agg(x)`|Do the AND calculation to all the values in a column.|-|
|`bitwise_or_agg(x)`|Do the OR calculation to all the values in a column.|-|

