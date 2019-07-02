# General aggregate functions {#LogService_user_guide_0103 .reference}

The query and analysis feature of Log Service allows you to use general aggregate functions to analyze logs. The following table describes the specific statements.

|Statement|Description|Example|
|:--------|:----------|:------|
|`arbitrary(x)`|Returns an arbitrary value in column x.|`latency > 100 | select arbitrary(method)`|
|`avg(x)`|Calculates the arithmetic mean of all the values in column x.|`latency > 100 | select avg(latency)`|
|`checksum(x)`|Calculates the checksum of all the values in column x and returns a Base64-encoded value.|`latency > 100 | select checksum(method)`|
|`count(*)`|Calculates the number of rows.|N/A|
|`count(x)`|Calculates the number of non-null values in column x.|`latency > 100 | count(method)`|
|`count(digit)`|Functions the same as `count(*)` to calculate the number of rows. ``For example, `count(1)`.|N/A|
|`count_if(x)`|Calculates the number of true values.|`latency > 100 | count_if(url like ‘%abc’)`|
|`geometric_mean(x)`|Calculates the geometric mean of all the values in column x.|`latency > 100 | select geometric_mean(latency)`|
|`max_by(x,y)`|Returns the value of x associated with the maximum value of y.|To query the method for the maximum latency: `latency>100 | select max_by(method,latency)` |
|`max_by(x,y,n)`|Returns the values of x associated with the n largest values of y in descending order of y.|To query the method for the top three rows with the maximum latency: `latency > 100 | select max_by(method,latency,3)` |
|`min_by(x,y)`|Returns the value of x associated with the minimum value of y.|To query the method for the minimum latency: `* | select min_by(method,latency)`|
|`min_by(x,y,n)`|Returns the values of x associated with the n smallest values of y in ascending order of y.|To query the method for the top three rows with the minimum latency: `* | select min_by(method,latency,3)`|
|`max(x)`|Returns the maximum value of all the values in column x.|`latency > 100| select max(inflow)`|
|`min(x)`|Returns the minimum value of all the values in column x.|`latency > 100| select min(inflow)`|
|`sum(x)`|Returns the sum of all the values in column x.|`latency > 10 | select sum(inflow)`|
|`bitwise_and_agg(x)`|Returns the bitwise AND of all the values in column x in 2's complement representation.|N/A|
|`bitwise_or_agg(x)`|Returns the bitwise OR of all the values in column x in 2's complement representation.|N/A|

