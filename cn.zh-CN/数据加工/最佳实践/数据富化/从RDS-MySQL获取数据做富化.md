# 从RDS-MySQL获取数据做富化 {#concept_2039232 .concept}

本文档介绍如何通过资源函数从RDS-MySQL中获取数据对日志数据进行富化。

假设RDS中存放用户信息表格userinfo如下：

|id|province|city|uid|
|--|--------|----|---|
|1|jiangsu|nanjing|01234|
|2|henan|zhengzhou|01235|
|3|heilongjiang|haerbin|01236|
|4|jiangsu|yantai|01237|

## 定期刷新全量获取 {#section_28t_iey_rlk .section}

假设富化数据定期全量刷新，如果希望数据加工任务能够自动定期去获取，可以如下配置。

``` {#codeblock_7en_pcz_n7f}
res_rds_mysql(..., refresh_interval=300)
```

上述语法会返回一个表格结构并且会自动跟踪表格，每隔5分钟重新获取一遍mysql表的内容并刷新这个表格内容。

## 获取部分数据 {#section_xm0_x4b_r9z .section}

如果仅使用RDS-MySQL中个别字段做富化，推荐使用参数`table`、`sql`和`fields`来进行行或者列过滤。这样可以降低维表大小，增加富化效率。

如下进行列过滤，值选择`city`和`uid`列，两者效果没有任何区别。

``` {#codeblock_27o_c8n_zpg}
res_rds_mysql(..., sql="select city, uid from userinfo")      # 列过滤
res_rds_mysql(..., table="userinfo", fields=["city", "uid"])    # 列过滤
```

如下使用sql进行列与行过滤，选择所有`uid > 1234`的数据。

``` {#codeblock_rnq_yc0_gdh}
res_rds_mysql(..., sql="select * from userinfo where uid > 1234")   # 行过滤
res_rds_mysql(..., sql="select city, uid from userinfo where uid > 1234")   # 行列过滤
```

## 获取后再过滤数据 {#section_ssm_38l_dsj .section}

在使用参数`table`、`sql`和`fields`来进行行或者列过滤不能满足需求时，可以进一步使用参数`fetch_exclude_data`和`/`或`fetch_include_data`来进行行过滤。例如：

``` {#codeblock_0d6_1uw_v1p}
res_rds_mysql(..., fetch_include_data="uid==0123*")    # 保留所有uid以0123开头的数据
res_rds_mysql(..., fetch_exclude_data="uid < 1234")    # 去除所有uid小于1234的数据
res_rds_mysql(..., fetch_include_data="city:n", fetch_exclude_data="uid < 1234")
```

以上注释解释了两者的区别，注意到这里两个参数的格式都是查询字符串。

同时配置`fetch_exclude_data`和`fetch_include_data`，会优先执行`fetch_exclude_data`将不符合的数据剔除，然后执行`fetch_include_data`将符合的数据添加进来。`uid`大于等于1234且以`city`包含字母n的所有数据做维表。

**说明：** 这种过滤是获取数据到本地后再进行过滤，因此效率没有参数`table`、`sql`和`fields`过滤高。

## 调整返回表格结构 {#section_qn2_ybe_hn2 .section}

默认返回的表格与RDS-MySQL中的表格结构一致，如果需要调整，例如将`province`字段改成`prov`等，可以使用如下方法。

``` {#codeblock_bz9_q80_bf8}
res_rds_mysql(..., sql="select id, uid, province as prov, city from userinfo")
res_rds_mysql(..., table="userinfo", fields=["id", "uid", ("province", "prov"), "city" ])
```

