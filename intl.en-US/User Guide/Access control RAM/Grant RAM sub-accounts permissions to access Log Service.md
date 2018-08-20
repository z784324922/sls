# Grant RAM sub-accounts permissions to access Log Service {#task_xsk_ttc_ry .task}

In actual scenarios, a main account may allocate the Operation & Maintenance \(O&M\) jobs of Log Service to its Resource Access Management \(RAM\) sub-accounts, enabling the sub-accounts to perform routine O&M on Log Service. Alternatively, RAM sub-accounts under a main account may want to access Log Service resources. In this case, the main account must authorize its RAM sub-accounts to access or perform operations in Log Service.  For security reasons, we recommend that you grant the minimum permissions to RAM sub-accounts within the required scope.

To authorize RAM sub-accounts to access Log Service resources by using a main account, follow these steps.  For more information about the RAM sub-accounts, see [Introduction](../../../../intl.en-US/Quick Start/Introduction.md).

1.  Create a RAM sub-account 
    1.  Log on to the RAM console. 
    2.  Click **Users** in the **left-side navigation pane**. Click Create User in the upper-right corner. 
    3.  Enter the user information. Select the **Automatically generate an AccessKey** for this user check box and then click **OK**. 
2.  Grant sub-accounts permissions to access Log Service resources Log Service provides two system authorization policies: `AliyunLogFullAccess` and `AliyunLogReadOnlyAccess`. These two authorization policies respectively indicate the Full Access permission and Read-Only permission. You can also customize authorization policies in the RAM console. For how to create an authorization policy, see [Create a custom authorization policy](../../../../intl.en-US/User Guide/Authorization/Authorization Policy Management.md). For examples of authorization policies, see [RAM authorization policies of Log Service](../../../../intl.en-US/API Reference/RAM subaccount access/Overview.md). This document introduces how to grant the Read-Only permission to sub-accounts.
    1.  On the **User Management** page, click **Authorize** at the right of the sub-account.  The Edit User-Level Authorization dialog box appears. 
    2.  Select **AliyunLogReadOnlyAccess**under Available Authorization Policy Names 
3.  Log on to the console as a sub-account The sub-account has the permission to access Log Service console after being created and authorized. You can log on to the console as a RAM sub-account in the following ways:
    1.  On the RAM Overview page in the RAM console, click the RAM user logon link and use the sub-account username and password created in step 1 to log on to the **console**. 

         ![](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/pic/47664/cn_zh/1483463236703/%E5%AD%90%E7%94%A8%E6%88%B71.png "RAM user") 

    2.  Access the sub-account logon page and use the sub-account username and password created in step 1 to log on to the [console](https://signin.aliyun.com/?spm=a2c4g.11186623.2.8.syJBhv). By default, the **Enterprise Alias**  is the account ID \(ali uid\) of the main account. You can view and configure your enterprise alias by navigating to **Settings** \> **Enterprise Alias Settings** in the RAM console.

**Examples of custom authorization policies**

**Example 1**

For example, use the main account to grant permissions to the sub-accounts so that they can:

1.  View what Log Service projects the main account has.
2.  Have the Read-Only permission to a specified Log Service project of the main account.

The authorization policy that meets these two conditions at the same time is as follows:

```
{
   "Version": "1",
   "Statement": [
     {
       "Action": ["log:ListProject"],
       "Resource": ["acs:log:*:*:project/*"],
       "Effect": "Alow"
      },
     {
       "Action": [
         "log:Get*",
         "log:List*"
       ],
       "Resource": "acs:log:*:*:project/<指定的project名称>/*",
       "Effect": "Allow"
     }
   ]
 }
```

**Example 2**

1.  View what Log Service projects the main account has.
2.  Have the Read-Only permission to specified Logstores , savedsearch and dashboards of a specified Log Service project of the main account.

```
{
  "Version": "1",
  "Statement": [
    {
      "Action": [
        "log:ListProject"
      ],
      "Resource": "acs:log:*:*:project/*",
      "Effect": "Allow"
    },
    {
      "Action": [
        "log:List*"
      ],
      "Resource": "acs:log:*:*:project/<指定的Project名称>/logstore/*",
      "Effect": "Allow"
    },
    {
      "Action": [
        "log:Get*",
        "log:List*"
      ],
      "Resource": [
        "acs:log:*:*:project/<指定的Project名称>/logstore/<指定的Logstore名称>"
      ],
      "Effect": "Allow"
    },
    {
      "Action": [
        "log:Get*",
        "log:List*"
      ],
      "Resource": [
        "acs:log:*:*:project/<指定的Project名称>/dashboard/*",
        "acs:log:*:*:project/<指定的Project名称>/dashboard/<指定的仪表盘名称>"
      ],
      "Effect": "Allow"
    },
    {
      "Action": [
        "log:Get*",
        "log:List*"
      ],
      "Resource": [
        "acs:log:*:*:project/<指定的Project名称>/savedsearch/*",
        "acs:log:*:*:project/<指定的Project名称>/savedsearch/<指定的快速查询名称>"
      ],
      "Effect": "Allow"
    }
  ]
}
```

