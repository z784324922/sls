# RAM自定义授权场景 {#reference_s3z_m1l_z2b .reference}

通过RAM访问控制可以为名下的RAM用户（子用户）授权。

主账号可以对其名下的RAM用户（子用户）进行授权，授予其访问或者操作日志服务的权限。您可以为RAM用户授予[系统授权策略](intl.zh-CN/用户指南/访问控制 RAM/授权RAM 用户.md)和[自定义授权策略](../../../../intl.zh-CN/用户指南/授权管理/授权策略管理.md)。

## 注意事项 {#section_gst_t4l_bfb .section}

-   出于安全性的考虑，日志服务建议您将RAM用户的权限设置为需求范围内的最小权限。
-   通常情况下，您需要为RAM用户授予Project列表的只读权限，否则RAM用户无法进入Project列表查看资源。
-   动作`log:ListProject`提供Project列表的查看权限：
    -   具备此权限时，支持只读查看所有Project，暂不支持仅查看某几个Project。
    -   不具备此权限时，无法查看任何Project。

本文档为您演示常见的自定义授权场景和授权内容，包括：

-   [控制台场景：Project列表和指定Project的只读权限](#)
-   [控制台场景：指定Logstore的只读权限和快速查询的创建、使用权限](#)
-   [控制台场景：指定Project中所有快速查询、仪表盘和指定Logstore的只读权限](#)
-   [API场景：指定Project的写入权限](#)
-   [API场景：指定Project的消费权限](#)
-   [API场景：指定Logstore的消费权限](#)

更多信息：

-   [可授权的资源](../../../../intl.zh-CN/API 参考/RAM子用户访问/资源列表.md)
-   [可授权的动作](../../../../intl.zh-CN/API 参考/RAM子用户访问/动作列表.md)
-   [鉴权规则](../../../../intl.zh-CN/API 参考/RAM子用户访问/鉴权规则.md)

## 控制台场景：Project列表和指定Project的只读权限 {#section_vrx_w1l_z2b .section}

例如，主账号需要赋予RAM用户以下权限：

1.  RAM用户可以看到主账号的日志服务Project列表。
2.  RAM用户对主账号的指定Project有只读的访问权限。

同时满足1、2的授权策略如下：

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

## 控制台场景：指定Logstore的只读权限和快速查询的创建、使用权限 {#section_akw_rbl_z2b .section}

例如，主账号需要赋予RAM用户以下权限：

1.  RAM用户登录控制台可以看到主账号的日志服务Project列表。
2.  RAM用户对指定Logstore具有只读权限，且可以创建、使用快速查询。

同时满足1、2的授权策略如下：

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
        "log:List*"
      ],
      "Resource": [
        "acs:log:*:*:project/<指定的Project名称>/dashboard",
        "acs:log:*:*:project/<指定的Project名称>/dashboard/*"
      ],
      "Effect": "Allow"
    },
    {
      "Action": [
        "log:Get*",
        "log:List*",
        "log:Create*"
      ],
      "Resource": [
        "acs:log:*:*:project/<指定的Project名称>/savedsearch",
        "acs:log:*:*:project/<指定的Project名称>/savedsearch/*"
      ],
      "Effect": "Allow"
    }
  ]
}
```

## 控制台场景：指定Project中所有快速查询、仪表盘和指定Logstore的只读权限 {#section_ig5_xbl_z2b .section}

例如，主账号需要赋予RAM用户以下权限：

1.  RAM用户可以看到主账号的日志服务Project列表。
2.  RAM用户仅能查看指定Logstore，同时可以查看所有的快速查询和仪表盘列表。

**说明：** 如果希望赋予RAM用户指定Logstore的只读权限，则必须同时赋予该RAM用户所有的快速查询和仪表盘列表的查看权限。

同时满足1、2的授权策略如下：

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
        "acs:log:*:*:project/<指定的Project名称>/dashboard",
        "acs:log:*:*:project/<指定的Project名称>/dashboard/*"
      ],
      "Effect": "Allow"
    },
    {
      "Action": [
        "log:Get*",
        "log:List*"
      ],
      "Resource": [
        "acs:log:*:*:project/<指定的Project名称>/savedsearch",
        "acs:log:*:*:project/<指定的Project名称>/savedsearch/*"
      ],
      "Effect": "Allow"
    }
  ]
}
```

## API场景：指定Project的写入权限 {#section_vtv_ccl_z2b .section}

 RAM用户只能向某一Project写入数据，无法进行查询等其他操作。

```
{
  "Version": "1",
  "Statement": [
    {
      "Action": [
        "log:Post*"
      ],
      "Resource": "acs:log:*:*:project/<指定的project名称>/*",
      "Effect": "Allow"
    }
  ]
}
```

## API场景：指定Project的消费权限 {#section_o1m_fcl_z2b .section}

 RAM用户只能消费某一Project的数据，无法进行数据写入、查询等其他操作。

```
{
  "Version": "1",
  "Statement": [
    {
      "Action": [
        "log:GetCursorOrData",
        "log:GetConsumerGroupCheckPoint",
        "log:UpdateConsumerGroup",
        "log:ConsumerGroupHeartBeat",
        "log:ConsumerGroupUpdateCheckPoint",
        "log:ListConsumerGroup",
        "log:CreateConsumerGroup"
      ],
      "Resource": "acs:log:*:*:project/<指定的project名称>/*",
      "Effect": "Allow"
    }
  ]
}
```

## API场景：指定Logstore的消费权限 {#section_aw3_hcl_z2b .section}

 RAM用户只能消费指定Logstore的数据，无法进行数据写入、查询等其他操作。

```
{
  "Version": "1",
  "Statement": [
    {
      "Action": [
        "log:GetCursorOrData",
        "log:GetConsumerGroupCheckPoint",
        "log:UpdateConsumerGroup",
        "log:ConsumerGroupHeartBeat",
        "log:ConsumerGroupUpdateCheckPoint",
        "log:ListConsumerGroup",
        "log:CreateConsumerGroup"
      ],
      "Resource": [
        "acs:log:*:*:project/<指定的project名称>/logstore/<指定的Logstore名称>",
        "acs:log:*:*:project/<指定的project名称>/logstore/<指定的Logstore名称>/*"
      ],
      "Effect": "Allow"
    }
  ]
}
```