两个方法效果相同。关于`fields`参数请参见[资源函数](cn.zh-CN/数据加工/数据加工语法/表达式函数/资源函数.md#)。

## 结合e\_table\_map和e\_search\_map\_table做数据富化实践 {#section_e98_4bk_03e .section}

**说明：** `res_rds_mysql`函数在控制台上单独使用没有任何意义，因为该函数只是去mysql获取数据，并没有做任何富化的操作。下面将详细介绍使用`res_rds_mysql`函数做富化的几个函数用法。

-   使用e\_table\_map完全匹配。
    -   假设如下数据库表中内容：

        |province|city|population|cid|eid|
        |--------|----|----------|---|---|
        |上海|上海|2000|1|00001|
        |天津|天津|800|1|00002|
        |北京|北京|4000|1|00003|
        |河南|郑州|3000|2|00004|
        |江苏|南京|1500|2|00005|

    -   源Logstore中数据：

        ``` {#codeblock_uuz_ram_swo}
        # 下图为4条日志
        time:"1566379109"
        data:"test-one"
        cid:"1"
        eid:"00001"
        
        time:"1566379111"
        data:"test_second"
        cid:"1"
        eid:"12345"
        
        time:"1566379111"
        data:"test_three"
        cid:"2"
        eid:"12345"
        
        time:"1566379113"
        data:"test_four"
        cid:"2"
        eid:"12345"
        
        ......
        ```

    -   匹配mysql表中`cid`的数据，并且将`provice`等字段添加到源Logstore 中。

        ``` {#codeblock_7s4_78g_8ik}
        # DSL 编排语法
        # 该语法会将mysql表中cid的值和logstore数据中cid的值完全相等的行的"provice","city","population"三个字段的值添加到源logstore的数据中，组成新的数据
        e_table_map(res_rds_mysql(...),"cid",["provice","city","population"])
        ```

    -   使用规则后产生新数据：

        ``` {#codeblock_47s_a2d_sy3}
        # 下图为4条日志
        time:"1566379109"
        data:"test-one"
        cid:"1"
        eid:"00001"
        provice:"上海"
        city:"上海"
        population:"2000"
        
        time:"1566379111"
        data:"test_second"
        cid:"1"
        eid:"12345"
        provice:"上海"
        city:"上海"
        population:"2000"
        
        time:"1566379111"
        data:"test_three"
        cid:"2"
        eid:"12345"
        provice:"河南"
        city:"郑州"
        population:"3000"
        
        time:"1566379113"
        data:"test_four"
        cid:"2"
        eid:"12345"
        provice:"河南"
        city:"郑州"
        population:"3000"
        
        ......
        ```

        **说明：** 在对数据库表中数据的`cid`的值进行等值匹配的时候，有多个`cid`值为1的行，但是我们只获取匹配到第一行的数据，对后续的数据并没有进行处理，这就是当前`e_table_map`的规则特点。首先要单个或多个字段的等值匹配，其次`e_table_map`规则目前只支持匹配一行数据结果。后续我们会支持多行匹配的形式，目前如果想要使用多行匹配，将匹配到的所有数据组合成新的数据，`e_search_table_map`可以解决这个问题。

-   使用e\_search\_table\_map进行字符串搜索匹配以及多行匹配。
    -   源Logstore原始日志数据：

        ``` {#codeblock_9xt_blh_68f}
        time:1563436326
        data:123
        city:nanjing
        province:jiangsu
        ```

    -   MYSQL 数据库表中数据：

        |content|name|age|
        |-------|----|---|
        |city~=n\*|aliyun|10|
        |province~=su$|Maki|18|
        |city:nanjing|vicky|20|

    -   ETL加工编排。

        根据指定的mysql表中的字段的值去匹配原始日志，其中指定mysql字段的值为`key/value`形式，value为正则表达式，key为匹配原始日志中字段的值。根据匹配结果，将mysql指定字段的值添加到原始日志中，支持多行匹配，将多个值添加到原始日志的一个字段中。

    -   样例一：单行匹配，匹配到mysql表中一行就返回，不会继续匹配。

        ``` {#codeblock_6z9_x5j_21e}
        e_search_table_map(res_rds_mysql(address="rds-host", username="mysql-username",password="xxx",database="xxx",table="xx",refresh_interval=60),"content","name")
        ```

        **说明：** 

        -   使用了`e_search_table_map`语法，详细请参见[e\_search\_table\_map](../../../../cn.zh-CN/数据加工/数据加工语法/全局操作函数/映射富化函数.md#section_mp3_goc_rxa)。
        -   `res_rds_mysql()`里填入的是从RDS MYSQl获取数据的配置，该函数会获取指定的mysql表格，具体语法请参见[res\_rds\_mysql](cn.zh-CN/数据加工/数据加工语法/表达式函数/资源函数.md#section_49h_ufh_ptu)。
        -   `content`字段指定的是mysql表中的字段，会使用该字段的值的内容去匹配原始日志中的内容，具体匹配规则请参见[e\_search](../../../../cn.zh-CN/数据加工/数据加工语法/表达式函数/事件检查函数.md#section_syl_ku4_k84)，支持正则、完全匹配、模糊匹配等形式。
        DSL 编排结果：

        匹配查找原始日志中以n开头的`city`字段的值，找到后将表中的`name`字段的值添加到原始日志中。

        ``` {#codeblock_29w_u0l_k7e}
        time:1563436326
        data:123
        city:nanjing
        province:jiangsu
        name:aliyun
        ```

    -   样例二：多行匹配，遍历mysql表中所有行，将匹配到的数据，添加到指定字段中。

        在参数中添加了`multi_match=True`和`multi_join=","`两个字段，分别表示开启多行匹配，匹配到多个值时使用逗号进行组合，使用该语法会将匹配到的所有行的内容进行添加。

        ``` {#codeblock_ler_ctt_st2}
        e_search_table_map(res_rds_mysql(address="rds-host", username="mysql-username",password="xxx",database="xxx",table="xx",refresh_interval=60),"content","name",multi_match=True,multi_join=",")
        ```

        DSL 编排结果：

        该语法会分别按照符合`e_search`规则的方式去查找`city=n*`、`province=su$`和`city:nanjing`，其中`~=`后面是正则表达式，冒号`:`表示是否包含该字段的用法。mysql表中按照规则分别匹配中三行，所以将三个`name`值按照逗号分隔添加到原始日志中。

        ``` {#codeblock_d6c_a61_eug}
        time:1563436326
        data:123
        city:nanjing
        province:jiangsu
        name:aliyun,Maki,vicky
        ```


