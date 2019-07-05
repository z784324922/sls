# Log analysis through JDBC {#concept_odt_smq_zdb .concept}

In addition to [Overview](../../../../reseller.en-US/API Reference/Overview.md), you can use Java Database Connectivity \(JDBC\) and standard SQL-92 to query and analyze logs.

## Connection parameters {#section_cd5_ngk_5cb .section}

|Parameter|Example|Description|
|:--------|:------|:----------|
|host|regionid.example.com|[Service endpoint](../../../../reseller.en-US/API Reference/Service endpoint.md)The service endpoint. Currently, you can access Log Service through the intranet from a classic network or from Virtual Private Cloud \(VPC\).|
|port|10005|The port number. Default value: 10005.|
|user|bq2sjzesjmo86kq|The [AccessKey ID](../../../../reseller.en-US/API Reference/AccessKey.md).|
|password|4fdO1fTDDuZP|The [AccessKey Secret](../../../../reseller.en-US/API Reference/AccessKey.md).|
|database|sample-project|The [project](../../../../reseller.en-US/Product Introduction/Basic concepts/Project .md) under your account.|
|table|sample-logstore|The [Logstore](../../../../reseller.en-US/Product Introduction/Basic concepts/Logstore.md) under the project.|

For example, use a MySQL command to connect to Log Service as follows:

```
mysql -hcn-shanghai-intranet.log.aliyuncs.com -ubq2sjzesjmo86kq -p4fdO1fTDDuZP -P10005
use sample-project; // Uses a project.
```

## Prerequisites {#section_yvl_vgk_5cb .section}

-   The AccessKey of an Alibaba Cloud account or a RAM user is obtained to access JDBC. The RAM user belongs to the project owner and has the permission to read data from the project.
-   MySQL does not support pagination for queries connected through JDBC.

## Syntax {#section_pbb_wgk_5cb .section}

**Precautions**

A WHERE clause must contain `__date__` or `__time__` to limit the time range of a query. The data type of `__date__` is timestamp, and the data type of `__time__` is bigint.

For example:

-   `__date__ > '2017-08-07 00:00:00' and __date__ < '2017-08-08 00:00:00'`
-   `__time__ > 1502691923 and __time__ < 1502692923`

A WHERE clause must contain at least one of the preceding conditions.

**Filter syntax**

The following table describes the filter syntax in a WHERE clause.

|Semantics|Example|Description|
|:--------|:------|:----------|
|String search|`key = "value"`|Queries data after word-breaking.|
|String fuzzy search|`key = "valu*"`|Queries data in fuzzy match mode after word-breaking.|
|Value comparison|`num_field > 1`|Supports comparison operators such as `>`, `>=`, `=`, `<`, and `<=`.|
|Logical operation|`and or not`|For example, `a = "x" and b ="y"` or `a = "x" and not b ="y"`.|
|Full-text search|`__line__ ="abc"`|Requires the special key \(`__line__`\).|

**Computation syntax**

For more information about supported computation operators, see the syntax description in [Real-time analysis](reseller.en-US/User Guide/Index and query/Syntax description.md).

**SQL-92 syntax**

The SQL-92 syntax is a combination of filter syntax and computation syntax.

The following query is used as an example:

```
status>200 |select avg(latency),max(latency) ,count(1) as c GROUP BY  method  ORDER BY c DESC  LIMIT 20
```

According to the standard SQL-92 syntax, the filter and the time condition in the query can be combined into a new query condition as follows:

```
select avg(latency),max(latency) ,count(1) as c from sample-logstore where status>200 and __time__>=1500975424 and __time__ < 1501035044 GROUP BY  method  ORDER BY c DESC  LIMIT 20
```

## Access Log Service through JDBC {#section_lmx_qhk_5cb .section}

**Application call**

You can use the MySQL syntax to connect to Log Service in any application that supports MySQL Connector. For example, you can use JDBC or Python MySQLdb.

Example:

```
import com.mysql.jdbc.*;
import java.sql.*;
import java.sql.Connection;
import java.sql.ResultSetMetaData;
import java.sql.Statement;
public class testjdbc {
    public static void main(String args[]){
        Connection conn = null;
        Statement stmt = null;
        try {
            // Step 2: Register the JDBC driver.
            Class.forName("com.mysql.jdbc.Driver");
            // Step 3: Establish a connection.
            System.out.println("Connecting to a selected database...");
            conn = DriverManager.getConnection("jdbc:mysql://cn-shanghai-intranet.log.aliyuncs.com:10005/sample-project","accessid","accesskey");
            System.out.println("Connected database successfully...") ;
            // Step 4: Start a query.
            System.out.println("Creating statement...");
            stmt = conn.createStatement();
            String sql = "SELECT method,min(latency,10)  as c,max(latency,10) from sample-logstore where  __time__>=1500975424 and __time__ < 1501035044 and latency > 0  and latency < 6142629 and  not (method='Postlogstorelogs' or method='GetLogtailConfig') group by method " ;
            String sql-example2 = "select count(1) ,max(latency),avg(latency), histogram(method),histogram(source),histogram(status),histogram(clientip),histogram(__source__) from  test10 where __date__  >'2017-07-20 00:00:00'  and  __date__ <'2017-08-02 00:00:00' and __line__='abc#def' and latency < 100000 and (method = 'getlogstorelogS' or method='Get**' and method <> 'GetCursorOrData' )";
            String sql-example3 = "select count(1) from  sample-logstore where     __date__  >       '2017-08-07 00:00:00' and  __date__ <     '2017-08-08 00:00:00' limit 100";
            ResultSet rs = stmt.executeQuery(sql);
            // Step 5: Extract data from the result set.
            while(rs.next()){
                // Retrieves data by column name.
                ResultSetMetaData data = rs.getMetaData();
                System.out.println(data.getColumnCount());
                for(int i = 0;i < data.getColumnCount();++i) {
                    String name = data.getColumnName(i+1);
                    System.out.print(name+":");
                    System.out.print(rs.getObject(name));
                }
                System.out.println();
            }
            rs.close();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (SQLException e) {
            e.printStackTrace();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            if (stmt ! = null) {
                try {
                    stmt.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (conn ! = null) {
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

**Tool call**

On a classic network or in VPC, you can use the MySQL client to connect to Log Service.

**Note:** 

1.  Enter your project name at 1.
2.  Enter your Logstore name at 2.

