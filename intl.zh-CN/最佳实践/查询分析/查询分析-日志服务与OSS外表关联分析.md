# 查询分析-日志服务与OSS外表关联分析 {#task_otl_z2x_fhb .task}

在日志分析场景中，我们经常遇到日志中的信息不完善。例如，日志中包含了用户的点击行为，但是却缺少用户的属性，例如注册信息、资金等信息。 而分析日志的时候，往往需要联合分析用户的属性和行为，例如分析用户地域对付费习惯的影响等。

日志服务提供的这种跨数据源（OSS）的分析能力，可以帮助用户解决以下问题：

-   节省费用
    -   异构数据，根据数据的特性选择合适的存储系统，最大限度的节省成本。 对于更新少的数据，选择存放在OSS上，只需要支付少量的存储费用。如果存放在MySQL上，还要支付计算实例的费用。
    -   OSS是阿里云的存储系统，可以走内网读取数据，免去了流量费用。
-   节省精力

    在我们轻量级的联合分析平台中，不需要搬迁数据到同一个存储系统中，节省了用户精力。

-   节省时间
    -   当用户需要分析数据时，使用一条SQL，秒级别即可获得结果。
    -   把常用的视图，定义成报表，打开即可看到结果。

1.  上传CSV文件到OSS。 
    1.  定义一份属性文件，包含用户ID、用户昵称、性别、省份、年龄。![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/149429/156894967241528_zh-CN.png)
    2.  保存该文件为`user.csv`，使用osscmd上传到OSS。

        ```
        osscmd   put  ~/user.csv   oss:/testossconnector/user.csv
        ```

2.  定义外部存储。 通过SQL定义一个存储名为user\_meta虚拟外部表。

    ``` {#codeblock_sce_l3u_cyc}
    * | create table user_meta ( userid bigint, nick varchar, gender varchar, province varchar, gender varchar,age bigint) with ( endpoint='example.com',accessid='<youraccessid>',accesskey='<accesskey>',bucket='testossconnector',objects=ARRAY['user.csv'],type='oss')
    ```

    **说明：** 

    -   在SQL中，需要指定三部分信息：
        -   表的schema：包含的列，以及每一列的属性。
        -   OSS访问信息：OSS的域名，accessid 和accesskey。
        -   OSS文件信息：文件所属bucket，以及文件的object路径。
    -   objects是一个数组，可以同时包含多个文件。
    执行结果为true，表示执行成功。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/149429/156894967241543_zh-CN.png)

    执行SQL查看结果：`select * from user_meta` 。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/149429/156894967241545_zh-CN.png)

3.  联合分析。 

    在原始日志中，包含了用户的ID信息，我们可以通过SQL关联日志中的ID和OSS文件中的userid，补全日志的信息。

    ```
    * | select * from chiji_accesslog l join user_meta1 u on l.userid = u.userid
    ```

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/149429/156894967341546_zh-CN.png)

    -   统计用户性别的访问情况：

        ``` {#codeblock_7qa_hc6_fvk}
        * | select u.gender, count(1) from chiji_accesslog l join user_meta1 u on l.userid = u.userid group by u.gender
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/149429/156894967341547_zh-CN.png)

    -   统计用户年龄的访问情况：

        ``` {#codeblock_cr3_c0a_isr}
        * | select u.age, count(1) from chiji_accesslog l join user_meta1 u on l.userid = u.userid group by u.age
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/149429/156894967341550_zh-CN.png)

    -   统计不同年龄段在时间维度上的访问趋势：

        ``` {#codeblock_511_9qw_1am}
        * | selectdate_trunc('minute',__time__) as minute, count(1) ,u.age from chiji_accesslog l join user_meta1 u on l.userid = u.userid group by u.age,minute
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/149429/156894967341551_zh-CN.png)


