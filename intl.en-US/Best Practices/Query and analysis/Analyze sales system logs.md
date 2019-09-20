# Analyze sales system logs {#concept_59005_zh .concept}

Bills are the core data of e-commerce companies and the outcome of a series of marketing and promotional activities. This data contains a lot of valuable information. With the information, you can define user profiles, providing guidelines for future marketing plans. The billing data can also serve as a popularity indicator, providing suggestions for subsequent stocking options.

The billing data is stored as logs in Alibaba Cloud Log Service. With a computing capacity of processing hundreds of millions of log entries per second, Log Service supports high-speed queries and SQL-based statistics. This topic uses several examples to describe how to mine useful information.

The following is a complete bill that contains goods information \(name and price\), deal information \(final price, payment method, and discount information\), and buyer information \(membership information\):

``` {#codeblock_a8e_i8s_afl}
__source__:  10.164.232.105      __topic__:  bonus_discount:  category:  men's clothing   commodity:  Every day discount autumn and winter teenager velvet and thickened skinny jeans men's winter slim pants commodity_id:  443 discount:  member_discount:  member_level:  nomember_point:  memberid:  mobile:  pay_transaction_id:  060f0e0d080e0b05060307010c0f0209010e0e010c0a0605000606050b0c0400 pay_with:  alipay real_price:  52.0 suggest_price:  52.0
```

## Statistical analysis {#section_cz5_jsu_opl .section}

Before query and analysis, enable and set indexes. For more information, see [Enable and set indexes](../../../../reseller.en-US/Index and query/Enable and set indexes.md#).

1.  View the percentage of the sales of each category of products to the total sales.

    ``` {#codeblock_rmn_3g7_it4}
    *|select count(1) as pv ,category group by category limit 100                    
    ```

2.  View the sales trends of different women's clothes.

    ``` {#codeblock_4qg_e0f_bsm}
    category: women's clothing  | select count(1) as deals , commodity 
     group by commodity order by deals  desc limit 20                    
    ```

3.  View share and turnover of different payment methods.

    ``` {#codeblock_cjk_2yl_bzs}
    *  | select count(1) as deals , pay_with group by pay_with  order by deals  desc limit 20
    
    *  | select sum(real_price)  as total_money , pay_with group by pay_with  order by total_money  desc limit 20            
    ```

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13209/156894001532465_en-US.png)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13209/156894001532466_en-US.png)


