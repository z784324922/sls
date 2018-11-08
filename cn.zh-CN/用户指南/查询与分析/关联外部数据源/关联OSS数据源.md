# 关联OSS数据源 {#concept_fgy_nrt_q2b .concept}

日志服务支持通过外部存储的方式完成OSS和日志服务联合查询，即在查询中将OSS数据与日志服务数据同时作为数据源。

## 功能优势 {#section_pvn_fd5_q2b .section}

-   节约存储成本。对于异构数据，可以根据数据的特性选择合适的存储系统，最大限度的节省成本。 对于更新少的数据，选择存放在OSS上，只需要支付少量的存储费用。如果存放在MySQL上，还要支付计算实例的费用。
-   节约流量费用。OSS是阿里云的存储系统，可以走内网读取数据，免去了流量费用。

## 前提条件 {#section_fyw_nd5_q2b .section}

-   已开通日志服务，并创建了Project和Logstore。

    日志服务准备流程请参考[准备流程](intl.zh-CN/用户指南/准备工作/准备流程.md)。

-   已开通OSS，并创建了存储空间。

    详细步骤请参考[创建存储空间](../../../../intl.zh-CN/快速入门/创建存储空间.md)。


## 配置步骤 {#section_obn_3d5_q2b .section}

1.  登录OSS控制台，并上传CSV格式文件到OSS。

    文件上传步骤请参考[上传文件](../../../../intl.zh-CN/快速入门/上传文件.md)。

2.  在日志服务中定义外部存储。

    在日志服务查询页面的查询框中输入相应SQL语句，通过SQL定义虚拟外部表，映射到OSS文件。执行结果result为true，表示执行成功。

    在SQL语句中您需要定义外部存储名称、表的schema等信息，并通过with语法指定OSS访问信息及文件信息。

    |项目|参数|说明|
    |:-|:-|:-|
    |外部存储名称|tableName|外部存储名称，即虚拟表的名称。|
    |表的schema|包括表的列名及格式。|定义表的属性。|
    |OSS访问信息|endpoint|地域。|
    |accessid|您的AccessKeyID。|
    |accesskey|您的AccessKey Secret。|
    |OSS文件信息|bucket|CSV文件所在的OSS Bucket名称。|
    |objects|CSV文件的路径。**说明：** objects为array类型，可以包含多个OSS文件。

|
    |type|取值固定为oss，表示外部存储类型为OSS。|

    例如，通过SQL语句定义一个外部存储，名为tableName：

    ```
    * | create table tableName ( userid bigint, nick varchar, gender varchar, province varchar, gender varchar,age bigint) with ( endpoint='oss-cn-hangzhou-internal.aliyuncs.com',accessid='**************************',accesskey ='****************************',bucket='testoss*********',objects=ARRAY['user.csv'],type='oss')
    ```

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17023/15416439388538_zh-CN.png)

    在上例中，分别指定了表的属性：

    -   userid为 bigint类型。
    -   nick为varchar类型。
    -   gender为varchar类型。
    -   province为varchar类型。
    -   age为bigint类型。
3.  验证是否已成功定义外部存储。

    执行SQL`select * from user_meta`，查看返回结果是否为您之前定义的表内容。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17023/15416439388539_zh-CN.png)

4.  通过JOIN语法完成OSS文件和日志服务数据的联合查询。

    在日志服务查询页面，查询框中输入SQL语法，使用JOIN语法，引用OSS外部存储。

    例如，在查询中引用user\_meta表，通过关联日志中的id和oss文件中的userid，补全日志的信息。

    `* | select * from chiji_accesslog l join user_meta u on l.userid = u.userid`

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17023/15416439388540_zh-CN.png)


