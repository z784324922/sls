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

-   为子账号授予日志服务的全部操作权限：授予全部管理权限`AliyunLogFullAccess`。详细步骤请参考[RAM 用户](cn.zh-CN/用户指南/访问控制 RAM/RAM 用户.md)。
-   主账号开启DRDS SQL审计分析后，为子账号授予日志查看的权限：授予只读权限`AliyunLogReadOnlyAccess`。详细步骤请参考[RAM 用户](cn.zh-CN/用户指南/访问控制 RAM/RAM 用户.md)。
-   仅为子账号授予开通及及使用DRDS SQL审计与分析的权限，不授予其他日志服务管理权限：创建自定义授权策略，并为子账号授予该自定义授权策略。详细步骤请参考本文档。

1.  登录 [RAM 控制台](https://ram.console.aliyun.com/)。 
2.  在策略管理中打开自定义授权策略页签。 
3.  在页面右上角单击**新建授权策略**。 
4.  单击**空白模板**，并输入**策略名称**和**策略内容**。 请替换参数后，输入以下策略内容。您也可以添加其他自定义授权内容。

    **说明：** 请将`${Project}`与`${Logstore}`替换为您的日志服务Project和Logstore名称。

    ```
    
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

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/40744/154099285421230_zh-CN.png)

5.  单击**新建授权策略**。 
6.  在左侧导航栏中单击用户管理。 
7.  找到子账号并单击对应的**授权**。 
8.  添加上文中创建的自定义授权策略，并单击**确定**。 

