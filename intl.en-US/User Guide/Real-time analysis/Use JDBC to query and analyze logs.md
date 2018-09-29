# Use JDBC to query and analyze logs {#concept_odt_smq_zdb .concept}

In addition to [Overview](../../../../intl.en-US/API Reference/Overview.md), you can use JDBC and standard SQL 92 for log query and analysis.

## Connection parameters {#section_cd5_ngk_5cb .section}

|Connection parameter|Example|Description|
|:-------------------|:------|:----------|
|host|regionid.example.com|[Service endpoint](../../../../intl.en-US/API Reference/Service endpoint.md)The access point, Currently, only the intranet access of classic network and Virtual Private Cloud \(VPC\) access are supported.|
|port|10005|Use 10005 as the port by default.|
|user|bq2sjzesjmo86kq|The [AccessKey ID](../../../../intl.en-US/API Reference/AccessKey.md) .|
|password|4fdO1fTDDuZP|The [AccessKey Secret](../../../../intl.en-US/API Reference/AccessKey.md).|
|database|sample-project|The [project](../../../../intl.en-US/Product Introduction/Basic concepts/Project .md) under your account.|
|table|sample-logstore|The [Logstore](../../../../intl.en-US/Product Introduction/Basic concepts/Logstore.md) under project.|

For example, use a MySQL command to connect to Log Service as follows:

```
mysql -hcn-shanghai-intranet.log.aliyuncs.com -ubq2sjzesjmo86kq -p4fdO1fTDDuZP -P10005
use sample-project; //Use a project.
```

## Prerequisites {#section_yvl_vgk_5cb .section}

You must use the AccessKey of the main account or a sub-account to access the JDBC interface. The sub-account must belong to the project owner and have the project-level read permission.

## Syntax description {#section_pbb_wgk_5cb .section}

**Instructions**

The WHERE condition must contain `__date__`or `__time__` to limit the time range of query. The type of `__date__` is timestamp, and the type of `__time__` is bigint.

Example:

-   `__date__ > '2017-08-07 00:00:00' and __date__ < '2017-08-08 00:00:00'`
-   `__time__ > 1502691923 and __time__ < 1502692923`

At least one of the preceding conditions must be contained.

**Filter syntax**

The filter syntax in the WHERE condition is as follows:

|Meaning|Example|Description|
|:------|:------|:----------|
|String search|`key = "value"`|Results after word segmentation are queried.|
|String fuzzy search|`key = "valu*"`|Results of fuzzy match after word segmentation are queried.|
|Value comparison|`num_field > 1`|Comparison operators including `>`, `>=`, `=`, `<` and `<=` are supported.|
|Logic operations|`and or not`|For example, `a = "x" and b ="y"` or `a = "x" and not b ="y"`.|
|Full-text search|`__line__ ="abc"`|Full-text index search requires the special key \(`__line__`\).|

**Computation syntax**

For supported computation operators, see [Analysis syntax](intl.en-US/User Guide/Real-time analysis/Syntax description.md).

**SQL92 syntax**

The SQL92 syntax is a combination of filter and computation syntaxes.

The following query is used as an example:

```
status>200 |select avg(latency),max(latency) ,count(1) as c GROUP BY method ORDER BY c DESC LIMIT 20
```

The filter part and time condition in the query can be combined into a new query condition based on standard SQL92 syntax.

```
select avg(latency),max(latency) ,count(1) as c from sample-logstore where status>200 and __time__>=1500975424 and __time__ < 1501035044 GROUP BY method ORDER BY c DESC LIMIT 20
```

## Access Log Service by using JDBC protocol {#section_lmx_qhk_5cb .section}

**Program call**

Developers can use the MySQL syntax to connect to Log Service in any program that supports MySQL connector. For example, JDBC or Python MySQLdb can be used.

Example:

```
import com.mysql.jdbc.*;
Import java. SQL .*;
import java.sql.Connection;
import java.sql.ResultSetMetaData;
import java.sql.Statement;
public class testjdbc {
    public static void main(String args[]){
        Connection conn = null;
        Statement stmt = null;
        try {
            //STEP 2: Register JDBC driver
            Class.forName("com.mysql.jdbc.Driver");
            //STEP 3: Open a connection
            System.out.println("Connecting to a selected database...");
            conn = DriverManager.getConnection("jdbc:mysql://cn-shanghai-intranet.log.aliyuncs.com:10005/sample-project","accessid","accesskey");
            System.out.println("Connected database successfully...")
            //STEP 4: Execute a query
            System. Out. println ("creating statement ...");
            stmt = conn.createStatement();
            String sql = "SELECT method,min(latency,10) as c,max(latency,10) from sample-logstore where __time__>=1500975424 and __time__ < 1501035044 and latency > 0 and latency < 6142629 and not (method='Postlogstorelogs' or method='GetLogtailConfig') group by method " ;
            String sql-example2 = "select count(1) ,max(latency),avg(latency), histogram(method),histogram(source),histogram(status),histogram(clientip),histogram(__source__) from test10 where __date__ >'2017-07-20 00:00:00' and __date__ <'2017-08-02 00:00:00' and __line__='abc#def' and latency < 100000 and (method = 'getlogstorelogS' or method='Get**' and method <> 'GetCursorOrData' )";
            String sql-example3 = "select count(1) from sample-logstore where __date__ > '2017-08-07 00:00:00' and __date__ < '2017-08-08 00:00:00' limit 100";
            ResultSet rs = stmt.executeQuery(sql);
            //STEP 5: Extract data from result set
            while(rs.next()){
                //Retrieve by column name
                ResultSetMetaData data = rs.getMetaData();
                System.out.println(data.getColumnCount());
                for(int i = 0;i < data.getColumnCount();++i) {
                    String name = data.getColumnName(i+1);
                    System.out.print(name+":");
                    System.out.print(rs.getObject(name));
                
                System.out.println();
            
            Rs. Close ();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (SQLException e) {
            e.printStackTrace();
        } catch (Exception e) {
            E. printstacktrace ();
        } Finally {
            if (stmt ! = null) {
                try {
                    Stmt. Close ();
                } catch (SQLException e) {
                    e.printStackTrace();
                
            
            if (conn ! = null) {
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                
            
        
    

```

**Tool call**

In the classic network intranet or VPC environment, use the MySQL client to connect to Log Service.

**Note:** 

1.  Enter your project name at ①.
2.  Enter your Logstore name at ②.

