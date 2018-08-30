# External MySQL Store {#concept_gbk_qrt_q2b .concept}

The query analysis engine of Log Service provides query analysis functions across LogStore and ExternalStore, and uses the join syntax of SQL to associate logs with user meta information. You can use the engine to analyze metrics related to user attributes.

In addition to referring to External Store during the query, Log Service also supports writing the results directly to the External Store \(for example, MySQL \) for further processing of the results.

Best practices for creating External MySQL Store: [Database and log association analysis](https://yq.aliyun.com/articles/594996).

## Configuration method {#section_blb_qwt_q2b .section}

-   External Store API

    API supports interfaces such as create, update, delete, and list. For information about specific API, see the API documentation.

-   External Store management tools

    External Store operations can be done either through the python SDK or CLI. For information about CLI, see the [CLI documentation](https://help.aliyun.com/document_detail/65384.html?spm=a2c4g.11186623.6.858.TorOkX).


## Procedure {#section_gqf_pwt_q2b .section}

1.  Collects logs to Log Service.

    See [Collection methods](intl.en-US/User Guide/Data Collection/Collection methods.md) to choose a method to collect logs to Log Service.

2.  Connect to an MSQL database and create a MySQL table.
3.  Add a whitelist

    Please see [Set whitelist](../../../../intl.en-US/User Guide/Security/Set a whitelist.md) to set a whitelist: `100.104.0.0/16`.

    If you connect to a common MySQL database, add the address to the security group.

4.  Create an External Store.

    You can create an External Store by using multiple methods. In this example, the Python CLI method is used.

    1.  Install the CLI by executing the following command.

        ```
        pip install -U aliyun-log-cli
        ```

    2.  Create an External Store. Specify the project to which it belongs, and the configuration file /root/config.json of the External Store.

        ```
        aliyunlog log create_external_store --project_name="log-rds-demo" --config="file:///root/config.json" 
        ```

        In the configuration /root/config.json, specify the name of the External Store, as well as the parameters of the External Store, and other information.

        **Note:** 

        -   For an RDS VPC environment, you must enter the vpc-id and instance-id.
        -   For a common MySQL or a classic network RDS, enter an empty string in vpc-ic and instance-id.
        -   Currently only RDS VPCs in some regions are supported. For information about supported regions, see[Overview](intl.en-US/User Guide/Real-time analysis/External Store/Overview.md). If you have demands for other regions, please open a ticket.
        Configuration file parameters:

        |Parameter|Description|
        |:--------|:----------|
        |region|The region where your service is located.|
        |vpc-id|The ID of the VPC.|
        |instance-id|The ID of the RDS instance.|
        |host|The ID of the ECS instance.|
        |port|The ECS instance port.|
        |username |The name of the user.|
        |password|The password.|
        |db|The database.|
        |table.|The data table.|

        Configuration file example

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

5.  More operations

    You can also update or remove an External Store through the CLI.

    -   Update a MySQL External Store:

        ```
        aliyunlog log update_external_store --project_name="log-rds-demo" --config="file:///root/config.json" 
        ```

    -   Delete a MySQL External Store:

        ```
        aliyunlog log delete_external_store --project_name="log-rds-demo" --store_name=abc
        ```


