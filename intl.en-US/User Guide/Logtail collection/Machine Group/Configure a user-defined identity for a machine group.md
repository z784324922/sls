# Configure a user-defined identity for a machine group {#concept_gyy_k3q_zdb .concept}

Besides IP addresses, you can use the label user-defined ID to dynamically define the machine group.

User-defined identity is advantageous in following scenarios:

-   In a custom network environment such as Virtual Private Cloud \(VPC\), the IP addresses of different machines may conflict with each other, which makes Log Service fail to manage Logtail. User-defined ID helps to avoid such situation. 
-   Multiple machines use the same label to implement the auto scaling of the machine group. You must only configure the same user-defined ID for the newly added machine. Log Service can automatically identify it, and add to the machine group.

## Procedure { .section}

To use the user-defined identity to dynamically define the machine group, procedure is as follows.

1.  Enable user-defined ID
    -   **Linux Logtail**

        Set the user-defined ID by using the `C:\LogtailData\user_defined_id` file.  

        For example, set a user-defined machine ID as follows:

        ```
        #cat /etc/ilogtail/user_defined_id
        ```

    -   **Windows Logtail**

        Set the user-defined ID by using the `C:\LogtailData\user_defined_id` file.  

        For example, set a user-defined machine ID as follows:

        ```
        C:\LogtailData>more user_defined_id
        aliyun-ecs-rs1e16355
        ```

        Add `aliyun-ecs-rs1e16355` to a machine group. The configuration takes effect in one minute.

        **Note:** If the directory  /etc/ilogtail/ or C:\\LogtailData , file /etc/ilogtail/user\_defined\_id or C:\\LogtailData\\user\_defined\_id does not exist, create it manually.

2.  Create a machine group.
    1.  On the Machine Groups page, click **Create Machine Group** in the upper-right corner.
    2.  Complete the configurations for the machine group.
        -   Group Name: Enter a name for the machine group.
        -   Machine Group Identification:   Select **User-defined Identity**. 
        -   User-defined Identity:   Enter the user-defined ID configured in step 1.
    3.  Click **Confirm** to create the machine group.  To expand machines, complete step 1 on the server to be added.
3.  View machine group status

    On the Machine Groups page, click **Machine Status** at the right of the machine group to view the list of machines that use the same user-defined identity and their heartbeat status.


## Other operations {#section_stq_dcy_pdb .section}

## Disable user-defined ID {#section_ikv_cs1_ry .section}

To use IP address as the machine group identification, delete the user\_defined\_id file. The configuration takes effect in one minute. 

```
rm -f /etc/ilogtail/user_defined_id
```

-   **Linux operating system**

    ```
    rm -f /etc/ilogtail/user_defined_id
    ```

-   **Windows Logtail**

    ```
    Del c: \ logtaildata \ user_defined_id
    ```


## Effective time {#section_x1z_fs1_ry .section}

After you add, delete, or modify the user\_defined\_id file, the latest configuration takes effect in one minute by default.

To bring the configuration into effect immediately, run the following command to restart Logtail:

```
/etc/init.d/ilogtaild stop
/etc/init.d/ilogtaild start
```

-   **Linux operating system**

    ```
    /etc/init.d/ilogtaild stop
    /etc/init.d/ilogtaild start
    ```

-   **Windows Logtail**

    **Navigate to Windows Control Panel** \> **Administrative Tools ** \> **Service **, right-click the LogtailWorker service in the service list and select **Restart** to bring the configuration into effect.


## Examples {#section_qjz_2dy_pdb .section}

Generally, the system is composed of multiple modules. Each module can contain multiple machines, for example, a common website is composed of frontend HTTP request processing module, cache module, logic processing module, and storage module. Each part is horizontally scalable. Therefore, logs must be collected in real time when machines are being added.

1.  Create a user-defined identity. 

    After installing the Logtail client, enable the user-defined ID for the server.   For the modules in the preceding example, the user-defined identities can be defined as http\_module, cache\_module, logic\_module, and store\_module.

2.  Create a machine group.

    Enter the corresponding user-defined identify of the machine group in the **User-defined Identity**field when creating the machine group. See the following configurations of the http\_module machine group. The http\_module Machine Group is shown in the following figure:

    ![](images/5254_en-US.png "Create a machine group. ")

3.  Click Machine Status at the right of the machine group to view the list of machines that use the same user-defined identity and their heartbeat status.
4.  If the frontend module has a machine 10.1.1.3 added, complete step 1 on the newly added machine. After the successful operation, you can view the added machine in the **Machine Group Status** dialog box.

