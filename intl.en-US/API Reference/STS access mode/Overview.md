# Overview {#reference_ijc_p5q_12b .reference}

## Access Log Service resources of another account by using STS {#section_fzx_hk5_12b .section}

The projects, Logstores, configurations, and machine groups you create are your own resources.  By default, you have the full operation permissions on your resources, and can use all APIs described in this document to perform operations on your resources.

To grant another account the permissions to access your resources, you must use Security Token Service \(STS\) to obtain the temporary AccessKey/token to call specific operations. Before reading the following instructions, see the [Introduction](../../../../intl.en-US/.md).

Assume that User A creates a project, LogStore, and other resources in Log Service, and User B wants to call an API to access these resources, the operation steps are as follows.

## User A {#section_qhh_jk5_12b .section}

**Create a role**

User A creates a role for the trusted account B in the Resource Access Management \(RAM\) console or by using API. The role details are as follows:

```
{
  "Statement": [
    {
      "Action": "sts:AssumeRole",
      "Effect": "Allow",
      "Principal": {
        "RAM": [
          "acs:ram::<Alibaba Cloud account ID of user B>:root"
        ]
      }
    }
  ],
  "Version": "1"
}
```

**Grant permissions to the created role**

After a role is created, user A must grant specific operation permissions to the role.

The permissions required for data writing only are described as follows:

```
```
{
  "Version": "1",
  "Statement": [
    {
      "Action": "log:PostLogStoreLogs",
      "Resource": "*",
      "Effect": "Allow"
    }
  ]
}
```
```

Permissions required for data extraction by using the collaborative consumer group are as follows:

```
```
{
  "Version": "1",
  "Statement": [
    {
      "Action": [
         "log:GetCursorOrData",
         "log:CreateConsumerGroup",
         "log:ListConsumerGroup",
         "log:ConsumerGroupUpdateCheckPoint",
         "log:ConsumerGroupHeartBeat",
         "log:GetConsumerGroupCheckPoint",
         "log:UpdateConsumerGroup"
      ]
      "Resource": "*",
      "Effect": "Allow"
    }
  ]
}
```
```

How to set a resource is described below:

-   Configure the resource as follows:
-   To authorize the role to access a specific project: acs:log:*:\{projectOwnerAliUid\}:project/*
-   To authorize  the role to access a specific Logstore: acs:log:*:\{projectOwnerAliUid\}:project/\{projectName\}/logstore/\{logstoreName\}/*

For complete resource description, see  [Log Service RAM  resources](intl.en-US/API Reference/RAM subaccount access/Resource list.md).

## User B {#section_pxl_mk5_12b .section}

**Create and authorize a RAM user**

Create a RAM user and grant the AssumeRole permission  to the created RAM user in the RAM console or by using APIs/SDKs.

**Call STS interface to obtain the temporary AccessKey/token**

[For more information, see STS SDK usage Limits](../../../../intl.en-US/SDK Reference/SDK Reference - RAM/Java SDK.md)

**Call a Log Service interface**

[Log Service SDK Instructions](../../../../intl.en-US/SDK Reference/Basic Descriptions /Overview .md)

## Code example {#section_iq3_nk5_12b .section}

The sample code is applicable to the case where User B writes data to a project of User A through STS \(based on JavaSDK\).

[Code link](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/47277/cn_zh/1479281238498/StsSample.java)

