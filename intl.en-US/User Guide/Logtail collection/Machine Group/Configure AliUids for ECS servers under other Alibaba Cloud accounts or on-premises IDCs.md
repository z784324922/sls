# Configure AliUids for ECS servers under other Alibaba Cloud accounts or on-premises IDCs {#task_arl_ynt_qy .task}

If Logtail is installed on ECS servers under other Alibaba Cloud accounts, provided by other cloud vendors, or located in on-premises IDCs, you must configure AliUids for the servers so that they can be added into machine groups for log collection.

If the target server for log collection through Logtail is purchased by another Alibaba Cloud account or provided by another cloud vendor, you need to install Logtail on the server and configure an AliUid for it. By doing so, you grant your Alibaba Cloud account the permissions to access and collect logs from the server. Otherwise, the server does not receive heartbeat information and cannot collect logs.

-   The target server for log collection is under another Alibaba Cloud account, provided by another cloud vendor, or located in an on-premises IDC.
-   The Logtail client is installed on the server.

    For more information, see [Install Logtail in Linux](reseller.en-US/User Guide/Logtail collection/Install/Install Logtail in Linux.md) and [Install Logtail in Windows](reseller.en-US/User Guide/Logtail collection/Install/Install Logtail in Windows.md) as needed.


1.  View the Alibaba Cloud account ID, namely, the AliUid. 

    You can view the AliUid of the account to which the Log Service project belongs on the Account Management page.

    ![](images/5286_en-US.png "View your Alibaba Cloud account ID")

2.  Configure an AliUid for the server. 
    -   **In Linux:**

        Create a file named after the AliUid in the /etc/ilogtail/users directory. If the directory does not exist, you need to create one. You can configure multiple AliUids for a single server by running a command similar to the following:

        ```
        touch /etc/ilogtail/users/1559122535028493
        touch /etc/ilogtail/users/1329232535020452
        ```

        If you do not need Logtail to collect data to your Log Service project, you can delete the AliUid:

        ```
        rm /etc/ilogtail/users/1559122535028493
        ```

    -   **In Windows:**

        Create a file named after the AliUid in the C:\\LogtailData\\users directory.

        If you want to delete the AliUid, you can simply delete this file. \(C:\\LogtailData\\users\\1559122535028493 is used as an example file.\)

        **Note:** 

        -   After an AliUid is configured for a server, the Alibaba Cloud account has the permission to collect logs from the server by using Logtail. You need to delete unnecessary AliUid files from the server in a timely manner.
        -   Addition and deletion of an AliUid take effect within 1 minute.

