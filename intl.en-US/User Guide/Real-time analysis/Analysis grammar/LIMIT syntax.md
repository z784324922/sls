# LIMIT syntax {#reference_kfl_2mq_d2b .reference}

Limit syntax is used to limit the number of rows in the output results.

## Syntax format: {#section_ex2_54x_zdb .section}

Log Service supports the following two LIMIT syntax formats.

-   Read only the first N rows:

    ```
    limit N
    ```

-   Read N rows from the S rows:

    ```
    limit S , N
    ```


**Note:** 

-   When you use LIMIT syntax to read results across pages, it is used to get only the final result and cannot be used to get results in the middle of SQL.
-   LIMIT syntax cannot be used in subquery. For example:

    ```
    * | select count(1) from ( select distinct(url) from limit 0,1000)
    ```


## Examples {#section_ncm_rdg_v2b .section}

-   Get only 100 rows of results:

    ```
    * | select distinct(url) from log limit 100
    ```

-   Get results from 0 rows to 999th rows, 1000 rows in total:

    ```
    * | select distinct(url) from log limit 0,1000
    ```

-   Get results from 1000th rows to 1999th rows, 1000 rows in total:

    ```
    * | select distinct(url) from log limit 1000,1000
    ```


