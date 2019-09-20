# 从其他Logstore获取数据做富化 {#concept_2054817 .concept}

本文档介绍如何通过资源函数从其他Logstore中获取数据对日志数据进行富化。

资源函数`res_log_logstore_pull`支持使用两种模式从其他Logstore中获取数据。

-   获取指定时间间隔内Logstore的数据内容。
-   不设置结束时间，持续获取目标Logstore内容。

关于函数的更多详细说明请参见[res\_log\_logstore\_pull](cn.zh-CN/数据加工/数据加工语法/表达式函数/资源函数.md#section_b3c_kth_p0t)。

**说明：** `res_log_logstore_pull`是一个单独的语法，只负责从目标Logstore获取数据，自己本身并没有做任何富化的操作，所以不建议单独使用`res_log_logstore_pull`，结合`e_table_map`和`e_search_table_map`语句一起使用才有意义。

## 样例数据 {#section_rds_7x6_s7v .section}

假设我们有两个Logstore，`source_logstore`是存储个人信息，`target_logstore`是酒店存储客人入住信息 。现在我们将酒店的入住信息拿来做富化。

**说明：** 此处采用`pull_log`接口获取数据，富化的Logstore并不依赖索引。

个人信息`source_logstore`。

``` {#codeblock_zd8_lgz_dv7}
topic:xxx
city:xxx
cid:12345
name:maki


topic:xxx
city:xxx
cid:12346
name:vicky

topic:xxx
city:xxx
cid:12347
name:mary
```

酒店入住信息`target_logstore`。

``` {#codeblock_jn3_lwg_df3}
time:1567038284
status:check in
cid:12345
name:maki
room_number:1111

time:1567038284
status:check in
cid:12346
name:vicky
room_number:2222

time:1567038500
status:check in
cid:12347
name:mary
room_number:3333

time:1567038500
status:leave
cid:12345
name:maki
room_number:1111
```

基本语法。

``` {#codeblock_66a_avr_ejb}
res_log_logstore_pull(
        endpoint,
        ak_id,
        ak_secret,
        project,
        logstore,
        fields,
        from_time=None,
        to_time=None,
        fetch_include_data=None,
        fetch_exclude_data=None,
        primary_keys=None,
        delete_data=None,
        refresh_interval_max=60,
        fetch_interval=2):
```

## 获取指定时间内所有数据 {#section_499_1iz_00f .section}

**说明：** 此处的时间是日志获取时间。

-   DSL编排语法。

    ``` {#codeblock_obt_6qo_3u7}
    res_log_logstore_pull(..., ["cid","name","room_number"],from_time=1567038284,to_time=1567038500)
    ```

-   获取到的数据。

    ``` {#codeblock_imm_30h_wwa}
    #这里我们的语法中field填入了cid,name,room_number三个字段，并且指定了时间范围，将会获取这个时间范围内,Logstore所有数据的这三个字段的值
    
    cid:12345
    name:maki
    room_number:1111
    
    cid:12346
    name:vicky
    room_number:2222
    
    cid:12347
    name:mary
    room_number:3333
    
    cid:12345
    name:maki
    room_number:1111
    ```


## 设置黑白名单过滤数据 {#section_2wr_jew_3yy .section}

-   设置白名单。
    -   DSL 编排语法。

        ``` {#codeblock_yub_ya3_ndz}
        # 设置白名单，只有room_number值等于1111的数据会被拉取下来。
        res_log_logstore_pull(..., ["cid","name","room_number"，"status"],from_time=1567038284,to_time=1567038500,fetch_include_data="room_number:1111")
        ```

    -   获取到的数据。

        ``` {#codeblock_p9o_iar_uot}
        # 设置了ferch_include_data白名单，只有包含room_numver:1111的数据会被拉取下来，其他数据不会被拉取。
        
        status: check in
        cid:12345
        name:maki
        room_number:1111
        
        status:leave
        cid:12345
        name:maki
        room_number:1111
        ```

-   设置黑名单。
    -   DSL 编排语法。

        ``` {#codeblock_v3q_707_e51}
        res_log_logstore_pull(..., ["cid","name","room_number"，"status"],from_time=1567038284,to_time=1567038500,fetch_exclude_data="room_number:1111")
        ```

    -   获取到的数据。

        ``` {#codeblock_axu_i46_snl}
        # 设置黑名单fetch_exclude_data，当数据包含room_number:1111的时候丢弃这条数据。
        status:check in
        cid:12346
        name:vicky
        room_number:2222
        
        
        status:check in
        cid:12347
        name:mary
        room_number:3333
        ```

-   同时设置黑白名单。
    -   DSL编排语法。

        ``` {#codeblock_8gw_ubx_0o8}
        res_log_logstore_pull(..., ["cid","name","room_number"，"status"],from_time=1567038284,to_time=1567038500,fetch_exclude_data="status:leave",fetch_include_data="status:check in")
        ```

    -   获取到的数据。

        ``` {#codeblock_3ir_b02_lni}
        # 黑白名单同时存在的情况下，优先进行黑名单数据的匹配，然后在匹配白名单。
        #黑名单填入的是status:leave的值，当数据包含status:leave的值时候，数据会被直接丢弃。
        #然后匹配白名单，白名单我们填入的是status:check in当数据包含status: check in的值时候，该数据才会被拉取下来。
        status:check in
        cid:12345
        name:maki
        room_number:1111
        
        
        status:check in
        cid:12346
        name:vicky
        room_number:2222
        
        
        status:check in
        cid:12347
        name:mary
        room_number:3333
        ```


## 持续拉取目标Logstore数据 {#section_g22_mkt_xhf .section}

如果目标Logstore的数据是持续写入，我们需要持续的去拉取的时候，设置`to_time`参数为None就可以。同时可以通过`fetch_interval`设置拉取的时间间隔，通过`refresh_interval_max`设置当拉取遇到错误时重试的最大时间间隔。

DSL编排语法。

``` {#codeblock_kwc_bkm_3d8}
res_log_logstore_pull(..., ["cid","name","room_number"，"status"],from_time=1567038284,to_time=None,fetch_interval=15,refresh_interval_max=60)
# 需要注意的是，在持续拉取的过程中，如果遇到错误，服务器会一直退火重试，直到成功为止，不会停止数据加工进程。
```

## 开启主键维护获取目标Logstore数据 {#section_t6u_gxk_ntw .section}

-   注意事项：

    目前该功能仅支持所有数据存储在目标Logstore的一个Shard中，否则请不要使用该功能。多Shard场景会在以后版本中进行支持。

-   背景：

    以上述个人信息`source_logstore`和酒店信息`target_logstore`的数据为例。因为Logstore中的数据只能写入无法删除，而有时候我们希望不要匹配已经删除的数据，此时就需要开启主键维护功能。

-   需求演示：

    我们需要获取酒店信息`target_logstore`中，已经入住但还没离开的客人信息。当`status=leave`时表示客人已经离开酒店，不需要拉取该信息。

-   DSL编排语法：

    ``` {#codeblock_jcm_4wu_xls}
    res_log_logstore_pull(..., ["cid","name","room_number"，"status","time"],from_time=1567038284,to_time=None,primary_keys="cid",delete_data="status:leave")
    ```

-   得到的数据：

    ``` {#codeblock_f2b_8yu_dfa}
    ## 可以看到name为maki的客人的最后更新status为leave ,已经离开酒店，所以并没有将maki的数据拉取下来。
    time:1567038284
    status:check in
    cid:12346
    name:vicky
    room_number:2222
    
    time:1567038500
    status:check in
    cid:12347
    name:mary
    room_number:3333
    ```


**说明：** `primary_keys`目前只支持设置单字符串。需要设置Logstore数据中值唯一的字段，例如样例中的`cid`，类似数据库的唯一主键。当设置`primary_keys`的时候，`delete_data`也必须不为None，这样才有意义。

## 使用函数做数据富化 {#section_n9e_p17_l0q .section}

-   使用e\_table\_map函数。
    -   DSL编排语法

        ``` {#codeblock_nqs_k1f_9l9}
        # 使用e_table_map做富化
        e_table_map(res_log_logstore_pull(...,
                fields=["cid","room_number"],
                from_time="begin",
                ), "cid","room_number")
        ```

    -   富化后数据

        使用e\_table\_map将拉取的酒店信息和用户信息作了富化，将每个人居住的房间号`room_number`通过`cid`字段富化到了个人信息的数据当中。关于函数的使用请参见[e\_table\_map](cn.zh-CN/数据加工/数据加工语法/全局操作函数/映射富化函数.md#section_s80_usp_myx)。

        ``` {#codeblock_e2t_jzh_6mr}
        topic:xxx
        city:xxx
        cid:12345
        name:maki
        room_nuber:1111
        
        topic:xxx
        city:xxx
        cid:12346
        name:vicky
        room_number:2222
        
        topic:xxx
        city:xxx
        cid:12347
        name:mary
        room_number:3333
        ```

-   使用e\_search\_table\_map函数。
    -   DSL编排语法

        ``` {#codeblock_itd_goo_68o}
        # 使用e_search_table_map做富化
        e_search_table_map(res_log_logstore_pull(...,
                fields=["cid","room_number"],
                from_time="begin",
                ), "cid=12346","room_number")
        ```

    -   富化后的数据

        使用e\_search\_table\_map函数将个人信息和酒店入住信息做搜索匹配，将匹配酒店入住数据中`cid=12346`的数据，并且将命中的数据的`room_number`字段添加到源数据中。关于函数的使用请参见[e\_search\_table\_map](cn.zh-CN/数据加工/数据加工语法/全局操作函数/映射富化函数.md#section_mp3_goc_rxa)。

        ``` {#codeblock_unz_w81_lqb}
        topic:xxx
        city:xxx
        cid:12345
        name:maki
        room_nuber:2222
        
        topic:xxx
        city:xxx
        cid:12346
        name:vicky
        room_number:2222
        
        topic:xxx
        city:xxx
        cid:12347
        name:mary
        room_number:2222
        ```


