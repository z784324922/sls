# Use JDBC to count and visualize logs {#concept_mfk_snq_zdb .concept}

MySQL is a popular relational database. Many softwares support obtaining MySQL data by using MySQL transport protocol and SQL syntax.  You can connect to MySQL if you know SQL syntax. Log Service provides MySQL protocol to query and analyze logs. You can use a standard MySQL client to connect to Log Service and use the standard SQL syntax to compute and analyze logs. Clients that support the MySQL transport protocol include MySQL client,  JDBC, and Python MySQLdb.

Using bike sharing logs as an example, the following section describes how to use JDBC to connect to Log Service and read log data, the MySQL protocol and SQL syntax to compute logs, and DataV to visualize log data or computation results on a big screen.

**JDBC scenarios:**

-   Use a visualization tool such as DataV, Tableau, or Kibana to connect to Log Service by using the MySQL protocol.
-   Use libraries such as JDBC in Java or MySQLdb in Python to access Log Service and process query results in the program.

## Data example {#section_nqv_skq_12b .section}

A bike sharing log contains your age, gender, battery usage, vehicle ID, operation latency, latitude, lock type, longitude, operation type, operation result, and unlocking method. Data is stored in `Logstore:ebike` of project:`project:trip_demo` . The region where the project resides is cn-hangzhou.

The log sample is as follows:

```
Time :10-12 14:26:44
__source__: 11.164.232.105 
__topic__: v1 
age: 55 
battery: 118497.673842 
bikeid: 36 
gender: male 
latency: 17 
latitude: 30.2931185245 
lock_type: smart_lock 
longitude: 120.052840484 
op: unlock 
op_result: ok 
open_lock: bluetooth 
userid: 292
```

## Prerequisite  {#section_fnf_wkq_12b .section}

To use the index and analysis functions of logs, enable the functions for each column of Logstore in the console or by using APIs.

## JDBC statistics {#section_s15_skq_12b .section}

1.  Create a Maven project and add JDBC dependency in pom dependency.

    ```
    <dependency>
     <groupId>MySQL</groupId>
     <artifactId>mysql-connector-java</artifactId>
     <version>5.1.6</version>
    </dependency>
    ```

2.  Create a Java class and use JDBC in code for query.

    ```
    
    * Created by mayunlei on 2017/6/19.
    
    import com.mysql.jdbc.*;
    import java.sql.*;
    import java.sql.Connection;
    import java.sql.Statement;
    
    * Created by mayunlei on 2017/6/15.
    
    public class jdbc {
     public static void main(String args[]){
                //Modify to your configuration here.
              final String endpoint = "cn-hangzhou-intranet.sls.aliyuncs.com";//The domain name of Log Service intranet or Virtual Private Cloud (VPC).
              final String port = "10005"; //The MySQL protocol port of Log Service.
         final String project = "trip-demo";
         Final string logstore = "ebike ";
         final String accessKeyId = "";
         final String accessKey = "";
         Connection conn = null;
         Statement stmt = null;
         try {
                      //Step 1: Load the JDBC driver.
             Class.forName("com.mysql.jdbc.Driver");
             //Step 2: Create a link.
             conn = DriverManager.getConnection("jdbc:mysql://"+endpoint+":"+port+"/"+project,accessKeyId,accessKey);
                      //Step 3: Create a statement.
             stmt = conn.createStatement();
                      //Step 4: Define query statements. Query the number of logs that are generated on October 11, 2017 and meet the condition op = "unlock", and query the average operation latency.
             String sql = "select count(1) as pv,avg(latency) as avg_latency from "+logstore+" " +
                     "where __date__ >= '2017-10-11 00:00:00' " +
                     " and __date__ < '2017-10-12 00:00:00'" +
                     " and op ='unlock'";
                      //Step 5: Run query conditions.
             ResultSet rs = stmt.executeQuery(sql);
                      //Step 6: Extract the query result.
             while(rs.next()){
                 //Retrieve by column name
                 System.out.print("pv:");
                             //Obtain pv from the result.
                 System.out.print(rs.getLong("pv"));
                 System.out.print(" ; avg_latency:");
                 // Get avg_latency in results
                 System.out.println(rs.getDouble("avg_latency"));
                 System.out.println();
             
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
                 
             
             if (conn ! = null) {
                 try {
                     conn.close();
                 } catch (SQLException e) {
                     e.printStackTrace();
                 
             
         
     
    
    ```


## Use DavaV to access and display data {#section_ssb_blq_12b .section}

Visualized big screen DataV displays data and connects to Log Service to read log data or display log computation results.

1.  Create data sources

    You can select MySQL for  RDS or Log Service as a data source as per your needs.  The following section uses the MySQL protocol as an example to describe how to connect to Log Service.

    As shown in the figure, select the corresponding region and the intranet, and enter an AccessKey for the username and password. The AccessKey can be of a main account or a sub-account that has the read permission to Log Service.  Enter 10005 in the Port field and enter the project name in the Database field.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13158/5884_en-US.png "Editing data")

2.  Creates a view.

    Select a business template for the view and click any view on the big screen. Modify the data and the data source of the view on the right.

    As shown in the figure, set the data source type to Database, select the data source log\_analytics created in the previous step, and enter the SQL statement for query in the SQL field. Enter the mapping between query results and view fields under Field Mapping.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13158/5887_en-US.png "Select a database")

3.  Preview the view and publish.![](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/pic/60618/cn_zh/1508223950493/%E9%A2%84%E8%A7%88%E8%A7%86%E5%9B%BE.png)

    Click Preview to view the preview effect.

     

    ![](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/pic/60618/cn_zh/1508223967228/%E9%A2%84%E8%A7%88%E8%A7%86%E5%9B%BE-1.png)


