# Phone number functions {#concept_a15_tzn_yfb .concept}

Phone number functions are used to query the attributes of phone numbers that are registered in Mainland China.

## Functions {#section_o2q_qfv_yfb .section}

|Function name|Description|Example|
|:------------|:----------|:------|
|mobile\_province|This function is used to query the provincial attribute of a phone number. The phone number must be of the numeric type. If the phone number is of the string type, you can use `try_cast` to convert the type to numeric.| `* | select mobile_province(12345678)`

 `* | select mobile_province(try_cast('12345678' as bigint) )`

 |
|mobile\_city|This function is used to query the urban attribute of a phone number. The phone number must be of the numeric type. If the phone number is of the string type, you can use `try_cast` to convert the type to numeric.| `* | select mobile_city(12345678)`

 `* | select mobile_city(try_cast('12345678' as bigint) )`

 |
|mobile\_carrier|This function is used to query the telecom operator to which a phone number belongs. The phone number must be of the numeric type. If the phone number is of the string type, you can use `try_cast` to convert the type to numeric.| `* | select mobile_carrier(12345678)`

 `* | select mobile_carrier(try_cast('12345678' as bigint) )`

 |

## Scenarios {#section_ymm_tfv_yfb .section}

-   **Query phone number attributes and generate a report.**

    Assume that an e-commerce company collects logs about the activities of its customers. The company can extract the fields involving phone numbers, and then collects the attributes of the phone numbers by using the following query statement:

    ```
    SELECT mobile_city(try_cast("mobile" as bigint)) as "city", mobile_province(try_cast("mobile" as bigint)) as "province", count(1) as "number of requests" group by "province", "city" order by "number of request" desc limit 100 
    ```

    In the statement, `mobile` is used as the input field of the `mobile_city` and `mobile_province` functions to show the provinces and cities of the phone numbers. The returned information from the preceding query is shown in the following figure.

    You can also choose to show the phone number attributes on a map, as shown in the following figure.

-   **Check phone number attributes and report abnormal logon information.**

    If a telecom operator wants to filter its customers whose daily locations are different from their phone number attributes \(according to additional attributes and frequently accessed IP addresses\), the following statement can be used:

    ```
    * | select mobile, client_ip, count(1) as PV where mobile_city(try_cast("mobile" as bigint)) ! = ip_to_city(client_ip) and ip_to_city(client_ip) ! = '' group by client_ip, mobile order by PV desc 
    ```

    Furthermore, you can create alarm rules that use phone number attributes.


