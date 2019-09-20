# Perform association query and analysis on logs and tables from OSS {#task_otl_z2x_fhb .task}

The scenario where the information in logs is incomplete is common in log analysis. For example, logs contains the information about user clicks, but lacks user properties such as the registration and fund information. In some log analysis scenarios, such as analyzing the impact of regions on users' payment habits, you need to analyze the properties and behavior of users.

Log Service works together with Object Storage Service \(OSS\) to provide the association analysis feature that has the following benefits:

-   Low cost
    -   Log Service supports disparate data sources. Data can be stored in different storage systems based on the data features. This lowers the cost. Data that involves infrequent changes can be stored in OSS buckets. You only need to pay for the storage service. If the data is stored in an ApsaraDB RDS for MySQL instance, you also need to pay for the computing service.
    -   OSS is a storage service provided by Alibaba Cloud. Data can be read from OSS buckets over the internal network without any traffic costs.
-   Less workload

    With the lightweight association analysis platform, you do not need to store all data in one storage system. This saves your energy.

-   Efficiency
    -   If you run a SQL statement to analyze data, you can get the result within seconds.
    -   You can define the common view as a report so that you can view the result directly.

1.  Upload a CSV file to OSS. 
    1.  Define a property file that contains the userid, nick, gender, province, and age properties.
    2.  Save and name the file as `user.csv`. Use ossutil to upload the file to OSS.

        ``` {#codeblock_8lt_r41_hbz}
        osscmd   put  ~/user.csv   oss:/testossconnector/user.csv
        ```

2.  Define the ExternalStore. Run the following SQL statement to define a virtual external table named user\_meta:

    ``` {#codeblock_sce_l3u_cyc}
    * | create table user_meta ( userid bigint, nick varchar, gender varchar, province varchar, gender varchar,age bigint) with ( endpoint='example.com',accessid='<youraccessid>',accesskey='<accesskey>',bucket='testossconnector',objects=ARRAY['user.csv'],type='oss')
    ```

    **Note:** 

    -   In the preceding SQL statement, you need to specify the following information:
        -   Table schema: columns involved in the table and the properties of each column.
        -   OSS access information: the OSS domain name, AccessKey ID, and AcccessKey secret.
        -   OSS object information: the bucket where the user.csv object is stored and the object path.
    -   The value of the objects property is an array of multiple objects.
    The result of true indicates that the external table is created.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/149429/156894967841543_en-US.png)

    Run the `select * from user_meta` statement to view the data in the table.

3.  Perform association analysis. 

    The raw logs contain the user ID information. You can run the following SQL statement to associate the ID field in logs with the userid field in OSS objects to complete the information in logs:

    ``` {#codeblock_cde_s6f_zan}
    * | select * from chiji_accesslog l join user_meta1 u on l.userid = u.userid
    ```

    -   Analyze the access information of users of different genders.

        ``` {#codeblock_7qa_hc6_fvk}
        * | select u.gender, count(1) from chiji_accesslog l join user_meta1 u on l.userid = u.userid group by u.gender
        ```

    -   Analyze the access information of users of different ages.

        ``` {#codeblock_cr3_c0a_isr}
        * | select u.age, count(1) from chiji_accesslog l join user_meta1 u on l.userid = u.userid group by u.age
        ```

    -   Analyze the access trends of users of different ages in different time periods.

        ``` {#codeblock_511_9qw_1am}
        * | selectdate_trunc('minute',__time__) as minute, count(1) ,u.age from chiji_accesslog l join user_meta1 u on l.userid = u.userid group by u.age,minute
        ```


