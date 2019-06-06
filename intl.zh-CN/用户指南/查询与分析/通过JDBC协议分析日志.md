# 通过JDBC协议分析日志 {#concept_odt_smq_zdb .concept}

除[概览](../../../../intl.zh-CN/API 参考/概览.md)外，您还可以使用JDBC + 标准SQL 92进行日志查询与分析。

## 连接参数 {#section_cd5_ngk_5cb .section}

|连接参数|示例|说明|
|:---|:-|:-|
|host|regionid.example.com|[服务入口](../../../../intl.zh-CN/API 参考/服务入口.md)，目前仅支持经典网络内网访问和VPC网络访问|
|port|10005|默认使用10005作为端口号|
|user|bq2sjzesjmo86kq|[访问秘钥](../../../../intl.zh-CN/API 参考/访问秘钥.md) AccesskeyId|
|password|4fdO1fTDDuZP|[访问秘钥](../../../../intl.zh-CN/API 参考/访问秘钥.md)Accesskey|
|database|sample-project|账号下的[项目（Project）](../../../../intl.zh-CN/产品简介/基本概念/项目.md)|
|table|sample-logstore|项目下的[日志库（Logstore）](../../../../intl.zh-CN/产品简介/基本概念/日志库.md)|

例如通过MySQL命令连接示例如下：

```
mysql -hcn-shanghai-intranet.log.aliyuncs.com -ubq2sjzesjmo86kq -p4fdO1fTDDuZP -P10005
use sample-project; // 使用某个Project
```

## 前提条件 {#section_yvl_vgk_5cb .section}

-   访问JDBC接口，必须使用主账号的AK或者子帐号的AK。子帐号必须是Project owner的子帐号，同时子帐号具有Project级别的读权限。
-   MySQL JDBC不支持分页。

## 语法说明 {#section_pbb_wgk_5cb .section}

**注意事项**

在where条件中必须包含`__date__`或`__time__`来限制查询的时间范围。`__date__`是timestamp类型`__time__`是bigint类型。

例如：

-   `__date__ > '2017-08-07 00:00:00' and __date__ < '2017-08-08 00:00:00'`
-   `__time__ > 1502691923 and __time__ < 1502692923`

上述两种条件必须出现一个。

**过滤语法**

关于where下过滤（filter）语法如下：

|语义|示例|说明|
|:-|:-|:-|
|字符串搜索|`key = "value"`|查询的是分词之后的结果。|
|字符串模糊搜索|`key = "valu*"`|查询的是分词之后模糊匹配的结果。|
|数值比较|`num_field > 1`|支持的比较运算符包括`>`、 `>=`、 `=`、 `<`和`<=`。|
|逻辑运算|`and or not`|例如`a = "x" and b ="y"`或`a = "x" and not b ="y"`。|
|全文搜索|`__line__ ="abc"`|如果使用全文索引搜索，需要使用特殊的key（`__line__`）。|

**计算语法**

支持计算操作符参见[分析语法](intl.zh-CN/用户指南/查询与分析/实时分析简介.md)。

**SQL92语法**

过滤 + 计算组合为SQL92语法。

例如对于如下查询：

```
status>200 |select avg(latency),max(latency) ,count(1) as c GROUP BY  method  ORDER BY c DESC  LIMIT 20
```

我们可以将查询中过滤部分+ 时间条件组合成为查询的条件，变成标准SQL92语法：

```
select avg(latency),max(latency) ,count(1) as c from sample-logstore where status>200 and __time__>=1500975424 and __time__ < 1501035044 GROUP BY  method  ORDER BY c DESC  LIMIT 20
```

## 通过JDBC协议访问 {#section_lmx_qhk_5cb .section}

**程序调用**

开发者可以在任何一个支持MySQL connector的程序中使用MySQL语法连接日志服务。例如使用JDBC或者Python MySQLdb。

使用样例：

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
            //STEP 2: Register JDBC driver
            Class.forName("com.mysql.jdbc.Driver");
            //STEP 3: Open a connection
            System.out.println("Connecting to a selected database...");
            conn = DriverManager.getConnection("jdbc:mysql://cn-shanghai-intranet.log.aliyuncs.com:10005/sample-project","accessid","accesskey");
            System.out.println("Connected database successfully...");
            //STEP 4: Execute a query
            System.out.println("Creating statement...");
            stmt = conn.createStatement();
            String sql = "SELECT method,min(latency,10)  as c,max(latency,10) from sample-logstore where  __time__>=1500975424 and __time__ < 1501035044 and latency > 0  and latency < 6142629 and  not (method='Postlogstorelogs' or method='GetLogtailConfig') group by method " ;
            String sql-example2 = "select count(1) ,max(latency),avg(latency), histogram(method),histogram(source),histogram(status),histogram(clientip),histogram(__source__) from  test10 where __date__  >'2017-07-20 00:00:00'  and  __date__ <'2017-08-02 00:00:00' and __line__='abc#def' and latency < 100000 and (method = 'getlogstorelogS' or method='Get**' and method <> 'GetCursorOrData' )";
            String sql-example3 = "select count(1) from  sample-logstore where     __date__  >       '2017-08-07 00:00:00' and  __date__ <     '2017-08-08 00:00:00' limit 100";
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

**工具类调用**

在经典网内网/VPC环境通过MySQL Client进行连接。

**说明：** 

1.  ①处填写您的Project。
2.  ②处填写您的Logstore。

