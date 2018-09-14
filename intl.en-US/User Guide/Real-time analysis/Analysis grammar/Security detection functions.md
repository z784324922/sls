# Security detection functions {#concept_zwy_cmb_r2b .concept}

Based on the global white hat shared security asset library, Log Service provides security detection functions. All you need to do is to pass any IP address, domain name, or URL in the log to security detection functions, you can detect whether it is secure or not.

## Scenarios {#section_inh_fmb_r2b .section}

1.  Enterprises and institutions that have a strong demand for service operation and maintenance, such as enterprises of Internet, games, information, and more. The IT and security Operation and Maintenance \(O&M\) personnel of these industries can use security detection functions to timely filter for suspicious accesses, attacks, and intrusions. The security detection function also supports further in-depth analysis and measures to defend against them.
2.  Enterprises and institutions that have strong demand for internal asset protection, such as banks, securities, e-commerce, and more. Their IT and security O&M personnel can instantly discover internal access to dangerous websites, download the trojan horse, and more, and take immediate action.

## Features {#section_hfx_fmb_r2b .section}

-   Reliable: Relies on the global shared white hat security asset library with timely update.
-   Fast: Takes only a few seconds to detect millions of IP address, domain names, or URLs.
-   Simple: Seamlessly supports any network log. The result can be obtained by calling three SQL functions: security\_check\_ip, security\_check\_domain, and security\_check\_url.
-   Flexible: Supports both interactive queries and building report views. You can configure alarms and take further action.

## Function list {#section_g4z_gmb_r2b .section}

|Function name|Description|Example|
|:------------|:----------|:------|
|security\_check\_ip|Check if the IP address is secure, where:-   Return 1: Hit, indicating insecure
-   Return 0: Missing

|`select security_check_ip(real_client_ip)`|
|security\_check\_domain|Check if the domain is secure, where:-   Return 1: Hit, indicating insecure
-   Return 0: Missing

|`select security_check_domain(site)`|
|security\_check\_url|Check if the URL is secure, where:-   Return 1: Hit, indicating insecure
-   Return 0: Missing

|`select security_check_domain(concat(host, url)`|

## Example {#section_cnr_3mb_r2b .section}

-   **Check external suspicious access behavior and generate reports**

    An e-commerce collects logs of the Ngnix server that it operates, and intends to scan clients that access the server to check if insecure client IP addresses exist. In this case, pass the ClientIP field in the Ngnix log to the `security_check_ip` funciton, display IP addresses whose return value is 1, and show the country, the network operator and other related information of the IP addresses.

    The query analysis statement is:

    ```
    * | select ClientIP, ip_to_country(ClientIP) as country, ip_to_provider(ClientIP) as provider, count(1) as PV where security_check_ip(ClientIP) = 1 group by ClientIP order by PV desc
    ```

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17042/15369087358689_en-US.png)

    Set to map view display:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17042/15369087358690_en-US.png)

-   **Check internal suspicious access behavior and configure alarms**

    For example, a securities operator collects network traffic logs recorded when its internal devices access the external network through a gateway proxy. To check if someone has accessed websites with problems, perform the following query:

    ```
    * | select client_ip, count(1) as PV where security_check_ip(remote_addr) = 1 or security_check_site(site) = 1 or security_check_url(concat(site, url)) = 1 group by client_ip order by PV desc
    ```

    You can also save this statement as a quick query and configure a security alarm. When a client access dangerous websites frequently, the alarm is triggered. Configure 5-minute intervals for checking if someone has accessed dangerous websites frequently \(more than 5 times\) during the past one hour. Change parameters based on your needs. The configuration is as follows:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17042/15369087358691_en-US.png)


