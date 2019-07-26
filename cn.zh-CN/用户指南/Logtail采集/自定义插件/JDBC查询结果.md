# JDBC查询结果 {#concept_jpm_gxc_wdb .concept}

以SQL形式定期采集数据库中的数据，根据SQL可实现各种自定义采集方式。

**说明：** 此功能目前仅支持Linux，依赖Logtail 0.16.0及以上版本，版本查看与升级参见[安装Logtail（Linux系统）](cn.zh-CN/用户指南/Logtail采集/安装/安装Logtail（Linux系统）.md)。

## 功能 {#section_iym_yzq_pdb .section}

-   支持提供MySQL接口的数据库，包括RDS：
-   支持分页设置
-   支持时区设置
-   支持超时设置
-   支持checkpoint状态保存
-   支持SSL
-   支持每次最大采集数量限制

## 实现原理 {#section_p3m_c1r_pdb .section}

![实现原理](images/2930_zh-CN.png "实现原理")

如上图所示，Logtail内部根据用户配置定期执行指定的SELECT语句，将SELECT返回的结果作为数据上传到日志服务。

Logtail在获取到执行结果时，会将结果中配置的CheckPoint字段保存在本地，当下次执行SQL时，会将上一次保存的CheckPoint带入到SELECT语句中，以此实现增量数据采集。

## 应用场景 {#section_egv_21r_pdb .section}

-   根据自增ID或时间等标志进行增量数据同步。
-   自定义筛选同步。

## 参数说明 {#section_hyn_f1r_pdb .section}

该输入源类型为：`service_mysql`。

|参数|类型|必选或可选|参数说明|
|:-|:-|:----|:---|
|Address|string|可选|MySQL地址，默认为”127.0.0.1:3306”。|
|User|string|可选|数据库用户名，默认为”root”。|
|Password|string|可选|数据库密码，默认为空。|
|DialTimeOutMs|int|可选|数据库连接超时时间，单位为ms，默认为5000ms。|
|ReadTimeOutMs|int|可选|数据库连接超时时间，单位为ms，默认为5000ms。|
|StateMent|string|必选|SQL语句。|
|Limit|bool|可选|是否使用Limit分页，默认为false。|
|PageSize|int|可选|分页大小，Limit为true时必须配置。|
|MaxSyncSize|int|可选|每次同步最大记录数，为0表示无限制，默认为0。|
|CheckPoint|bool|可选|是否使用checkpoint，默认为false。|
|CheckPointColumn|string|可选|checkpoint列名称，CheckPoint为true时必须配置。|
|CheckPointColumnType|string|可选|checkpoint列类型，支持`int`和`time`两种类型。|
|CheckPointStart|string|可选|checkpoint初始值。|
|CheckPointSavePerPage|bool|可选|为true时每次分页时保存一次checkpoint，为false时每次同步完后保存checkpoint。|
|IntervalMs|int|必选|同步间隔，单位为ms。|

## 使用限制 {#section_g3n_31r_pdb .section}

-   建议使用`Limit`分页，使用`Limit`分页时，SQL查询会自动在`StateMent`后追加LIMIT语句。
-   使用`CheckPoint`时，`StateMent`中SELECT出的数据中必须包含checkpoint列，且where条件中必须包含checkpoint列，该列的值填`?` 

    例如checkpoint为”id”，`StateMent`为`SELECT * from ... where id > ?`。

-   `CheckPoint`为true时必须配置`CheckPointColumn`、`CheckPointColumnType`、`CheckPointStart`。
-   `CheckPointColumnType`只支持`int`和`time`类型，`int`类型内部存储为int64，`time`支持MySQL的date、datetime、time。

## 操作步骤 {#section_hyv_p1r_pdb .section}

从MySQL中增量同步logtail.VersionOs中count \> 0的数据，同步间隔为10s，checkpoint起始时间为2017-09-25 11:00:00，请求方式为分页请求，每页100，每次分页后保存checkpoint。具体配置步骤如下。

1.  选择输入源。

    单击**接入数据**按钮，并在**接入数据**页面中选择**MYSQL查询结果**。

2.  选择日志空间。

    可以选择已有的Logstore，也可以新建Project或Logstore。

3.  创建机器组。

    在创建机器组之前，您需要首先确认已经安装了Logtail。 安装完Logtail后单击**确认安装完毕**创建机器组。如果您之前已经创建好机器组 ，请直接单击**使用现有机器组**。

4.  机器组配置。

    选择一个机器组，将该机器组从**源机器组**移动到**应用机器组**。

5.  数据源设置。

    **插件配置**输入框中已为您提供配置模板，请根据您的需求替换配置参数信息。

    `inputs`部分为采集配置，是必选项；`processors`部分为处理配置，是可选项。采集配置部分需要按照您的数据源配置对应的采集语句，处理配置部分请参考[处理采集数据](cn.zh-CN/用户指南/Logtail采集/自定义插件/处理采集数据.md)配置一种或多种采集方式。

    **说明：** 若安全需求较高，建议将SQL访问用户名/密码配置为xxx，待配置同步至本地机器后，在/usr/local/ilogtail/user\_log\_config.json文件找到对应配置进行修改。

    示例配置如下：

    ``` {#codeblock_0si_odq_48y}
    {
     "inputs": [
         {
             "type": "service_mysql",
             "detail": {
                 "Address": "********:3306",
                 "User": "logtail",
                 "Password": "*******",
                 "DataBase": "logtail",
                 "Limit": true,
                 "PageSize": 100,
                 "StateMent": "SELECT * from logtail.VersionOs where time > ?",
                 "CheckPoint": true,
                 "CheckPointColumn": "time",
                 "CheckPointStart": "2017-09-25 11:00:00",
                 "CheckPointSavePerPage": true,
                 "CheckPointColumnType": "time",
                 "IntervalMs": 10000
             }
         }
     ]
    }
    ```

6.  查询分析配置。

    默认已经设置好索引，您也可以手动进行修改。

7.  修改本地配置。

    如果您没有在数据源设置页面输入真实的URL、账号、密码等信息，需要在采集配置下发到本地后手动修改其中的内容。

    **说明：** 如果您服务端输入的是真实信息，则无需此步骤。

    1.  登录Logtail所在服务器，查找`/usr/local/ilogtail/user_log_config.json`文件中`service_mysql`关键词，修改下述对应的`Address`、`User`、`Password`等字段。
    2.  执行以下命令重启Logtail。

        ``` {#codeblock_86x_opl_l2m}
        sudo /etc/init.d/ilogtaild stop; sudo /etc/init.d/ilogtaild start
        ```


## 示例 {#section_z45_gbr_pdb .section}

例如，按照以上操作步骤配置处理方式后，即可在日志服务控制台查看采集处理过的日志数据。表结构和Logtail采集的日志样例如下。

-   **表结构** 

    ``` {#codeblock_y43_85a_o3v}
    CREATE TABLE `VersionOs` (
      `id` int(11) unsigned NOT NULL AUTO_INCREMENT COMMENT 'id',
      `time` datetime NOT NULL,
      `version` varchar(10) NOT NULL DEFAULT '',
      `os` varchar(10) NOT NULL,
      `count` int(11) unsigned NOT NULL,
      PRIMARY KEY (`id`),
      KEY `timeindex` (`time`)
    )
    ```

-   **样例输出** 

    ``` {#codeblock_obw_fza_fwi}
    "count":  "4"  
    "id:  "721097"  
    "os:  "Windows"  
    "time:  "2017-08-25 13:00:00"  
    "version":  "1.3.0"
    ```


