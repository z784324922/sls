# RAM授权 {#concept_d2w_k4q_zdb .concept}

配置OSS投递任务之前，OSS Bucket的拥有者需要配置[快捷授权](https://ram.console.aliyun.com/?spm=a2c4g.11186623.2.4.OhqCND#/role/authorize?request=%7B%22Requests%22%3A%20%7B%22request1%22%3A%20%7B%22RoleName%22%3A%20%22AliyunLogDefaultRole%22%2C%20%22TemplateId%22%3A%20%22DefaultRole%22%7D%7D%2C%20%22ReturnUrl%22%3A%20%22https%3A//sls.console.aliyun.com/%22%2C%20%22Service%22%3A%20%22Log%22%7D)，授权完成后，当前账号的日志服务有权限对OSS Bucket进行写入操作。

本文为您介绍配置OSS投递任务中多种场景下的RAM授权操作。

-   如您需要对OSS Bucket进行更细粒度的访问控制，请参考[修改授权策略](#)。
-   如果日志服务Project和OSS Bucket不是同一阿里云账号创建的，请参考[跨阿里云账号投递](#)。
-   如果子账号需要将当前主账号的日志数据投递至同一账号下的OSS Bucket，请参考[子账号直接投递](#)。
-   如果子账号需要将日志数据投递至其他阿里云账号下的OSS Bucket，请参考[子账号的跨账号投递](#)。

## 修改授权策略 {#section_bfw_5jf_vdb .section}

通过[快捷授权](https://ram.console.aliyun.com/?spm=a2c4g.11186623.2.9.OhqCND#/role/authorize?request=%7B%22Requests%22%3A%20%7B%22request1%22%3A%20%7B%22RoleName%22%3A%20%22AliyunLogDefaultRole%22%2C%20%22TemplateId%22%3A%20%22DefaultRole%22%7D%7D%2C%20%22ReturnUrl%22%3A%20%22https%3A//sls.console.aliyun.com/%22%2C%20%22Service%22%3A%20%22Log%22%7D)，角色 AliyunLogDefaultRole 默认被授予 AliyunLogRolePolicy，日志服务具有所有 OSS Bucket 的写入权限。

如您需要更精细的访问控制粒度，请解除角色 AliyunLogDefaultRole 的 AliyunLogRolePolicy 授权，并参考*OSS用户指南*创建更细粒度的权限策略，授权给角色 AliyunLogDefaultRole。

## 跨阿里云账号投递 {#section_nw3_1kf_vdb .section}

如您的日志服务Project和OSS Bucket不是同一阿里云账号创建的，您需要按照以下步骤配置授权策略。

例如，需要将A账号下的日志服务数据投递至B账号创建的OSS Bucket中。

1.  主账号B通过[快捷授权](https://ram.console.aliyun.com/?spm=a2c4g.11186623.2.9.OhqCND#/role/authorize?request=%7B%22Requests%22%3A%20%7B%22request1%22%3A%20%7B%22RoleName%22%3A%20%22AliyunLogDefaultRole%22%2C%20%22TemplateId%22%3A%20%22DefaultRole%22%7D%7D%2C%20%22ReturnUrl%22%3A%20%22https%3A//sls.console.aliyun.com/%22%2C%20%22Service%22%3A%20%22Log%22%7D)创建角色AliyunLogDefaultRole，并授予其OSS的写入权限。
2.  在访问控制RAM控制台中单击左侧导航栏角色管理，找到AliyunLogDefaultRole，并单击角色名称以查看该角色的基本信息。

    其中，该角色描述中`Service`配置项定义了角色的合法使用者，例如`log.aliyuncs.com`表示当前账号可扮演此角色以获取OSS的写入权限。

3.  在`Service`配置项中添加角色描述`A_ALIYUN_ID@log.aliyuncs.com`。账号A的账号ID请在**账号管理** \> **安全设置**中查看。

    例如A 的主账号 ID 为1654218965343050，更改后的账号描述为：

    ```
    {
    "Statement": [
     {
       "Action": "sts:AssumeRole",
       "Effect": "Allow",
       "Principal": {
         "Service": [
           "1654218965343050@log.aliyuncs.com",
           "log.aliyuncs.com"
         ]
       }
     }
    ],
    "Version": "1"
    }
    ```

    该角色描述表示账号A有权限通过日志服务获取临时 Token 来操作 B 的资源，关于角色描述的更多信息请参考[授权策略管理](../../../../../intl.zh-CN//授权管理/授权策略管理.md)。

4.  A账号创建投递任务，并在配置投递任务时，**RAM角色**一栏填写OSS Bucket拥有者的RAM角色标识ARN，即账号B通过快速授权创建的RAM角色AliyunLogDefaultRole。

    RAM角色的ARN可以在该角色的基本信息中查看，格式为`acs:ram::13234:role/logrole`。


## 子账号直接投递 {#section_t4x_rqc_wgb .section}

如果希望授予名下子账号创建OSS投递任务等权限，那么需要由主账号登录RAM控制台，为子账号授权。授权范围除OSS投递相关权限之外，还需要包括 PassRole 权限。所以，您需要按照以下步骤创建自定义授权策略并绑定到子账号上。

1.  主账号在RAM控制台创建自定义授权策略。

    示例授权策略如下：

    **说明：** 授权策略中需要包含PassRole权限。

    ```
    {
      "Version": "1",
      "Statement": [
        {
          "Effect": "Allow",
          "Action": "log:*",
          "Resource": "*"
        },
        {
          "Effect": "Allow",
          "Action": "ram:PassRole",
          "Resource": "*"
        }
      ]
    }
    ```

2.  将此自定义授权策略赋予子账号。
    1.  在**用户管理**页面，单击指定子账号右侧的**授权**。
    2.  在**可选授权策略名称**中查找以上自定义授权策略，并将其加入**已选授权策略名称**，并单击**确定**。

## 子账号的跨账号投递 {#section_olj_lkf_vdb .section}

如果主账号A的子账号A1创建投递任务，将A账号的日志数据投递至B账号的OSS Bucket中，需要由主账号A为子账号A1授予PassRole权限。

配置步骤如下：

1.  参考[跨阿里云账号投递](#section_nw3_1kf_vdb)，主账号B配置快捷授权，并添加角色描述。
2.  主账号A登录访问控制RAM控制台，为子账号A1授予AliyunRAMFullAccess权限。
    1.  主账号A在用户管理页面，单击子账号A1右侧的**授权**。
    2.  在**可选授权策略名称**中查找**AliyunRAMFullAccess**，将其加入**已选授权策略名称**，并单击**确定**。

        授权成功后，子账号A1将具备RAM所有权限。

        如果您需要控制A1的权限范围，只授予投递 OSS 的必要权限，可以修改授权策略的`Action`和`Resource`参数。

        示例授权策略如下，您需要将`Resource`的内容替换为AliyunLogDefaultRole的角色ARN。

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

    3.  子账号A1创建投递任务，并在配置投递任务时，**RAM角色**一栏填写OSS Bucket拥有者的RAM角色标识ARN，即账号B通过快速授权创建的RAM角色AliyunLogDefaultRole。

