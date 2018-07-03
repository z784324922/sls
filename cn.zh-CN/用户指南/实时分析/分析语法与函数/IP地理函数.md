# IP地理函数 {#concept_wmd_slq_zdb .concept}

IP 识别函数，可以识别一个IP是内网IP还是外网IP，也可以判断IP所属的国家、省份、城市。

|函数名|含义|样例|
|:--|:-|:-|
|`ip_to_domain(ip)`|判断IP所在的域，是内网还是外网。返回intranet或internet。|`SELECT ip_do_domain(ip)`|
|`ip_to_country(ip)`|判断IP所在的国家。|`SELECT ip_to_country(ip)`|
|`ip_to_province(ip)`|判断IP所在的省份。|`SELECT ip_to_province(ip)`|
|`ip_to_city(ip)`|判断IP所在的城市。|`SELECT ip_to_city(ip)`|
|`ip_to_geo(ip)`|判断IP所在的城市的经纬度，范围结果格式为`纬度,经度`。|`SELECT ip_to_geo(ip)`|
|`ip_to_city_geo(ip)`|判断IP所在的城市的经纬度，返回的是城市经纬度，每个城市只有一个经纬度，范围结果格式为`纬度,经度`。|`SELECT ip_to_city_geo(ip)`|
|`ip_to_provider(ip)`|获取IP对应的网络运营商。|`SELECT ip_to_provider(ip)`|
|`ip_to_country(ip,'en')`|判断IP所在的国家，返回国际码。|`SELECT ip_to_country(ip,'en')`|
|`ip_to_country_code(ip)`|判断IP所在的国家，返回国际码。|`SELECT ip_to_country_code(ip)`|
|`ip_to_province(ip,'en')`|判断IP所在的省份，返回英文省名或者中文拼音。|`SELECT ip_to_province(ip,'en')`|
|`ip_to_city(ip,'en')`|判断IP所在的城市，返回英文城市名或者中文拼音。|`SELECT ip_to_city(ip,'en')`|

## 示例 {#section_bzf_bzk_5cb .section}

-   在查询中过滤掉内网访问请求，看请求总数

    ```
    * | selectcount(1)whereip_to_domain(ip)!='intranet'
    ```

-   查看Top10的访问省份

    ```
    * | SELECT count(1) as pv, ip_to_province(ip) as province GROUP BY province order by pv desc limit 10
    ```

    响应结果样例

    ```
    [
        {
            "__source__": "",
            "__time__": "1512353137",
            "province": "浙江省",
            "pv": "4045"
        }, {
            "__source__": "",
            "__time__": "1512353137",
            "province": "上海市",
            "pv": "3727"
        }, {
            "__source__": "",
            "__time__": "1512353137",
            "province": "北京市",
            "pv": "954"
        }, {
            "__source__": "",
            "__time__": "1512353137",
            "province": "内网IP",
            "pv": "698"
        }, {
            "__source__": "",
            "__time__": "1512353137",
            "province": "广东省",
            "pv": "472"
        }, {
            "__source__": "",
            "__time__": "1512353137",
            "province": "福建省",
            "pv": "71"
        }, {
            "__source__": "",
            "__time__": "1512353137",
            "province": "阿联酋",
            "pv": "52"
        }, {
            "__source__": "",
            "__time__": "1512353137",
            "province": "美国",
            "pv": "43"
        }, {
            "__source__": "",
            "__time__": "1512353137",
            "province": "德国",
            "pv": "26"
        }, {
            "__source__": "",
            "__time__": "1512353137",
            "province": "吉隆坡",
            "pv": "26"
        }
    ]
    ```

    在上述结果中包含了内网IP，有时候，开发自己的测试是从内网发出的，为了过滤掉这部分访问请求，可以使用下边的分析语句：

-   过滤掉内网请求，查看Top 10的网络访问省份

    ```
    * | SELECT count(1) as pv, ip_to_province(ip) as province WHERE ip_to_domain(ip) != 'intranet'  GROUP BY province ORDER BY pv desc limit 10
    ```

-   查看不同国家的平均响应延时，最大响应延时，最大延时对应的request

    ```
    * | SELECT AVG(latency),MAX(latency),MAX_BY(requestId, latency) ,ip_to_country(ip) as country group by country limit 100
    ```

-   查看不同网络运营商的平均延时

    ```
    * | SELECT AVG(latency) , ip_to_provider(ip) as provider group by provider limit 100
    ```

-   查看IP的经纬度，绘制地图

    ```
    * | select count(1) as pv , ip_to_geo(ip) as geo group by geo order by pv desc
    ```

    返回的格式为：

    |pv|geo|
    |:-|:--|
    |100|35.3284,-80.7459|


