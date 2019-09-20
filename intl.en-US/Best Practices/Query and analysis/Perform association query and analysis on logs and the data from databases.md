# Perform association query and analysis on logs and the data from databases {#task_m5d_4xw_fhb .task}

The scenario where data is distributed across different regions is common in log analysis. In this scenario, you need to perform hierarchical analysis on user data based on both the logs and the data from databases. The result is written to databases and can be queried through report systems. Association query of Logstores and databases is required.

-   User log data: Taking game logs as an example, a classic game log contains properties such as the operation, target, blood, mana, network, payment method, click location, status code, user ID.
-   User metadata: Logs record incremental events. However, static user information, such as the gender, registration time, and region, is fixed and hard to be obtained on the client. The static user information cannot be recorded in logs. The static user information is known as user metadata.
-   Association analysis of Logstores and ApsaraDB RDS for MySQL instances: The query and analysis engine of Log Service can perform association query and analysis of Logstores and ExternalStores. You can use the SQL JOIN syntax to associate logs and metadata to analyze metrics related with user properties. In addition to referencing ExternalStores for association query and analysis, Log Service also supports writing results to ExternalStores such as ApsaraDB RDS for MySQL instances. This allows you to further process the results.

    -   Logstore: allows you to collect, store, query, and analyze logs.
    -   ExternalStore: maps data to ApsaraDB for RDS tables. Developers can store the user information in the tables.
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/149430/156894909541587_en-US.png)


1.  Collect logs to Log Service. 
    -   Collect logs on mobile clients by using the [Android SDK](../../../../reseller.en-US/SDK Reference/Android SDK.md#) or [iOS SDK](../../../../reseller.en-US/SDK Reference/IOS SDK.md#).
    -   Collect logs on servers by using [Logtail](../../../../reseller.en-US/Data Collection/Logtail collection/Overview/Overview.md#).
2.  Create a user property table. 

    Create a table named chiji\_user to store user properties including the ID, username, gender, account balance, registration time, and province.

    ``` {#codeblock_sms_hjn_9ud}
    CREATE TABLE `chiji_user` ( 
      `uid` int(11) NOT NULL DEFAULT '0', 
      `user_nick` text, 
      `gender` tinyint(1) DEFAULT NULL, 
      `age` int(11) DEFAULT NULL, 
      `register_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP, 
      `balance` float DEFAULT NULL, 
      `province` text, PRIMARY KEY (`uid`) 
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8
    ```

3.  Create an ExternalStore. 
    1.  Install the Alibaba Cloud Command Line Interface \(CLI\) before creating an ExternalStore.

        ``` {#codeblock_g5n_ens_326}
        pip install -U aliyun-log-cli
        ```

    2.  Specify the project and create the configuration file /root/config.json for the ExternalStore.

        ``` {#codeblock_a9z_nsp_bui}
        aliyunlog log create_external_store --project_name="log-rds-demo" --config="file:///root/config.json" 
        ```

    3.  Set the ExternalStore name and parameters in the configuration file. For an ApsaraDB for RDS instance in a VPC, you need to set the vpc-id, instance-id, host, port, username, password, db, table, and region parameters.

        ``` {#codeblock_87w_nko_3m9}
        { 
             "externalStoreName": "chiji_user", 
             "storeType": "rds-vpc", 
             "parameter": { 
             "vpc-id": "vpc-m5eq4irc1pucpk85f****", 
             "instance-id": "rm-m5ep2z57814qs****", 
             "host": "example.com", 
             "port": "3306", 
             "username": "testroot", 
             "password": "123456789", 
             "db": "chiji", 
             "table": "chiji_user", 
             "region": "cn-qingdao" 
          } 
        }
        ```

4.  Add the specified IP address to the whitelist. 
    -   For an ApsaraDB for RDS instance, add the IP address `100.104.0.0/16` to the whitelist of IP addresses allowed to access the instance.
    -   For an ApsaraDB RDS for MySQL instance, add the IP address to the security group.
5.  Perform association analysis. 
    -   Analyze the gender distribution of active users.

        Use the JOIN syntax to associate logs and user properties through the identical value of the userid field in logs and the uid field in the ApsaraDB for RDS instance.

        ``` {#codeblock_n6r_d70_nna}
        * | select case gender when 1 then 'male' else 'female' end as gender , count(1) as pv from log l join chiji_user u on l.userid = u.uid group by gender order by pv desc
        ```

    -   Analyze the activity level of users in different provinces.

        ``` {#codeblock_hgl_in0_k1y}
        * | select province , count(1) as pv from log l join chiji_user u on l.userid = u.uid group by province order by pv desc
        ```

    -   Analyze the consumption trends of users of different genders.

        ``` {#codeblock_p52_6zo_2mv}
        * | select case gender when 1 then 'male' else 'female' end as gender , sum(money) as money from log l join chiji_user u on l.userid = u.uid group by gender order by money desc
        ```

6.  Store the result of the association query and analysis. 
    1.  Create a result table that stores page views \(PVs\) per minute.

        ``` {#codeblock_q06_9au_vwn}
        CREATE TABLE `report` ( 
          `minute` bigint(20) DEFAULT NULL, 
          `pv` bigint(20) DEFAULT NULL 
        ) ENGINE=InnoDB DEFAULT CHARSET=utf8
        ```

    2.  Create an ExternalStore for the report table by following the instructions in step 3, and save the result in the table.

        ``` {#codeblock_vww_tz6_qw0}
        * | insert into report select __time__- __time__ % 300 as min, count(1) as pv group by min
        ```

        The execution result of the SQL statement displays the number of the rows that are written to the ApsaraDB for RDS instance.

        ![SQL result](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/149430/156894909541597_en-US.png)


