# 从OSS文件获取数据做富化 {#concept_2071592 .concept}

本文档介绍如何通过资源函数从OSS中获取数据对日志数据进行富化。

## 相关配置 {#section_g1t_u8i_23p .section}

1.  创建OSS的AK访问密钥。

    推荐创建一个只读AK和一个只写AK。只读AK主要用于从OSS获取文件，只写AK主要用于将文件上传到OSS。

    创建OSS访问密钥详情请参见[如何构建RAM Policy](../../../../cn.zh-CN/开发指南/权限控制/基于RAM Policy的权限控制/如何构建RAM Policy.md#)。

    创建完成之后，您可以将获取数据的文件上传到OSS存储空间，具体操作请参见[上传文件](../../../../cn.zh-CN/快速入门/上传文件.md#)。

    OSS的Endpoint列表请参见[访问域名和数据中心](../../../../cn.zh-CN/开发指南/访问域名（Endpoint）/访问域名和数据中心.md#)。

2.  在加工规则中使用密钥。

    使用写入AK向OSS中名叫`test`的bucket中传入一个包含`test text`内容的`test.text`文件。

    ![OSS富化规则](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1644556/156898014559594_zh-CN.png)

    截图中加工规则为：

    ``` {#codeblock_85k_khm_or9}
    e_set("test_oss",res_oss_file(endpoint='http://oss-cn-hangzhou.aliyuncs.com',
                                                     ak_id=res_local("AK_ID"),
                                                     ak_key=res_local("AK_KEY"),
                                                     bucket='test', file='test.text',
                                                     format='text',change_detect_interval=0))
    ```

    规则说明：

    -   ``Endpoint代表以上测试的OSS Region属于杭州。
    -   AK信息是只读AK并且是从高级参数配置中获取。
    -   `bucket`代表的是存储空间，具体请参见[基本概念](../../../../cn.zh-CN/开发指南/基本概念.md#)。
    -   `file`代表的是`test.text`。
    -   `format`表示从OSS上获取`test.text`下来的数据格式是text文本格式，还可以使用binary字节流格式。
    获取下来的日志内容为：

    ``` {#codeblock_91b_9be_2ox}
    test_oss: test text
    ```

3.  定期刷新获取所有。

    富化数据如果定期会全量刷新，希望数据加工任务能够自动定期去获取，可以进行如下配置。

    ``` {#codeblock_n57_gh9_xgh}
    e_set("test_oss",res_oss_file(endpoint='http://oss-cn-hangzhou.aliyuncs.com',
                                                     ak_id=res_local("AK_ID"),
                                                     ak_key=res_local("AK_KEY"),
                                                     bucket='test', file='test.text',
                                                     format='text',change_detect_interval=300))
    ```

    上述加工语法会返回一个text文本结构，每隔5分钟重新检查OSS的`test.text`文件内容是否更新，更新的话则重新获取内容。

    `change_detect_interval`：检查是否有更新的时间间隔，如果有更新则重新获取。默认是0，表示不刷新。


## IP富化 {#section_7rt_v0n_67d .section}

对IP地址进行富化，根据IP解析出国家、省、市等信息。

-   原始日志

    ``` {#codeblock_j9y_zg5_e8j}
    ip: 1.2.3.4
    ```

-   LOG DSL编排

    在DSL编排中，使用`res_oss_file`函数获取IP库的时候，需要把返回的数据格式`format`设置为binary格式，自动从OSS上进行获取的时间间隔为200秒。默认情况下，输出的字段为`city_name`、`region_name`、`country_name`，以下编排使用tuple形式进行了重命名操作。本示例使用的是[IP网站](https://user.ipip.net/login.php)免费版，免费版默认输出信息有限制。

    ``` {#codeblock_wjo_3ep_kkd}
    e_set("geo",geo_parse(v("ip"), ip_db=res_oss_file(endpoint='http://oss-cn-hangzhou.aliyuncs.com',
                                                     ak_id='your ak_id',
                                                     ak_key='your ak_key',
                                                     bucket='your bucket', file='ipipfree.ipdb',
                                                                   format='binary',change_detect_interval=200),keep_fields=(("city_name","city"),("country_name","country"),("region_name","provence"))))
    ```

-   富化结果

    ``` {#codeblock_ies_bty_d66}
    ip: 1.2.3.4
    city: 杭州市
    provence: 浙江省
    country: 中国
    ```


## 使用e\_table\_map进行富化 {#section_5hz_e4k_nds .section}

根据输入字段的值，在从OSS上获取的CSV格式的数据中查找对应的行，返回对应字段的信息。更多富化信息请参见[使用搜索映射做高级数据富化](cn.zh-CN/.md#)。

-   原始日志

    ``` {#codeblock_uf6_kfl_std}
    account :  Sf24asc4ladDS
    ```

-   OSS文件数据

    |id|account|nickname|
    |--|-------|--------|
    |1|Sf24asc4ladDS|多弗朗明哥|
    |2|Sf24asc4ladSA|凯多|
    |3|Sf24asc4ladCD|罗杰|

-   LOG DSL编排

    以下DSL编排规则，首先先从OSS上拉取CSV格式的数据，数据格式如上。其次，使用`tab_parse_csv`进行表格构建。最后使用`e_table_map`对数据进行查找并返回相应的信息。

    ``` {#codeblock_nuk_w7a_637}
    e_table_map(tab_parse_csv(res_oss_file(endpoint='http://oss-cn-hangzhou.aliyuncs.com',ak_id=ak_id='your ak_id',ak_key='your ak_key', bucket='your bucket',file='data.csv',change_detect_interval=30)),"account","nickname")
    ```

-   富化结果

    ``` {#codeblock_3ga_rlz_oni}
    account :  Sf24asc4ladDS
    nickname: 多弗朗明哥
    ```


