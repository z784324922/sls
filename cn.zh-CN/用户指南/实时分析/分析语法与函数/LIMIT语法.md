# LIMIT语法 {#reference_kfl_2mq_d2b .reference}

LIMIT语法用于限制输出结果的行数。

## 语法格式 {#section_ex2_54x_zdb .section}

日志服务支持以下两种LIMIT语法格式。

-   只读取前N行：

    ```
    limit N
    ```

-   从S行开始读，读取N行：

    ```
    limit S , N
    ```


**说明：** 

-   limit 翻页读取时，只用于获取最终的结果，不可用于获取SQL中间的结果。
-   不支持将limit语法用于子查询内部。例如：

    ```
    * | select count(1) from ( select distinct(url) from limit 0,1000)
    ```


## 示例 {#section_ncm_rdg_v2b .section}

-   只获取100行结果：

    ```
    * | select distinct(url) from log limit 100
    ```

-   获取0行到第999行的结果，共计1000行：

    ```
    * | select distinct(url) from log limit 0,1000
    ```

-   获取第1000行到第1999行的结果，共计1000行：

    ```
    * | select distinct(url) from log limit 1000,1000
    ```


