# 对接JDBC {#concept_mfk_snq_zdb .concept}

MySQL是当前流行的关系型数据库，很多软件支持通过MySQL传输协议和SQL语法获取MySQL数据。用户只需要对SQL语法熟悉，即可完成对接。日志服务提供了MySQL协议查询和分析日志数据。用户可以使用标准MySQL客户端连接到日志服务，使用标准的SQL语法计算和分析日志。支持MySQL传输协议的客户端包括MySQL client，JDBC和Python MySQLdb。

本文以共享单车日志为例，演示如何使用JDBC连接日志服务、读取日志数据，使用MySQL协议和SQL语法来计算日志，并使用DataV完成日志数据或计算结果的大屏可视化。

**JDBC的使用场景:**

-   使用可视化类工具，例如DataV、Tableau或Kibana来通过MySQL协议连接日志服务。
-   使用Java的JDBC、Python的MySQLdb等库在程序中访问日志服务，在程序中处理查询结果。

## 数据样例 {#section_nqv_skq_12b .section}

共享单车日志内容包括用户年龄、性别、电量使用量、车辆ID、操作延时、纬度、锁类型、经度、操作类型、操作结果和开锁方式。数据保存在`project:trip_demo`的`Logstore:ebike`中。Project所在地域是cn-hangzhou。

日志样例如下：

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

## 前提条件 {#section_fnf_wkq_12b .section}

如需使用日志索引和分析功能，请先通过控制台或API为Logstore的每一列开启索引和分析功能。

## JDBC统计 {#section_s15_skq_12b .section}

1.  创建一个maven项目，在pom依赖中添加JDBC依赖。

    ```
    <dependency>
     <groupId>MySQL</groupId>
     <artifactId>mysql-connector-java</artifactId>
     <version>5.1.6</version>
    </dependency>
    ```

2.  新建一个java类，在代码中使用JDBC进行查询。

    ```
    /**
    * Created by mayunlei on 2017/6/19.
    */
    import com.mysql.jdbc.*;
    import java.sql.*;
    import java.sql.Connection;
    import java.sql.Statement;
    /**
    * Created by mayunlei on 2017/6/15.
    */
    public class jdbc {
     public static void main(String args[]){
          //在这里修改成你的配置
         final String endpoint = "cn-hangzhou-intranet.sls.aliyuncs.com";//日志服务内网或VPC域名
         final String port = "10005"; //日志服务MySQL 协议端口
         final String project = "trip-demo";
         final String logstore = "ebike";
         final String accessKeyId = "";
         final String accessKey = "";
         Connection conn = null;
         Statement stmt = null;
         try {
             //步骤1 ： 加载JDBC驱动
             Class.forName("com.mysql.jdbc.Driver");
             //步骤2 ： 创建一个链接
             conn = DriverManager.getConnection("jdbc:mysql://"+endpoint+":"+port+"/"+project,accessKeyId,accessKey);
             //步骤3 ： 创建statement
             stmt = conn.createStatement();
             //步骤4 ： 定义查询语句，查询2017年10月11日全天日志中满足条件op = "unlock"的日志条数，操作平均延时
             String sql = "select count(1) as pv,avg(latency) as avg_latency from "+logstore+"  " +
                     "where     __date__  >=  '2017-10-11 00:00:00'   " +
                     "     and  __date__  <   '2017-10-12 00:00:00'" +
                     " and     op ='unlock'";
             //步骤5 ： 执行查询条件
             ResultSet rs = stmt.executeQuery(sql);
             //步骤5 ： 提取查询结果
             while(rs.next()){
                 //Retrieve by column name
                 System.out.print("pv:");
                 //获取结果中的pv
                 System.out.print(rs.getLong("pv"));
                 System.out.print(" ; avg_latency:");
                 //获取结果中的avg_latency
                 System.out.println(rs.getDouble("avg_latency"));
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
             if (stmt != null) {
                 try {
                     stmt.close();
                 } catch (SQLException e) {
                     e.printStackTrace();
                 }
             }
             if (conn != null) {
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


## 使用DavaV连接展示数据 {#section_ssb_blq_12b .section}

可视化大屏DataV提供数据的展示功能，可以对接日志服务读取日志数据或展示日志计算结果。

1.  创建数据源。

    数据源可以选择MySQL for RDS或者日志服务，根据自己的需求选择对应的方式。本文以MySQL协议为例，展示如何接入。

    如图所示，选择对应的地域，网络选择内网，用户名和密码填写AccessKey，可以是主账号的AccessKey，也可以是有权限读取日志服务的子帐号AccessKey。端口输入10005，数据库输入Project名称。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13158/5884_zh-CN.png "编辑数据")

2.  创建视图。

    视图中选择好业务的模板，然后单击大屏中的任何一个视图，右侧单击修改数据，修改视图的数据源。

    如图，数据源选择上文创建的数据库，输入查询的SQL，在上边的字段映射中，输入查询结果和视图字段的映射关系。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13158/5887_zh-CN.png "选择数据库")

3.  预览视图并发布。![](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/pic/60618/cn_zh/1508223950493/%E9%A2%84%E8%A7%88%E8%A7%86%E5%9B%BE.png)

    单击预览，可以查看预览效果。

     

    ![](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/pic/60618/cn_zh/1508223967228/%E9%A2%84%E8%A7%88%E8%A7%86%E5%9B%BE-1.png)


