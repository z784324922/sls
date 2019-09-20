# 通过 STS 实现跨账号访问日志服务资源 {#reference_ijc_p5q_12b .task}

阿里云账号可以通过创建并授权用户角色的方式赋予其他云账号一定的资源权限，其他云账号扮演该角色，并为其名下的RAM用户授予AssumeRole权限之后，可通过主账号或子账号访问STS接口获取临时AK/Token、调用日志服务接口。

阿里云账号创建的Project、Logstore、Logtail采集配置和机器组等资源为当前云账号所有，创建资源的云账号对该资源具有全部管理权限，不仅可以操作资源，还可以授予其名下的RAM用户或其他云账号指定资源的操作、查看或管理权限。本文档介绍通过STS实现跨账号访问日志服务资源的操作步骤。

## 业务场景 {#section_2qx_xvv_3xb .section}

出于业务隔离或项目外包等需要，云账号A希望将部分日志服务业务授权给另一个阿里云账号B，由云账号B操作维护这部分业务。基本需求如下：

-   云账号B拥有向企业A的日志服务中写入数据和使用消费组的权限。
-   云账号B的指定RAM用户也拥有日志服务的写入和消费组权限。
-   云账号B可获取[STS](../../cn.zh-CN/API 参考（STS）/什么是STS.md)临时凭证，访问日志服务API接口。

## 操作步骤 {#section_ggf_o1f_xiu .section}

1.  云账号A创建RAM角色，并指定云账号B扮演该角色。
2.  云账号A为该RAM角色赋予日志服务的指定权限。
3.  云账号B创建RAM用户B1，并为其赋予`AliyunSTSAssumeRoleAccess`（调用 STS AssumeRole 接口）的系统策略。
4.  RAM账号B1调用STS AssumeRole接口访问日志服务API接口，操作日志服务资源。

## 步骤1 云账号A为云账号B创建RAM角色 {#section_cgn_d47_i2y .section}

云账号A创建RAM角色，并指定云账号B扮演该角色。

可以通过[RAM控制台](https://ram.console.aliyun.com/#/role/list)或 RAM 的API [CreateRole](https://api.aliyun.com/?product=Ram&api=CreateRole#/?product=Ram&api=CreateRole)创建RAM角色，此处以控制台创建为例。

1.  云账号A登录[RAM控制台](https://ram.console.aliyun.com/)。
2.  在左侧导航栏，单击**RAM角色管理**。
3.  单击**新建RAM角色**，选择可信实体类型为**阿里云账号**，单击**下一步**。
4.  输入**角色名称**和**备注**。
5.  **选择云账号**为**其他云账号**。 

    **说明：** 将鼠标悬停在控制台右上角头像的位置，单击**安全设置**，即可查询其他云账号的ID。

6.  填写云账号B的账号ID，单击**完成**。

以上步骤中创建的RAM角色详情如下。

``` {#codeblock_vhq_35x_dtj}
{
  "Statement": [
    {
      "Action": "sts:AssumeRole",
      "Effect": "Allow",
      "Principal": {
        "RAM": [
          "acs:ram::<云账号 B 的账号 ID>:root"
        ]
      }
    }
  ],
  "Version": "1"
}
```

## 步骤2 云账号A为RAM角色授权 {#section_zc1_ncn_7ft .section}

云账号A为该RAM角色赋予日志服务的指定权限。

1.  在左侧导航栏的**权限管理**菜单下，单击**权限策略管理**。
2.  单击**新建权限策略**。
3.  填写**策略名称**和**备注**。
4.  **配置模式**选择**脚本配置**。
5.  输入权限策略，并单击**确定**。 此处配置的权限策略为云账号A授予云账号B的权限内容。

    如果仅需要写数据，具体权限描述如下：

    ``` {#codeblock_zq6_zb6_vdc}
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

    如果需要通过协同消费库拖取数据，具体权限描述如下：

    ``` {#codeblock_cpi_z2x_g5g}
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

    以上两类资源都是授权指定用户的所有Project和Logstore：

    -   授权指定Project：`acs:log::\{projectOwnerAliUid\}:project/`。
    -   授权指定Logstore：`acs:log::\{projectOwnerAliUid\}:project/\{projectName\}/logstore/\{logstoreName\}/`。
    完整的资源说明请参考 [日志服务 RAM 资源](cn.zh-CN/API 参考/RAM__STS/资源列表.md)。

6.  在左侧导航栏，单击**RAM角色管理**。
7.  在**RAM角色名称**列表下，找到目标RAM角色。
8.  单击**添加权限**，被授权主体会自动填入。
9.  在左侧**权限策略名称**列表下，选择上面创建的需要授予RAM角色的权限策略，单击**确定**。
10. 单击**完成**。

## 步骤3 云账号B创建RAM用户B1并授权 {#section_1nd_hy9_jmz .section}

云账号B创建RAM用户B1，并为其赋予`AliyunSTSAssumeRoleAccess`（调用 STS AssumeRole 接口）的系统策略。

1.  云账号B登录[RAM控制台](https://ram.console.aliyun.com/)。
2.  在左侧导航栏的**人员管理**菜单下，单击**用户**。
3.  单击**新建用户**。 

    **说明：** 单击**添加用户**，可一次性创建多个RAM用户。

4.  设置RAM用户基本信息，勾选**控制台密码登录**和**编程访问**，并单击**确定**。 

    **说明：** 通过手机验证码验证权限。

5.  在左侧导航栏的**人员管理**菜单下，单击**用户**。
6.  在**用户登录名称/显示名称**列表下，找到目标RAM用户。
7.  单击**添加权限**，被授权主体会自动填入。
8.  在左侧**权限策略名称**列表下，选择需要授予RAM用户的权限策略**AliyunSTSAssumeRoleAccess**并单击**确定**。
9.  单击**完成**。

## 步骤4 RAM账号B1获取STS临时凭证访问日志服务 {#section_t8j_0km_np0 .section}

1.  调用 STS [AssumeRole](../../cn.zh-CN/API 参考（STS）/操作接口/AssumeRole.md)接口获取临时AK/Token。 您可以选择以下方式调用该接口：
    -   [OpenAPI](https://api.aliyun.com/#/?product=Sts&api=AssumeRole)在线调用。
    -   通过[STS SDK](../../cn.zh-CN/SDK参考/SDK参考（RAM）/Java SDK.md)调用。
2.  调用日志服务接口。 关于日志服务SDK请参见[概述](../cn.zh-CN/SDK 参考/基本介绍/概述.md#)。

## 示例代码 {#section_6a2_arl_p6w .section}

样例代码基于Java SDK，以云账号B通过STS向用户A的Project写入数据为例。

 [单击下载示例代码](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/47277/cn_zh/1479281238498/StsSample.java)

