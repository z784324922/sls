# Optimize query for analysis {#concept_vv5_qmq_zdb .concept}

The analysis efficiency varies from query to query. Common ways to optimize the query are as follows for your references:

## Avoid running Group By on string columns if possible {#section_nzh_51y_zdb .section}

Running Group By on strings leads to a large amount of hash calculations, which usually accounts for more than 50% of total calculations.

For example:

```
* | select count(1) as pv , date_trunc('hour',__time__) as time group by time
* | select count(1) as pv , from_unixtime(__time__-__time__%3600) as time group by __time__-__time__%3600
```

Both Query 1 and Query 2 calculate the log count value every hour. However, Query 1 converts time into a string, for example, `2017-12-12 00:00:00`, and then runs Group By on this string.  Query 2 calculates the on-the-hour time value, runs Group  By on the result, and then converts the value into a string.  Query 1 is less efficient than Query 2 because the former one needs to hash strings.

## List fields with relatively large dictionary values on top when running Group By on multiple columns {#section_lds_v1y_zdb .section}

For example, 13 provinces have 100 million users.

```
Fast: * | select province,uid,count(1) group by province,uid
Slow: * | select province,uid,count(1) group by uid,province
```

## Estimating functions  {#section_jtx_v1y_zdb .section}

provide much stronger performance than accurate calculation.  Estimation sacrifices some acceptable accuracy for fast calculation.

```
Fast: * | select approx_distinct(ip) 
Slow: * | select count(distinct(ip))
```

## Retrieve required columns in SQL and do not read all columns if possible {#section_cdb_w1y_zdb .section}

Use the query syntax to retrieve all columns.  To speed up calculation, retrieve only the required columns in SQL if possible.

```
Fast: * |select a,b c  
Slow: * |select *
```

## Non-group by columns, as far as possible in aggregate Functions {#section_kzh_51y_zdb .section}

For example, userid, user name, must be one corresponding, we just need to follow userid for group.

```
Fast:  * | select userid, arbitrary(username), count(1)groupby userid
Slow: * | select userid, username, count(1)groupby userid,username
```

