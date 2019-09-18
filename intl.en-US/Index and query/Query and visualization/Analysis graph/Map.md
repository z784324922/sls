# Map {#concept_wdv_gnq_zdb .concept}

You can add color blocks and marks to a map to display geographic data. Log Service provides three kinds of maps: Map of China, World Map, and AMap. Among them, AMap offers the scatter chart and heat map. You can use specific functions in query and analysis statements to display analysis results on different maps.

## Components {#section_uj4_gw1_5db .section}

-   Map canvas
-   Color block

## Properties {#section_iff_hw1_5db .section}

|Configuration item|Description|
|:-----------------|:----------|
|Location information|The location information recorded in logs. The dimension varies with the map as follows: -   Provinces \(Map of China\)
-   Country \(World Map\)
-   Longitude/Latitude \(AMap\)

 |
|Value Column|The data volume of the location information.|

## Procedure {#section_tfu_82w_yva .section}

1.  On the search page of a Logstore, enter a query and analysis statement in the search box, set the time range, and then click **Search & Analysis**.
    -   Map of China: Use the `ip_to_province` function.
    -   World Map: Use the `ip_to_country` function.
    -   AMap: Use the `ip_to_geo` function.
2.  Click ![](https://cdn.yuque.com/lark/2018/png/60648/1523262281180-89fdf120-2b29-448c-acd4-7c30d5b243e4.png) to select the map.
3.  Configure the properties of the map.

## Scenarios {#section_phn_4w1_5db .section}

## Map of China {#section_pj3_pw1_5db .section}

You can use the `ip_to_province` function to generate a map of China.

-   **SQL statement**:

``` {#codeblock_9a5_p2i_lxf}
* | select  ip_to_province(remote_addr) as address, count(1) as count group by address order by count desc limit 10
```

-   **Dataset**:

    |address|count|
    |:------|:----|
    |Guangdong|163|
    |Zhejiang|110|
    |Fujian|107|
    |Beijing|89|
    |Chongqing|28|
    |Heilongjiang|19|

-   Select `address` for **Provinces** and `count` for **Value Column**.

## World map {#section_ylc_ww1_5db .section}

You can use the `ip_to_country` function to generate a world map.

-   **SQL statement**:

    ``` {#codeblock_8tb_dz4_s0i}
    * | select  ip_to_country(remote_addr) as address, count(1) as count group by address order by count desc limit 10
    ```

-   **Dataset**:

    |address|count|
    |:------|:----|
    |China|8354|
    |United States|142|

-   Select `address` for **Country** and `count` for **Value Column**.

## AMap {#section_zyh_1x1_5db .section}

You can use the `ip_to_geo` function to generate an AMap. The address column in the dataset contains the latitude and longitude in sequence, which are separated with a comma \(,\). If the longitude and latitude are separated in the lng column and the lat column, you can use the `concat('lat', ',', lng')` function to merge the two columns.

-   **SQL statement**:

    ``` {#codeblock_eom_k7h_83i}
    * | select  ip_to_geo(remote_addr) as address, count(1) as count group by address order by count desc limit 10
    ```

-   **Dataset**:

    |address|count|
    |:------|:----|
    |39.9289,116.388|771|
    |39.1422,117.177|724|
    |29.5628,106.553|651|
    |30.2936,120.161420|577|
    |26.0614,119.306|545|
    |34.2583,108.929|486|

-   Select `address` for **Longitude/Latitude** and `count` for **Value Column**.

    The scatter chart is used by default. If data points are densely distributed on the map, you can switch to the heat map.


