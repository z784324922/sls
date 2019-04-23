# Overview {#reference_flv_yqn_12b .reference}

## Access Log Service resources of your primary account as a RAM user after RAM authorization {#section_gs2_cd5_12b .section}

The project, Logstore, config, and machinegroup that you create are your own resources. By default, you have the full operation permissions on your resources, and can use all APIs described in this document to perform operations on your resources.

However, in scenarios where a primary account has a sub-account, you cannot use an unauthorized sub-account to perform operations on resources of the primary account. You must grant the permission to the sub-account to perform operations on resources of the primary account using RAM authorization.

**Note:** Before using RAM to grant the sub-account permissions to access Log Service resources of a primary account, make sure that you have carefully read [User](../../../../reseller.en-US//Identities/Users.md) and [Authorization - Overview](../../../../reseller.en-US/User Guide/Access control RAM/Authorization - Overview.md#).

Authorization policies for Log Service are available on RAM console:

-   AliyunLogFullAccess

    This policy is used to grant the sub--account the full permission to access Log Service resources of a primary account. The authorization policy is described as follows:

    ```
      {
        "Version": "1",
        "Statement": [
          {
            "Action": "log:*",
            "Resource": "*",
            "Effect": "Allow"
          }
        ]
      }
    ```

-   AliyunLogReadOnlyAccess

    This policy is used to grant the sub-account the read-only permission for Log Service resources of a primary account. The authorization policy is described as follows:

    ```
     {
        "Version": "1",
        "Statement": [
          {
            "Action": [
              "log:Get*",
              "log:List*"
            ],
            "Resource": "*",
            "Effect": "Allow"
          }
        ]
      }
    ```

-   Upload data to the specified Logstore

    This policy is used to grant the sub-account permission to upload data directly to the specified Logstore by using the API/SDK. The authorization policy is described as follows:

    ```
      {
        "Version": "1",
        "Statement": [
          {
            "Action": [
              "log:Post*",
              "log:BatchPost*"
            ],
            "Resource": ["acs:log:*:*:project/<specific proejct name>/logstore/<specific logstore name>"],
            "Effect": "Allow"
          }
        ]
      }
    ```

-   Query data of a specific Logstore in the console

    This policy is used to grant the sub-account permission to have read-only access to the specified Logstore resources of the primary account \(view and extract logs, and view the Logstore list\). The authorization policy is described as follows:

    ```
      {
        "Version": "1",
        "Statement": [
          {
            "Action": ["log:ListProject", "log:ListLogStores"],
            "Resource": ["acs:log:*:*:project/<specific project name>/*"],
            "Effect": "Allow"
          }，
          {
            "Action": ["log:Get*"],
            "Resource": ["acs:log:*:*:project/<specific proejct name>/logstore/<specific logstore name>"],
            "Effect": "Allow"
          }
        ]
      }
    ```


If you do not need to grant an account the permission to access Log Service resources in another account, you can skip this section. Skipping this section does not affect your understanding and using the remaining parts in the file.

For more information:

-   [Types of Log Service resources that can be authorized in RAM](reseller.en-US/API Reference/RAM__STS/Resource list.md)
-   [Actions in RAM that can be performed on Log Service resources Authentication rules](reseller.en-US/API Reference/RAM__STS/Action list.md)
-   [used when a RAM user accesses the resources of a primary account by using Log Service APIs](reseller.en-US/API Reference/RAM__STS/Authentication rules.md)

