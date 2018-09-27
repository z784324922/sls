# External OSS Storage {#concept_fgy_nrt_q2b .concept}

Log Service supports the joint query of OSS and Log Service by means of External Store, namely, both the OSS data and Log Service data are used as the data source in the query.

## Benefits {#section_pvn_fd5_q2b .section}

-   Save storage costs. External OSS Storage use heterogeneous data. It selects the appropriate storage system based on the characteristics of the data, maximizing cost savings. For data with fewer updates, if you choose to store the data on the OSS, you only need to pay a small amount of storage fees. If you store the data on MySQL, you have to pay for instance calculation besides data storage.
-   Save traffic costs. As the storage system of Alibaba Cloud, OSS can read and write data through the intranet. Therefor you are free of traffic charges.

## Prerequisite {#section_fyw_nd5_q2b .section}

-   You have activated Log Service and created the project and Logstore.

    For information about the preparation of Log Service, see [Preparation](intl.en-US/User Guide/Preparation/Preparation.md).

-   You have enabled OSS and created storage space.

    For more information, see [Create a bucket](../../../../intl.en-US/Quick Start/Create a bucket.md).


## Procedure {#section_obn_3d5_q2b .section}

1.  Log on to the OSS console and upload the CSV format file to OSS.

    For information about file upload procedures, see [Upload an object](../../../../intl.en-US/Quick Start/Upload an object.md).

2.  Define External Storage in Log Service.

    Enter an SQL statement in the query box of the Log Service Query page. Use SQL to define the virtual external table and map it to the OSS file. If the execution result is true, the execution succeeds.

    In the SQL statement, define information such as the External Storage name and the table schema, and specify the OSS access information and file information by using the with syntax.

    |Item|Parameter|Description|
    |:---|:--------|:----------|
    |External Store name|tableName|The name of the External Storage, that is, the name of the virtual table.|
    |Table schema|Column names and the format of the table.|Defines the properties of the table.|
    |OSS access information|endpoint|Region.|
    |accessid|Your Access Key ID.|
    |accesskey|Your Access Key Secret.|
    |OSS file information|bucket|The name of the OSS bucket where the CSV file is located.|
    |objects|The path to the CSV file.**Note:** objects is of type array and can contain multiple OSS files.

|
    |type|This parameter is fixed at oss, which indicates that the external storage type is OSS.|

    For example, use an SQL statement to define an external storage named tableName:

    ```
    * | create table tableName ( userid bigint, nick varchar, gender varchar, province varchar, gender varchar,age bigint) with ( endpoint='oss-cn-hangzhou-internal.aliyuncs.com',accessid='**************************',accesskey ='****************************',bucket='testoss*********',objects=ARRAY['user.csv'],type='oss')
    ```

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17023/15380421958538_en-US.png)

    The preceding example specifies the table properties of as follows:

    -   userid is of type bigint.
    -   nick is of type varchar.
    -   gender is of type varchar.
    -   province is of type varchar.
    -   age is of type bigint.
3.  Verify that the External Storage has been successfully defined.

    Execute SQL `select * from user_meta` to check if the returned result is the table content that you previously defined.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17023/15380421958539_en-US.png)

4.  Complete the joint query of the OSS file and Log Service data by using the Join syntax.

    Enter an SQL statement in the query box on the Log Service Query page. Use the Join syntax to reference OSS External Storage.

    For example, reference the user\_meta table in the query, and associate the ID in the log with the userid in the OSS file to complete the log information.

    `* | select * from chiji_accesslog l join user_meta u on l.userid = u.userid`

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17023/15380421958540_en-US.png)


