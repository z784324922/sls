# 为子账号授予DRDS SQL审计权限 {#task_omb_rgl_qfb .task}

子账号开通或使用DRDS SQL审计功能之前，需要由主账号为其授权。

使用日志服务实时日志分析，需要如下权限 ：

|操作类型|支持的操作账号类型|
|:---|:--------|
|开通日志服务（全局一次性操作）|主账号|
|授权DRDS写入数据到您的日志服务 （全局一次性操作）| -   主账号
-   具备`AliyunLogFullAccess`权限的子账号
-   具备指定权限的子账号

 |
|使用日志分析功能| -   主账号
-   具备`AliyunLogFullAccess`权限的子账号
-   具备指定权限的子账号

 |

您也可以根据需求为子账号授权。

-   为子账号授予日志服务的全部操作权限：授予全部管理权限`AliyunLogFullAccess`。详细步骤请参考[授权RAM用户](../cn.zh-CN/访问控制RAM/授权RAM 用户.md#)。
-   主账号开启DRDS SQL审计分析后，为子账号授予日志查看的权限：授予只读权限`AliyunLogReadOnlyAccess`。详细步骤请参考[授权RAM用户](../cn.zh-CN/访问控制RAM/授权RAM 用户.md#)。
-   仅为子账号授予开通及及使用DRDS SQL审计与分析的权限，不授予其他日志服务管理权限：创建自定义授权策略，并为子账号授予该自定义授权策略。详细步骤请参考本文档。

1.  云账号登录[RAM控制台](https://ram.console.aliyun.com/)。
2.  在左侧导航栏的**权限管理**菜单下，单击**权限策略管理**。
3.  单击**新建权限策略**。
4.  填写**策略名称**和**备注**，**配置模式**选择**脚本配置**后输入**策略内容**。 请替换参数后，输入以下策略内容。您也可以添加其他自定义授权内容。

    **说明：** 请将`${Project}`与`${Logstore}`替换为您的日志服务Project和Logstore名称。

    ``` {#codeblock_e4i_3o7_uqq}
    {
      "Version": "1",
      "Statement": [
          {
          "Action": "log:GetProject",
          "Resource": "acs:log:*:*:project/${Project}",
          "Effect": "Allow"
        },
        {
          "Action": "log:CreateProject",
          "Resource": "acs:log:*:*:project/*",
          "Effect": "Allow"
        },
        {
          "Action": "log:ListLogStores",
          "Resource": "acs:log:*:*:project/${Project}/logstore/*",
          "Effect": "Allow"
        },
        {
          "Action": "log:CreateLogStore",
          "Resource": "acs:log:*:*:project/${Project}/logstore/*",
          "Effect": "Allow"
        },
        {
          "Action": "log:GetIndex",
          "Resource": "acs:log:*:*:project/${Project}/logstore/${Logstore}",
          "Effect": "Allow"
        },
        {
          "Action": "log:CreateIndex",
          "Resource": "acs:log:*:*:project/${Project}/logstore/${Logstore}",
          "Effect": "Allow"
        },
        {
          "Action": "log:UpdateIndex",
          "Resource": "acs:log:*:*:project/${Project}/logstore/${Logstore}",
          "Effect": "Allow"
        },
        {
          "Action": "log:CreateDashboard",
          "Resource": "acs:log:*:*:project/${Project}/dashboard/*",
          "Effect": "Allow"
        },
        {
          "Action": "log:UpdateDashboard",
          "Resource": "acs:log:*:*:project/${Project}/dashboard/*",
          "Effect": "Allow"
        },
        {
          "Action": "log:CreateSavedSearch",
          "Resource": "acs:log:*:*:project/${Project}/savedsearch/*",
          "Effect": "Allow"
        },
        {
          "Action": "log:UpdateSavedSearch",
          "Resource": "acs:log:*:*:project/${Project}/savedsearch/*",
          "Effect": "Allow"
        }
      ]
    }
    ```

    ![新建策略内容](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/40744/156897880860819_zh-CN.png)

5.  在左侧导航栏的**权限管理**菜单下，单击**授权**。
6.  单击**新增授权**。
7.  在**被授权主体**区域下，输入RAM用户名称后，单击需要授权的RAM用户。
8.  添加上面创建的自定义授权策略，并单击**确定**。

