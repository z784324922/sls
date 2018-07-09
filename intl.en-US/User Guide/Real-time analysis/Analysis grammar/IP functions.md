# IP functions {#concept_wmd_slq_zdb .concept}

IP recognition function can recognize whether the IP is an intranet IP or an Internet IP, and can determine the country, province, and city to which the IP belongs.

|Function name|Meaning|Example|
|:------------|:------|:------|
|`ip_to_domain(ip)`|Determines the domain in which the IP resides and whether the IP is an intranet IP or an Internet IP.  The returned value is intranet or Internet.|`SELECT ip_do_domain(ip)`|
|`ip_to_country(ip)`|Determines the country in which the IP resides.|`SELECT ip_to_country(ip)`|
|`ip_to_province(ip)`|Determines the province in which the IP resides.|`SELECT ip_to_province(ip)`|
|`ip_to_city(ip)`|Determines the city in which the IP resides. |`SELECT ip_to_city(ip)`|
|`ip_to_geo(ip)`|Determines the longitude and latitude of the city where IP is located, the result of the range is in the form of `latitude and longitude`.|`SELECT ip_to_geo(ip)`|
|`ip_to_city_geo(ip)`|Determines the longitude and latitude of the city where IP is located. Returns the latitude and longitude of the city, each city has only one latitude and longitude. The result of the range is in the form of `latitude and longitude`.|`SELECT ip_to_city_geo(ip)`|
|`ip_to_provider(ip)`|Obtains the network operator of the IP. |`SELECT ip_to_provider(ip)`|
|`ip_to_country(ip,'en')`|Determines the country where the IP is located and return the international code.|`SELECT ip_to_country(ip,'en')`|
|`ip_to_country_code(ip)`|Determine the country where the IP is located and return the international code.|`SELECT ip_to_country_code(ip)`|
|`ip_to_province(ip,'en')`|Determines the province where the IP is located, and returns the English province name or the Chinese alphabet.|`SELECT ip_to_province(ip,'en')`|
|`ip_to_city(ip,'en')`|Determines the city where IP is located, and returns the English city name or the Chinese alphabet.|`SELECT ip_to_city(ip,'en')`|

## Example {#section_bzf_bzk_5cb .section}

-   Filter out the intranet access requests in the query and view the total number of requests

    ```
    * | selectcount(1)whereip_to_domain(ip)! ='intranet'
    ```

-   View the top 10 access provinces

    ```
    * | SELECT count(1) as pv, ip_to_province(ip) as province GROUP BY province order by pv desc limit 10
    ```

    Response result example:

    ```
    [
        {
            "__source__": "",
            "__time__": "1512353137",
            "province": "Zhejiang province",
            "pv": "4045"
        }, {
            "__source__": "",
            "__time__": "1512353137",
            "province": "Shanghai city",
            "pv": "3727"
        }, {
            "__source__": "",
            "__time__": "1512353137",
            "province": "Beijing city",
            "pv": "954"
        }, {
            "__source__": "",
            "__time__": "1512353137",
            "province": "intranet IP",
            "pv": "698"
        }, {
            "__source__": "",
            "__time__": "1512353137",
            "province": "Guangdong Province ",
            "pv": "472"
        }, {
            "__source__": "",
            "__time__": "1512353137",
            "province": Fujian Province ",
            "pv": "71"
        }, {
            "__source__": "",
            "__time__": "1512353137",
            "province": "United ArabEmirates (UAE)",
            "pv": "52"
        }, {
            "__source__": "",
            "__time__": "1512353137",
            "province": "United States ",
            "pv": "43"
        }, {
            "__source__": "",
            "__time__": "1512353137",
            "province": "Germany ",
            "pv": "26"
        }, {
            "__source__": "",
            "__time__": "1512353137",
            "province": "Kuala Lumpur ",
            "pv": "26"
        }
    ]
    ```

    The preceding results include the intranet IP.  Sometimes developers make tests from the intranet. To filter out these access requests, use the following analysis syntax.

-   Filter out the intranet requests and view the top 10 network access provinces

    ```
    * | SELECT count(1) as pv, ip_to_province(ip) as province WHERE ip_to_domain(ip) ! = 'intranet' GROUP BY province ORDER BY pv desc limit 10
    ```

-   Check the average response latency, the maximum response latency, and the request of the maximum latency in different countries

    ```
    * | SELECT AVG(latency),MAX(latency),MAX_BY(requestId, latency), ip_to_country(ip) as country group by country limit 100
    ```

-   View the average latency for different network operators

    ```
    * | SELECT AVG(latency), ip_to_provider(ip) as provider group by provider limit 100
    ```

-   View the latitude and longitude of the IP, and build a map

    ```
    * | select count(1) as pv , ip_to_geo(ip) as geo group by geo order by pv desc
    ```

    The returned format is:

    |pv|geo|
    |:-|:--|
    |100|35.3284,-80.7459|


