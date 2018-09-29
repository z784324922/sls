# Create a machine group {#task_wc3_xn1_ry .task}

Log Service manages all the Elastic Compute Service \(ECS\) instances whose logs need to be collected by using the Logtail client in the form of machine groups.

After creating a Logtail configuration, you can create a machine group on the  Machine Groups page,  In addition, you can also follow the page prompts in Apply to the machine groups page, and click **Create Machine Group** to create a machine group.

You can define a machine group by using:

-   defines the name of the machine group and adds the intranet IP addresses of the machines in the group.

    You can add multiple ECS instances to a machine group to unify their Logtail configurations by adding the intranet IP addresses of Alibaba Cloud ECS instances.  For how to create a machine group for non-ECS instances, see [Collect logs from non-Alibaba Cloud ECS instances or ECS instances not in your account](reseller.en-US/User Guide/Logtail collection/Machine Group/Collect logs from non-Alibaba Cloud ECS instances or ECS instances not in your account.md) .

-   User-defined identity: Define an identity for the machine group and configure the identity on the corresponding machine for association.

    The system is composed of multiple modules. Each part of each module is horizontally scalable, and can contain multiple machines. To collect logs separately, you can create a machine group for each module.  Therefore, you must create a user-defined identity for each module and configure the identity on the servers of each module.  For example, generally, a website is composed of frontend  HTTP request processing module, cache module, logic processing module, and storage module. The user-defined identities can be defined as http\_module, cache\_module, logic\_module, and store\_module.


1.   Log on to the Log Service console. Go to  the Project Listpage. Click the project name. Go to the Logstore list page. 
2.   On the Logstore List page, click **LogHub - Collect** \> **Logtail Machine Group** The  Machine Groups page appears. **Click Create Machine Group** in the upper-right corner. You can also click Create Machine Group on the  **Apply to Machine Group** page in the data import wizard.
3.   Enter the Group Name. The name can be 3–128 characters long, contain lowercase letters, numbers, hyphens \(-\), and underscores \(\_\), and must begin and end with a lowercase letter or number.
4.   Select the Machine Group Identification. 
    -   **IPs**

        With this option selected, enter the intranetIP addresses  of the ECS instances in the IPs field.

        **Note:** 

        -   Make sure that the entered ECS instance belongs to the current logon Alibaba Cloud account.
        -   Make sure that the entered ECS instance and the current Log Service project are in the same Alibaba Cloud region.
        -   Make sure that you use the intranet IP address of the ECS instance. Use a line break to separate multiple IP addresses.
        -   The Windows ECS instances and Linux ECS instances cannot be added to the same machine group.
        -   Currently, Log Service has disabled the function of installing the Logtail client remotely by using the cloud shield. To install Logtail, see [Linux ](reseller.en-US/User Guide/Logtail collection/Install/Linux .md).
        ![](images/5277_en-US.png "IP address")

    -   **User-defined Identity**

        Enter your custom identity in the  User-defined Identity field, With this option selected.

        Before performing this step, confirm that you have created a user-defined identity on the server from which logs are to be collected.  For how to use the user-defined identity, see [Configure a user-defined identity for a machine group](reseller.en-US/User Guide/Logtail collection/Machine Group/Configure a user-defined identity for a machine group.md).

        To expand machines for a module, for example, to add servers for a frontend module, install Logtail and create a file whose user-defined identity is `http_module` on the servers to be added to automatically synchronize the configurations of different machine groups. After the successful operation, clickMachine Status to view the added machines.

        ![](images/5278_en-US.png "Custom identity")

5.   er the  **Machine Group Topic**. 
6.   Click **OK**. You can now view the machine group you just created on the machine group list.

    ![](images/5279_en-US.png "Machine Group list")


After creating a machine group, you can view the machine group list, modify the machine group, view the status, manage configurations, and delete the machine group.

