# RAM 用户 {#task_xsk_ttc_ry .task}

在实际的应用场景中，主账号可能需要将日志服务的运营维护工作交予其名下的RAM用户，由RAM用户对日志服务进行日常维护工作；或者主账号名下的RAM用户可能有访问日志服务资源的需求。此时，主账号需要对其名下的RAM用户进行授权，授予其访问或者操作日志服务的权限。出于安全性的考虑，日志服务建议您将RAM用户的权限设置为需求范围内的最小权限。

主账号授权RAM用户访问日志服务资源，需要按照以下步骤完成。关于RAM用户的详细信息，请参考[概述](../../../../intl.zh-CN/快速入门/概述.md)。

1.  创建子用户。 
    1.  登录访问控制服务控制台。 
    2.  在**用户管理**页面单击页面右上角的**新建用户**。 
    3.  填写用户信息，勾选 **为该用户自动生成AccessKey** 并单击 **确定**。 
2.  授权子用户访问日志服务资源。 日志服务提供两种系统授权策略，即`AliyunLogFullAccess`和`AliyunLogReadOnlyAccess`，分别表示管理权限和只读权限。您还可以在RAM控制台自定义授权策略，创建方法参考[创建自定义授权策略](../../../../intl.zh-CN/用户指南/授权管理/授权策略管理.md)，权限策略示例请参考[日志服务RAM授权策略](../../../../intl.zh-CN/API 参考/RAM子用户访问/概览.md)。本文档以给用户授予只读权限为例。
    1.  在 **用户管理** 页面找到对应的子用户，单击 **授权**。 
    2.  在弹出的对话框中，选择 **AliyunLogReadOnlyAccess**。 
3.  RAM登录控制台。 完成创建用户和用户授权之后，用户就有权限访问日志服务控制台了。您可以通过以下两种方式以RAM用户身份登录控制台。
    1.  在访问控制服务控制台概览页面，单击**RAM用户登录链接**，使用步骤1中创建的RAM用户用户名和密码登录。 

         ![](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/pic/47664/cn_zh/1483463236703/%E5%AD%90%E7%94%A8%E6%88%B71.png "RAM用户登录") 

    2.  直接访问[RAM用户通用登录页面](https://signin.aliyun.com/?spm=a2c4g.11186623.2.8.syJBhv)，使用步骤1中创建的RAM用户用户名和密码登录。 其中的 **\[企业号别名\]** 默认为主账号的账号id（ali uid），可以在RAM控制台的**设置** \> **账户别名设置**中查看并设置您的云帐号别名。

**自定义授权**

**示例1**

例如，主账号需要赋予RAM用户以下权限：

1.  RAM用户可以看到主账号有哪些日志服务Project。
2.  RAM用户对主账号的某个日志服务Project有只读的访问权限。

同时满足1、2的授权策略如下：

```
{
                "Version": "1",
                "Statement": [
                {
                "Action": ["log:ListProject"],
                "Resource": ["acs:log:*:*:project/*"],
                "Effect": "Allow"
                }，
                {
                "Action": [
                "log:Get*",
                "log:List*"
                ],
                "Resource": "acs:log:*:*:project/<指定的Proejct名称>/*",
                "Effect": "Allow"
                }
                ]
                }
```

**示例2**

1.  RAM用户可以看到主账号有哪些日志服务Project。
2.  RAM用户对主账号的某个日志服务Project下某些Logstore，快速查询和仪表盘有只读的访问权限。

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

