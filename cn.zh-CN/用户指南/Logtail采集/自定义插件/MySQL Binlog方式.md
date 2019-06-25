# MySQL Binlog方式 {#concept_ypy_xvc_wdb .concept}

MySQL Binlog 同步类似 [canal](https://github.com/alibaba/canal) 功能，以MySQL slave 的形式基于 Binlog 进行同步，性能较高。

**说明：** 此功能目前只支持 Linux，依赖 Logtail 0.16.0 及以上版本，版本查看与升级参见[安装Logtail（Linux系统）](cn.zh-CN/用户指南/Logtail采集/安装/安装Logtail（Linux系统）.md)。

## 功能 {#section_pwd_n5q_pdb .section}

-   通过 Binlog 订阅数据库增量更新数据，性能优越，支持 RDS 等 MySQL 协议的数据库。
-   支持数据库过滤（支持正则）。
-   支持同步点设置。
-   支持 Checkpoint 保存同步状态。

## 实现原理 {#section_ihv_n5q_pdb .section}

如下图所示，Logtail 内部实现了 MySQL Slave 的交互协议，将自己伪装成为 MySQL 的 Slave 节点，向 MySQL master 发送 dump 协议；MySQL 的 master 收到 dump 请求后，会将自身的 Binary log 实时推送给 Logtail，Logtail 对Binlog进行事件解析、过滤、数据解析等，并将解析好的数据上传到日志服务。

![](images/2929_zh-CN.png "实现原理")

## 应用场景 {#section_tkq_r5q_pdb .section}

适用于数据量较大且性能要求较高的数据同步场景。

-   增量订阅数据库改动进行实时查询与分析。
-   数据库操作审计。
-   通过数据库更新信息使用日志服务进行自定义查询分析、可视化、对接下游流计算、导入MaxCompute离线计算、导入OSS长期存储等。

## 注意事项 {#section_co4_ibp_5vm .section}

建议您适当放开对Logtail的资源限制以应对流量突增，避免Logtail因为资源超限被强制重启，对您的数据造成不必要的风险。

资源限制可通过修改/usr/local/ilogtail/ilogtail\_config.json文件实现，修改完成后执行命令`sudo /etc/init.d/ilogtaild stop;sudo /etc/init.d/ilogtaild start`重启Logtail。

例如，以下示例表示将CPU和内存资源的限制放宽到双核及2048 MB：

``` {#codeblock_719_zfi_mlx}
{
    ...

    "cpu_usage_limit":2,
    "mem_usage_limit":2048,

    ...
}
```

## 配置项 {#section_pvl_s5q_pdb .section}

MySQL Binlog方式输入源类型为：`service_canal`。

|配置项|类型|是否必须|说明|
|:--|:-|:---|:-|
|Host|string|否|数据库主机，默认为127.0.0.1。|
|Port|int|否|数据库端口，默认为3306。|
|User|string|否|数据库用户名，默认为root。|
|Password|string|否|数据库密码；默认为空。|
|ServerID|int|否|Logtail伪装成的Mysql Slave ID，默认为125。 **说明：** ServerID对于一个MySQL数据库必须唯一，否则同步失败。

 |
|IncludeTables|string 数组|是|包含的表名称（包括db，例如`test_db.test_table`），为**正则表达式**，若某表不符合IncludeTables任一条件则该表不会被采集；如果您希望采集所有表，请将此参数指定为`.*\\..*`。 **说明：** 若需要完全匹配，请在前后分别加上`^` `$`，例如`^test_db\\.test_table$`。

 |
|ExcludeTables|string 数组|否|忽略的表名称（包括db，例如`test_db.test_table`），为**正则表达式**，若某表符合ExcludeTables任一条件则该表不会被采集；不设置时默认收集所有表。 **说明：** 若需要完全匹配，请在前后分别加上`^` `$`，例如`^test_db\\.test_table$`。

 |
|StartBinName|string|否|首次采集的Binlog文件名，不设置时默认从当前时间点开始采集。|
|StartBinLogPos|int|否|首次采集的Binlog文件名的offset，默认为0。|
|EnableGTID|bool|否|是否附加[全局事务ID](https://dev.mysql.com/doc/refman/5.7/en/replication-gtids-concepts.html)，默认为true，为false时上传的数据将不附加。|
|EnableInsert|bool|否|是否收集insert事件的数据，默认为true，为false时insert事件将不采集。|
|EnableUpdate|bool|否|是否收集update事件的数据，默认为true，为false时update事件将不采集。|
|EnableDelete|bool|否|是否收集delete事件的数据，默认为true，为false时delete事件将不采集。|
|EnableDDL|bool|否|是否收集DDL（data definition language）事件的数。 **说明：** 该选项默认为false，为false时DDL事件将不采集。该选项不支持`IncludeTables` `ExcludeTables`过滤。

 |
|Charset|string|否|编码方式，默认为`utf-8`。|
|TextToString|bool|否|是否将`text`类型数据转换成string，默认为false。|
|PackValues|bool|否| 是否将事件的数据打包成 JSON 格式。默认为 false，表示不打包。

 如果启用此功能，Logtail 会将事件数据以 JSON 格式集中打包到 data 和 old\_data 两个字段中，其中 old\_data 仅在 row\_update 事件中有意义。

 示例：假设数据表有三列 c1，c2，c3，在不开启此功能下，row\_insert 事件数据中会有 c1，c2，c3 三个字段，而开启此功能后，c1，c2，c3会被统一打包到 data 字段，值为 `{"c1":"...", "c2": "...", "c3": "..."}`。

 **说明：** 该参数仅在 0.16.19 及以上版本支持。

 |
|EnableEventMeta|bool|否| 是否采集事件的元数据，默认为 false，表示不采集。

 Binlog 事件的元数据包括 event\_time、event\_log\_position、event\_size 以及 event\_server\_id 四项。 **说明：** 该参数仅在 0.16.21 及以上版本支持。

 |

## 使用限制 {#section_bss_bvq_pdb .section}

此功能目前仅支持 Linux，依赖Logtail 0.16.0 及以上版本，版本查看与升级参见[安装Logtail（Linux系统）](cn.zh-CN/用户指南/Logtail采集/安装/安装Logtail（Linux系统）.md)。

-   MySQL 必须开启 Binlog，且 Binlog 必须为 row 模式（默认 RDS 已经开启）。

    ``` {#codeblock_ue0_m4i_zlv}
    # 查看是否开启Binlog
    mysql> show variables like "log_bin";
    +---------------+-------+
    | Variable_name | Value |
    +---------------+-------+
    | log_bin       | ON    |
    +---------------+-------+
    1 row in set (0.02 sec)
    # 查看Binlog类型
    mysql> show variables like "binlog_format";
    +---------------+-------+
    | Variable_name | Value |
    +---------------+-------+
    | binlog_format | ROW   |
    +---------------+-------+
    1 row in set (0.03 sec)
    ```

-   ServerID 必须唯一，确保需同步的MySQL所有Slave的ID不重复。
-   需保证配置的用户具有需要采集的数据库读权限以及MySQL REPLICATION权限，示例如下：

    ``` {#codeblock_yb0_b2l_m65}
    CREATE USER canal IDENTIFIED BY 'canal';  
    GRANT SELECT, REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'canal'@'%';
    -- GRANT ALL PRIVILEGES ON *.* TO 'canal'@'%' ;
    FLUSH PRIVILEGES;
    ```

-   首次配置采集时如果不配置`StartBinName`，默认从当前时间点采集。

    如果想从指定位置采集，可以查看当前的Binlog以及offset，并将`StartBinName`、`StartBinLogPos`设置成对应的值，示例如下：

    **说明：** 

    当指定`StartBinName`时，第一次采集会产生较大开销。

    ``` {#codeblock_szy_tnx_42x}
    # StartBinName 设置成 "mysql-bin.000063", StartBinLogPos 设置成 0
    mysql> show binary logs;
    +------------------+-----------+
    | Log_name         | File_size |
    +------------------+-----------+
    | mysql-bin.000063 |       241 |
    | mysql-bin.000064 |       241 |
    | mysql-bin.000065 |       241 |
    | mysql-bin.000066 |     10778 |
    +------------------+-----------+
    4 rows in set (0.02 sec)
    ```

-   如安全需求较高，建议将SQL访问用户名和密码配置为xxx，待配置同步至本地机器后，在/usr/local/ilogtail/user\_log\_config.json文件找到对应配置进行修改。

    **说明：** 

    -   修改完毕后请执行以下命令重启Logtail：

        ``` {#codeblock_ofe_96x_qj1}
        sudo /etc/init.d/ilogtaild stop; sudo /etc/init.d/ilogtaild start
        ```

    -   如果您再次在Web端修改此配置，您的手动修改会被覆盖，请再次手动修改本地配置。
-    **RDS 相关限制：** 
    -   无法直接在RDS服务器上安装Logtail，您需要将Logtail安装在能连通RDS实例的任意一台ECS上。
    -   RDS 只读备库当前不支持Binlog采集，您需要配置主库进行采集。

## 数据可靠性 {#section_x54_4lj_yfb .section}

建议您启用MySQL服务器的全局事务ID（GTID）功能，并将Logtail升级到0.16.15及以上的版本以保证数据可靠性，避免因主备切换造成的数据重复采集。但在网络长时间中断时，依旧有可能发生数据漏采集情况。

-    **数据漏采集**：Logtail与MySQL服务器之间的网络**长时间中断**时，可能会产生数据漏采集情况。

    MySQL Binlog插件通过伪装成MySQL的slave节点来不断从master节点获取binlog数据，因此，Logtail会和master节点之间建立连接以获取数据。一旦Logtail和master之间的网络发生中断，Logtail会不断地尝试重连直至恢复，而master仍可以在和Logtail的网络中断期间不断地提供服务，产生新的binlog数据，并且回收旧的binlog数据。当网络恢复且Logtail重连成功后，Logtail会使用自身的checkpoint去向master请求更多的binlog数据，而由于长时间的网络中断，它所需要的数据很可能已经被回收了，这时就会触发Logtail的异常恢复机制。**在异常恢复机制中，Logtail会从master获取最近的binlog位置，以它为起点继续采集，这样就会跳过checkpoint和最近的binlog位置之间的数据，导致数据漏采集。** 

-    **数据重复采集**：如果master和slave之间的binlog序号不同步时，发生了主备切换事件，可能会产生数据重复采集情况。如果您的MySQL服务器支持并启用了GTID（MySQL 5.6引入），且Logtail是0.16.15及以上版本，即可避免以下场景中的数据重复采集情况。

    在MySQL主备同步的设置下，master会将产生的binlog同步给slave，slave在收到后会存储到本地的binlog文件中。如果master和slave之间的binlog序号不同步时，发生了主备切换事件，以binlog文件名和偏移作为checkpoint的机制将导致数据重复采集。

    例如，有一段数据在master上位于`(binlog.100, 4)`到`(binlog.105, 4)`之间，而在slave上是`(binlog.1000, 4)`到`(binlog.1005, 4)`，并且Logtail已经从master获取了这部分数据，将本地checkpoint更新到了`(binlog.105, 4)`。如果此时发生了主备切换且无任何异常发生，Logtail将会继续使用本地checkpoint去向新master获取binlog信息，即以`(binlog.105, 4)`去向新master请求更多的数据。但是因为新master上的`(binlog.1000, 4)`到`(binlog.1005, 4)`这部分数据的序号都大于Logtail所请求的序号，它会把它们返回给Logtail，这就引起了binlog数据的重复采集。


## 操作步骤 {#section_l22_nvq_pdb .section}

从RDS中同步`user_info`库中不以`_inner`结尾的表，配置如下。

1.  选择输入源。

    单击**数据接入向导**图标或**创建配置**，进入数据接入向导。并在**选择数据库类型**步骤中选择**MYSQL BINLOG**。

2.  填写输入配置。

    进入**输入源配置**页面，填写**插件配置**。

     **插件配置**输入框中已为您提供配置模板，请根据您的需求替换配置参数信息。

     `inputs`部分为采集配置，是必选项；`processors`部分为处理配置，是可选项。采集配置部分需要按照您的数据源配置对应的采集语句，处理配置部分请参考[处理采集数据](cn.zh-CN/用户指南/Logtail采集/自定义插件/处理采集数据.md)配置一种或多种采集方式。

    **说明：** 

    -   若安全需求较高，建议将SQL访问用户名/密码配置为xxx，待配置同步至本地机器后，在/usr/local/ilogtail/user\_log\_config.json文件找到对应配置进行修改。
    -   请将Host/User/Password/Port替换为实际访问参数。
    示例配置如下：

    ``` {#codeblock_6od_09m_1o9}
    {
     "inputs": [
         {
             "type": "service_canal",
             "detail": {
                 "Host": "************.mysql.rds.aliyuncs.com",
                 "User" : "root",
                 "ServerID" : 56321,
                 "Password": "*******",
                 "IncludeTables": [
                     "user_info\\..*"
                 ],
                 "ExcludeTables": [
                     ".*\\.\\S+_inner"
                 ],
                 "TextToString" : true,
                 "EnableDDL" : true
             }
         }
     ]
    }
    ```

3.  应用到机器组。

    进入**应用到机器组**页面。请在此处勾选运行有此插件的Logtail机器组。

    如您之前没有创建过机器组，单击**+创建机器组**以创建一个新的机器组。

    **说明：** Binlog采集只需要一台安装Logtail的机器，您的机器组中只需要配置一个服务器IP即可。如您的机器组中有多台机器，请不要配置一样的ServerID。

4.  修改本地配置。

    如果您没有在输入源配置页面输入真实的URL、账号、密码等信息，需要在采集配置下发到本地后手动修改其中的内容。

    **说明：** 如果您服务端输入的是真实信息，则无需此步骤。

    1.  登录Logtail所在服务器，查找`/usr/local/ilogtail/user_log_config.json`文件中`service_canal`关键词，修改下述对应的`Host`、`User`、`Password`等字段。
    2.  执行以下命令重启Logtail。

        ``` {#codeblock_ih5_drm_rbd}
        sudo /etc/init.d/ilogtaild stop; sudo /etc/init.d/ilogtaild start
        ```

    Binlog采集配置已经完成，如果您的数据库有在进行对应的修改操作，则Logtail会立即将变化的数据采集到日志服务。

    **说明：** Logtail默认采集Binlog的增量数据，如查看不到数据请确认在配置更新后数据库对应的表存在修改操作。


## 示例 {#section_ick_3wq_pdb .section}

例如，按照以上操作步骤配置处理方式后，对`user_info` 下的`SpecialAlarm`表分别执行`INSERT`、`UPDATE`、`DELETE`操作。数据库表结构、数据库操作及Logtail采集的样例如下。

-   表结构

    ``` {#codeblock_yld_r0d_t46}
    CREATE TABLE `SpecialAlarm` (
    `id` int(11) unsigned NOT NULL AUTO_INCREMENT,
    `time` datetime NOT NULL,
    `alarmtype` varchar(64) NOT NULL,
    `ip` varchar(16) NOT NULL,
    `count` int(11) unsigned NOT NULL,
    PRIMARY KEY (`id`),
    KEY `time` (`time`) USING BTREE,
    KEY `alarmtype` (`alarmtype`) USING BTREE
    ) ENGINE=MyISAM AUTO_INCREMENT=1;
    ```

-   数据库操作

    对数据库执行INSERT、DELETE和UPDATE三种操作：

    ``` {#codeblock_tz4_hcs_ui2}
    insert into specialalarm (`time`, `alarmType`, `ip`, `count`) values(now(), "NO_ALARM", "10.10.**.***", 55);
    delete from specialalarm where id = 4829235  ;
    update specialalarm set ip = "10.11.***.**" where id = "4829234";
    ```

    并为`zc.specialalarm`创建一个索引：

    ``` {#codeblock_rma_yzb_9ns}
    ALTER TABLE `zc`.`specialalarm` 
    ADD INDEX `time_index` (`time` ASC);
    ```

-   样例日志

    在数据预览或在查找页面中，可以看到对应每种操作的样例日志如下：

    -   INSERT语句

        ``` {#codeblock_yuk_402_pq4}
        __source__:  10.30.**.**  
        __tag__:__hostname__:  iZbp145dd9fccu*****  
        __topic__:    
        _db_:  zc  
        _event_:  row_insert  
        _gtid_:  7d2ea78d-b631-11e7-8afb-00163e0eef52:536  
        _host_:  *********.mysql.rds.aliyuncs.com  
        _id_:  113  
        _table_:  specialalarm  
        alarmtype:  NO_ALARM  
        count:  55  
        id:  4829235  
        ip:  10.10.22.133  
        time:  2017-11-01 12:31:41
        ```

    -   DELETE语句

        ``` {#codeblock_dag_crb_i0e}
        __source__:  10.30.**.**  
        __tag__:__hostname__:  iZbp145dd9fccu****
        __topic__:    
        _db_:  zc  
        _event_:  row_delete  
        _gtid_:  7d2ea78d-b631-11e7-8afb-00163e0eef52:537  
        _host_:  *********.mysql.rds.aliyuncs.com  
        _id_:  114  
        _table_:  specialalarm  
        alarmtype:  NO_ALARM  
        count:  55  
        id:  4829235  
        ip:  10.10.22.133  
        time:  2017-11-01 12:31:41
        ```

    -   UPDATE语句

        ``` {#codeblock_wkb_qlh_hkj}
        __source__:  10.30.**.**  
        __tag__:__hostname__:  iZbp145dd9fccu****  
        __topic__:    
        _db_:  zc  
        _event_:  row_update  
        _gtid_:  7d2ea78d-b631-11e7-8afb-00163e0eef52:538  
        _host_:  *********.mysql.rds.aliyuncs.com  
        _id_:  115  
        _old_alarmtype:  NO_ALARM  
        _old_count:  55  
        _old_id:  4829234  
        _old_ip:  10.10.22.133  
        _old_time:  2017-10-31 12:04:54  
        _table_:  specialalarm  
        alarmtype:  NO_ALARM  
        count:  55  
        id:  4829234  
        ip:  10.11.145.98  
        time:  2017-10-31 12:04:54
        ```

    -   DDL（data definition language）语句

        ``` {#codeblock_6aj_ww0_jhd}
        __source__:  10.30.**.**  
        __tag__:__hostname__:  iZbp145dd9fccu****  
        __topic__:    
        _db_:  zc  
        _event_:  row_update  
        _gtid_:  7d2ea78d-b631-11e7-8afb-00163e0eef52:539  
        _host_:  *********.mysql.rds.aliyuncs.com  
        ErrorCode:  0
        ExecutionTime:  0
        Query:  ALTER TABLE `zc`.`specialalarm` 
        ADD INDEX `time_index` (`time` ASC)
        StatusVars:
        ```


## 元数据字段 {#section_zr3_ktm_lfb .section}

binlog采集会将一些元数据和日志一起上传，具体上传的元数据列表如下：

|字段|说明|示例|
|:-|:-|:-|
| `_host_` |数据库host名称。| `*********.mysql.rds.aliyuncs.com` |
| `_db_` |数据库名称。| `my-database` |
| `_table_` |表的名称。| `my-table` |
| `_event_` |事件类型。| `row_update`、`row_insert`、`row_delete`等|
| `_id_` |本次采集的自增ID，从0开始，每次采集一个binlog事件后加1。| `1` |
| `_gtid_` |GTID。| `7d2ea78d-b631-11e7-8afb-00163e0eef52:536` |
| `_filename_` |binlog文件名。| `binlog.001` |
| `_offset_` |binlog文件偏移量，该值只会在每次commit后更新。| `12876` |

