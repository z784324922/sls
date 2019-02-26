# 通过 STS 实现跨账号访问日志服务资源 {#reference_ijc_p5q_12b .reference}

阿里云账号可以通过创建并授权用户角色的方式赋予其他云账号一定的资源权限，其他云账号扮演该角色，并为其名下的RAM用户授予AssumeRole权限之后，可通过主账号或子账号访问STS接口获取临时AK/Token、调用日志服务接口。

阿里云账号创建的Project、Logstore、、Logtail采集配置和机器组等资源为当前云账号所有，创建资源的云账号对该资源具有全部管理权限，不仅可以操作资源，还可以授予其名下的RAM用户（子账号）或其他云账号指定资源的操作、查看或管理权限。本文档介绍通过STS 实现跨账号访问日志服务资源的操作步骤。

## 业务场景 {#section_m52_mr3_wgb .section}

出于业务隔离或项目外包等需要，云账号A希望将部分日志服务业务授权给另一个阿里云账号B，由云账号B操作维护这部分业务。基本需求如下：

-   云账号B拥有向企业A的日志服务中写入数据和使用消费组的权限。
-   云账号B的指定RAM用户也拥有日志服务的写入和消费组权限。
-   云账号B可获取[STS](../../../../../intl.zh-CN/API 参考（STS）/简介.md)临时凭证，访问日志服务API接口。

## 操作步骤 {#section_ktw_ks3_wgb .section}

1.  云账号A创建RAM角色，并指定云账号B扮演该角色。
2.  云账号A为该RAM角色赋予日志服务的指定权限。
3.  云账号B创建RAM用户B1，并为其赋予`AliyunSTSAssumeRoleAccess`（调用 STS AssumeRole 接口）的系统策略。
4.  RAM账号B1调用STS AssumeRole接口访问日志服务API接口，操作日志服务资源。

## 步骤1 云账号A为云账号B创建RAM角色 {#section_plv_xs3_wgb .section}

云账号A创建RAM角色，并指定云账号B扮演该角色。

可以通过[RAM控制台](https://ram.console.aliyun.com/#/role/list)或 RAM 的API [CreateRole](https://api.aliyun.com/?product=Ram&api=CreateRole#/?product=Ram&api=CreateRole)创建RAM角色，此处以控制台创建为例。

1.  云账号A登录[RAM控制台](https://ram.console.aliyun.com/#/role/list)。
2.  在左侧导航栏中单击**RAM角色管理**。
3.  单击**新建RAM角色**。
4.  选择**可信实体类型**为**阿里云账号**。
5.  指定**受信云账号**为**其他云账号**，并输入云账号B的账号ID。
6.  填写**RAM角色名称**。
7.  单击**确定**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13263/155116412039397_zh-CN.png)


以上步骤中创建的RAM角色详情如下。

```
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

## 步骤2 云账号A为RAM角色授权 {#section_ksr_ct3_wgb .section}

云账号A为该RAM角色赋予日志服务的指定权限。

1.  云账号A创建自定义授权策略。

    1.  在左侧导航栏中单击**权限管理** \> **权限策略管理**。
    2.  单击**新建权限策略**。
    3.  输入**策略名称**，并选择**脚本配置**。
    4.  输入权限策略，并单击**确定**。

        此处配置的权限策略为云账号A授予云账号B的权限内容。

        如果仅需要写数据，具体权限描述如下：

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

        如果需要通过协同消费库拖取数据，具体权限描述如下：

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

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13263/155116412039398_zh-CN.png)

    资源（Resource）设置说明：

    -   以上两类资源都是授权指定用户的所有 project 和 logstore
    -   如果授权指定 project：acs:log:*:\{projectOwnerAliUid\}:project/*
    -   如果授权指定 logstore：acs:log:*:\{projectOwnerAliUid\}:project/\{projectName\}/logstore/\{logstoreName\}/*
    完整的资源说明请参考 [日志服务 RAM 资源](intl.zh-CN/API 参考/RAM__STS/资源列表.md)。

2.  将这个权限策略绑定到刚创建的RAM角色上。
    1.  [RAM控制台](https://ram.console.aliyun.com/#/role/list)的RAM角色管理页面找到步骤1中创建的RAM角色。
    2.  在**操作**栏单击**添加权限**。
    3.  选择指定自定义授权策略，并单击**确定**。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13263/155116412039399_zh-CN.png)


## 步骤3 云账号B创建RAM用户B1并授权 {#section_k2h_2t3_wgb .section}

云账号B创建RAM用户B1，并为其赋予`AliyunSTSAssumeRoleAccess`（调用 STS AssumeRole 接口）的系统策略。

1.  云账号B登录[RAM控制台](https://ram.console.aliyun.com/#/role/list)。
2.  在左侧导航栏中单击**人员管理** \> **用户**。
3.  单击**新建用户**。
4.  设置RAM用户基本信息，勾选**控制台密码登录**和**编程访问**，并单击**确定**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13263/155116412039400_zh-CN.png)

5.  通过手机验证码验证权限。
6.  在**人员管理** \> **用户**找到该用户，并单击**添加权限**。
7.  授权系统权限`AliyunSTSAssumeRoleAccess`，并单击**确定**。

    `AliyunSTSAssumeRoleAccess`表示允许该用户调用 STS AssumeRole 接口。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13263/155116412039401_zh-CN.png)


## 步骤4 RAM账号B1获取STS临时凭证，访问日志服务API接口 {#section_vvk_gt3_wgb .section}

1.  **调用 STS [AssumeRole](../../../../../intl.zh-CN/API 参考（STS）/操作接口/AssumeRole.md)接口获取临时AK/Token。**

    您可以选择以下方式调用该接口：

    -   [OpenAPI](https://api.aliyun.com/#/?product=Sts&api=AssumeRole)在线调用。
    -   通过[STS SDK](../../../../../intl.zh-CN/SDK参考/SDK参考（RAM）/Java SDK.md)调用。
2.  **调用日志服务接口。**

    [日志服务 SDK 使用说明](../../../../../intl.zh-CN/SDK 参考/基本介绍/概述.md)。


## 示例代码 {#section_iq3_nk5_12b .section}

样例代码基于Java SDK，以云账号B通过 STS 向用户 A 的 Project 写入数据为例。

[单击下载示例代码](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/47277/cn_zh/1479281238498/StsSample.java)

