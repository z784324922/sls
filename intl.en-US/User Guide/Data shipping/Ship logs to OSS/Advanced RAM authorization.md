# Advanced RAM authorization {#concept_d2w_k4q_zdb .concept}

Before perform the OSS shipping task, the owner of the OSS bucket must configure [quick authorization](https://ram.console.aliyun.com/?spm=a2c4g.11186623.2.4.OhqCND#/role/authorize?request=%7B%22Requests%22%3A%20%7B%22request1%22%3A%20%7B%22RoleName%22%3A%20%22AliyunLogDefaultRole%22%2C%20%22TemplateId%22%3A%20%22DefaultRole%22%7D%7D%2C%20%22ReturnUrl%22%3A%20%22https%3A//sls.console.aliyun.com/%22%2C%20%22Service%22%3A%20%22Log%22%7D). After the authorization is complete, Log Service of the current account has the permission to write to OSS bucket.

This document describes the RAM authorization for OSS shipping tasks in different scenarios.

-   If you need more fine-grained access control for OSS buckets, see [Modify the authorization policy](#section_bfw_5jf_vdb).
-   If a Log Service project and OSS bucket are not created with the same Alibaba Cloud account, see [Cross-account shipping](#section_nw3_1kf_vdb).
-   If a sub-account must ship log data to OSS bucket that belongs to another Alibaba Cloud account, see [Shipping between sub-account and main account](#section_olj_lkf_vdb).
-   If a sub-account must ship log data of the current main account to the OSS bucket of the same account, see [Grant RAM sub-accounts permissions to access Log Service](reseller.en-US/User Guide/Access control RAM/Grant a RAM user the permissions to access Log Service.md).

## Modify the authorization policy {#section_bfw_5jf_vdb .section}

After [quick authorization](https://ram.console.aliyun.com/?spm=a2c4g.11186623.2.9.OhqCND#/role/authorize?request=%7B%22Requests%22%3A%20%7B%22request1%22%3A%20%7B%22RoleName%22%3A%20%22AliyunLogDefaultRole%22%2C%20%22TemplateId%22%3A%20%22DefaultRole%22%7D%7D%2C%20%22ReturnUrl%22%3A%20%22https%3A//sls.console.aliyun.com/%22%2C%20%22Service%22%3A%20%22Log%22%7D), the role AliyunLogDefaultRole is granted to AliyunLogRolePolicy by default, and has write permission for all OSS buckets of account B.

If you need more fine-grained access control, revoke the AliyunLogRolePolicy authorization from the AliyunLogDefaultRole. See *OSS authorization* to create a more fine-grained permission policy, and authorize the AliyunLogDefaultRole.

## Cross-account shipping {#section_nw3_1kf_vdb .section}

If your Log Service project and OSS bucket are not created with the same Alibaba Cloud account, you must configure the authorization policy in following way.

For example, Log Service data of the account A must be shipped to the OSS bucket created by the account B.

1.  Using [quick authorization](https://ram.console.aliyun.com/?spm=a2c4g.11186623.2.9.OhqCND#/role/authorize?request=%7B%22Requests%22%3A%20%7B%22request1%22%3A%20%7B%22RoleName%22%3A%20%22AliyunLogDefaultRole%22%2C%20%22TemplateId%22%3A%20%22DefaultRole%22%7D%7D%2C%20%22ReturnUrl%22%3A%20%22https%3A//sls.console.aliyun.com/%22%2C%20%22Service%22%3A%20%22Log%22%7D) account B creates the role AliyunLogDefaultRole, and grants write permission to OSS.
2.  In the RAM console, click Role Management on the left-side navigation pane. Then, select AliyunLogDefaultRole, and click the role name to see the basic information.

    In the role description, `Service` configuration indicates the legal user of the role. For example, `log.aliyuncs.com` indicates that the current account can obtain the role to get OSS write permission.

3.  In `Service` configuration, you can modify the role description to add `A_ALIYUN_ID@log.aliyuncs.com`. ID of the main account A can be viewed in the Account Management \> Security Settings.

    For example, ID of the account A is 165421896534\*\*\*\*, and modified description is as follows:

    ```
    {
    "Statement": [
     {
       "Action": "sts:AssumeRole",
       "Effect": "Allow",
       "Principal": {
         "Service": [
           "165421896534****@log.aliyuncs.com",
           "log.aliyuncs.com"
         ]
       }
     }
    ],
    "Version": "1"
    }
    ```

    This role description indicates that account A has the permission to use Log Service to obtain the temporary token to operate the resources of the account B. For more information about the role description, see [Authorization policy management](../../../../reseller.en-US//Authorization management/Policy management.md).

4.  The account A creates a shipping task. When configuring the task, **RAM role** column must be filled with the RAM role identifier ARN of the OSS bucket owner, that is, the RAM role AliyunLogDefaultRole created by account B.

    The ARN of the RAM role can be viewed in the basic information. The format is as follows: `acs:ram::13234:role/logrole`.


## Shipping between sub-account and main account {#section_olj_lkf_vdb .section}

If the sub-account a\_1 of the main account A must use this role to create a shipping rule to ship logs to the OSS bucket of the account B. In this case, the main account A must grant the PassRole permission to the sub-account a\_1.

The configuration is as follows:

1.  Account B configures quick authorization and adds a description to the role. For more information, see [Cross-account shipping](#section_nw3_1kf_vdb).
2.  The main account A logs on to the RAM console and grants AliyunRAMFullAccess permission to the sub-account a\_1.
    1.  On the User Management page, click **Authorization** on the right side of the sub-account a\_1.
    2.  Search for **AliyunRAMFullAccess** in the **authorizable policies**, and add it to **selected policies**. Then click **Confirm**.

        After successful authorization, a\_1 has all RAM permissions.

        To control the permission range of a\_1, the main account A can grant a\_1 only the permissions required for shipping logs to OSS by modifying `Action` and `Resource` parameters.

        The contents of the `Resource` must be replaced with the ARN of AliyunLogDefaultRole. The example of authorization policy is as follows:

        ```
        {
        "Statement": [
        {
        "Action": "ram:PassRole",
        "Effect": "Allow",
        "Resource": "acs:ram::1111111:role/aliyunlogdefaultrole"
        }
        ],
        "Version": "1"
        }
        ```

    3.  The sub-account a\_1 creates a shipping task. When configuring the task, RAM role column must be filled with the **RAM role**identifier ARN of the OSS ARN of the OSS bucket owner, that is, the RAM role AliyunLogDefaultRole created by account B.

