# 关联MySQL数据源 {#concept_gbk_qrt_q2b .concept}

日志服务查询分析引擎，提供跨LogStore和ExternalStore的查询分析功能，使用SQL的join语法把日志和用户元信息关联起来。用户可以用来分析跟用户属性相关的指标。

除在查询过程中引用ExternalStore之外，日志服务还支持将计算结果直接写入ExternalStore中（例如MySQL），方便结果的进一步处理。

创建外部MySQL存储的最佳实践：[数据库与日志关联分析](https://yq.aliyun.com/articles/594996)。

## 配置方式 {#section_blb_qwt_q2b .section}

-   外部存储API

    API支持创建、更新、删除、列表等接口，具体API请参考API文档。

-   外部存储管理工具

    外部存储的操作可以通过Python SDK或者CLI（命令行工具）完成。CLI请参考[CLI文档](https://help.aliyun.com/document_detail/65384.html?spm=a2c4g.11186623.6.858.TorOkX)。


## 操作步骤 {#section_gqf_pwt_q2b .section}

1.  采集日志到日志服务。

    请参考[采集方式](intl.zh-CN/用户指南/数据采集/采集方式.md)，选择对应方式采集日志到日志服务。

2.  连接到MSQL数据库，并创建MySQL表。
3.  添加白名单。

    请参考[设置白名单](../../../../intl.zh-CN/用户指南/数据安全性/设置白名单.md)设置RDS白名单：`100.104.0.0/16`。

    如果是普通MySQL数据库，请添加该地址到安全组。

4.  创建ExternalStore。

    您可以选择多种方式创建ExternalStore，此处以Python CLI方式为例。

    1.  执行以下命令安装CLI。

        ```
        pip install -U aliyun-log-cli
        ```

    2.  创建ExternalStore，指定所属的Project，以及ExternalStore的配置文件/root/config.json。

        ```
        aliyunlog log create_external_store --project_name="log-rds-demo" --config="file:///root/config.json" 
        ```

        在配置文件/root/config.json中，指定外部存储的名称，以及外部存储的参数等信息。

        **说明：** 

        -   如果是RDS VPC环境，则必须填写vpc-id和instance-id。
        -   如果是普通的MySQL或者经典网络RDS，则vpc-ic和instance-id填写空字符串。
        -   目前仅支持部分region的 RDS VPC。支持的region请参考[简介](intl.zh-CN/用户指南/查询与分析/关联外部数据源/简介.md)。如果您有其他Region的使用需求，请提供单申请。
        配置文件参数：

        |参数|说明|
        |:-|:-|
        |region|您的服务所在区域。|
        |vpc-id|VPC的ID。|
        |instance-id|RDS实例ID。|
        |host|ECS实例ID。|
        |port|ECS实例端口。|
        |username|用户名。|
        |password|密码。|
        |db|数据库。|
        |table|数据表。|

        配置文件示例：

        ```
        {
            "externalStoreName": "chiji_user",
            "storeType": "rds-vpc",
            "parameter": {
                "vpc-id": "vpc-*********************",
                "instance-id": "rm-**"**************,
                "host": "rm-*****************.mysql.rds.aliyuncs.com",
                "port": "3306",
                "username": "test****",
                "password": "1234****",
                "db": "chiji",
                "table": "chiji_user",
                "region": "cn-qingdao"
            }
        }
        ```

5.  后续操作。

    您还可以通过CLI来更新或删除外部存储。

    -   更新MySQL外部存储：

        ```
        aliyunlog log update_external_store --project_name="log-rds-demo" --config="file:///root/config.json" 
        ```

    -   删除MySQL外部存储：

        ```
        aliyunlog log delete_external_store --project_name="log-rds-demo" --store_name=abc
        ```


