# Create a machine group with a custom ID as its identifier {#concept_gyy_k3q_zdb .concept}

In addition to IP addresses, you can also use custom IDs as identifiers of machine groups.

The advantages of using custom IDs as identifiers for machine groups are described in the following scenarios:

-   In a custom network, such as a VPC, IP addresses conflicts may occur among server. As a result, Log Service cannot manage Logtail. In this case, custom IDs can be added to the servers to prevent IP address conflicts.
-   A custom ID can be configured for multiple servers to achieve elastic scaling of a machine group. Specifically, you can add the same custom ID to new servers so that Log Service can automatically identify the servers and add them to the machine group.

## Procedure { .section}

1.  Set custom IDs on the servers.

    -   **For Logtail in Linux:**

        Set custom IDs by using the `/etc/ilogtail/user_defined_id` file.

        The following is an example:

        ```
        # vim /etc/ilogtail/user_defined_id
        ```

        Enter `userdefined` in this file.

    -   **For Logtail in Windows:**

        Set custom IDs by using the `C:\LogtailData\user_defined_id` file.

        The following is an example:

        ```
        C:\LogtailData>more user_defined_id
        userdefined_windows
        ```

    **Note:** 

    -   You cannot add Linux and Windows servers into the same machine group. Therefore, you must set different custom IDs for Linux and Windows servers.
    -   When multiple custom IDs are configured for a server, you need to use line breaks to separate them.
    -   If the /etc/ilogtail/ or C:\\LogtailData directory or the /etc/ilogtail/user\_defined\_id or C:\\LogtailData\\user\_defined\_id file does not exist, manually create one.
2.  Create a machine group.
    1.  Log on to the [Log Service console](https://partners-intl.console.aliyun.com/#/sls) and click the project name.
    2.  In the left-side navigation pane, click **Logtail Machine Group**.
    3.  On the Machine Groups page, click **Create Machine Group** the upper-right corner.
    4.  Set the machine group configurations.

        -   **Name**: Enter a name.

            The name must be 3 to 128 characters in length and can contain lowercase letters, numbers, hyphens \(-\), and underscores \(\_\). It must start and end with a lowercase letter or number.

            **Note:** Exercise caution when you set a name for the machine group because the name cannot be changed once it is set.

        -   **Identification**: Select **Custom ID**.
        -   \(Optional\) **Topic**: Enter a topic. For more information about machine group topics, see [Log topic](reseller.en-US/User Guide/Logtail collection/Text logs/Log topic.md).
        -   **Custom ID**: Enter the custom ID obtained from step 1.
        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13078/15541862165254_en-US.png)

    5.  Click **Confirm**.

        **Note:** To scale out a server group, you only need to add the custom ID to the new server.

3.  Check the status of the machine group.

    On the Machine Groups page, locate the target machine group, and click **Status** in the **Actions** column. Then, you can view the list containing all servers using the same custom ID and their heartbeat information.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13078/15541862175255_en-US.png)


## Disable custom IDs {#section_ikv_cs1_ry .section}

If you want use server IP addresses as machine group identifiers, delete the user\_defined\_id file. The setting takes effect within one minute.

-   **In Linux:**

    ```
    rm -f /etc/ilogtail/user_defined_id
    ```

-   **In Windows:**

    ```
    del C:\LogtailData\user_defined_id
    ```


## Effective time {#section_x1z_fs1_ry .section}

By default, addition, deletion, and modification of the user\_defined\_id file take effect within one minute.

If you want the setting to take effect immediately, run the following command and restart Logtail:

-   **In Linux:**

    ```
    /etc/init.d/ilogtaild stop
    /etc/init.d/ilogtaild start
    ```

-   **In Windows:**

    Choose **Control Panel** \> **Administrative Tools** \> **Services**. Then, right-click the **LogtailWorker** service and choose **Restart** from the shortcut menu.


## Example {#section_qjz_2dy_pdb .section}

The system is composed of multiple modules, each of which can contain multiple servers. For example, a common website can be divided into a frontend HTTP request processing module, a cache module, a logic processing module, and a storage module. Each module can be individually expanded. Therefore, you need to enable Log Service to collect logs from new servers in real time.

1.  Create a custom ID.

    After installing the Logtail client, use custom IDs as machine group identifiers. In this example, there are four identifiers \(indicating the four modules\): http\_module, cache\_module, logic\_module, and store\_module

2.  Create a machine group.

    Set **Identification** to the actual custom ID of the machine group. The following figure uses the http\_module machine group as an example.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13078/15541862165254_en-US.png)

3.  In the Machine Group Status dialog box, view the list containing all servers using the same custom ID and their heartbeat information.
4.  Add the custom ID to the 10.1.1.3 server if the server is added to the machine group. Then, you can view the new server in the **Machine Group Status** dialog box.

