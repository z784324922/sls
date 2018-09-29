# Collect logs from non-Alibaba Cloud ECS instances or ECS instances not in your account {#task_arl_ynt_qy .task}

To use Logtail to collect logs from non-Alibaba Cloud Elastic Compute Service \(ECS\) instances or ECS instances that are not created by your account, install Logtail on the server and configure the user identity \(account ID\) to verify that the server can be accessed by your account. Otherwise, the heartbeat status is set to FAIL and Logtail cannot collect data to Log Service. Follow these steps to configure the user identity.

1.   Install Logtail 

    To install the Logtail client on the server where you want to collect logs, see [Linux ](reseller.en-US/User Guide/Logtail collection/Install/Linux .md) if your operating system is Linux, and [Install Logtail on Windows](reseller.en-US/User Guide/Logtail collection/Install/Install Logtail on Windows.md) if your operating system is Windows.

2.   Configure a user identity 
    1.   View your Alibaba Cloud account ID 

        Log on to the Alibaba Cloud Account Management page to view the account ID of your Log Service project.

        ![](images/5286_en-US.png "View account ID")

    2.   Configure account ID identification file on the server 
        -   **Linux operating system**

            Create a file named after the account ID in the /etc/ilogtail/users directory. If the directory does not exist, create it manually. You can configure multiple account IDs on a single machine, for example:

            ```
            touch /etc/ilogtail/users/1559122535028493
            touch /etc/ilogtail/users/1329232535020452
            ```

            If you do not need Logtail to collect data to your Log Service project, you can delete the user identity:

            ```
            rm /etc/ilogtail/users/1559122535028493
            ```

        -   **Windows systems**

            Create a file named after the account ID in the C:\\LogtailData\\users directory to configure the user identity. To delete the user identity, delete this file directly.

            For example, C:\\LogtailData\\users\\1559122535028493.

            **Note:** 

            -   After the user identity \(account ID\) is configured on a machine, the cloud account has the permission to collect logs from the machine by using Logtail. Clear any unnecessary account ID files from machines timely.
            -   Adding and removing user identities can take effect within 1 minute.

