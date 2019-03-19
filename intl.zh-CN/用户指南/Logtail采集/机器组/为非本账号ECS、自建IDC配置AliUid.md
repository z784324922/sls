# 为非本账号ECS、自建IDC配置AliUid {#task_arl_ynt_qy .task}

其他账号购买的ECS、其他云厂商的服务器和自建IDC在安装Logtail之后，需要配置AliUid作为用户标识，才能加入机器组开始采集日志。

如果您需要通过Logtail采集日志的服务器并非阿里云ECS，或非本账号购买的ECS时，需要在服务器上安装Logtail之后，配置AliUid作为用户标识，证明这台服务器有权限被该账号访问、采集日志。否则在机器组中会显示服务器心跳失败，无法采集数据到日志服务。

-   需要采集日志的服务器为其他账号购买的ECS、其他云厂商的服务器和自建IDC。

-   已在服务器上安装Logtail客户端。

    详细步骤请参见[安装Logtail（Linux系统）](intl.zh-CN/用户指南/Logtail采集/安装/安装Logtail（Linux系统）.md) 和[安装Logtail（Windows系统）](intl.zh-CN/用户指南/Logtail采集/安装/安装Logtail（Windows系统）.md)。


1.  查看阿里云账号ID，即AliUid。 

    登陆账号管理页面，查看日志服务Project所属账号的AliUid。

    ![](images/5286_zh-CN.png "查看AliUid")

2.  登录服务器，配置AliUid作为用户标识。 
    -   **Linux系统**

        创建AliUid同名文件到 /etc/ilogtail/users 目录，如目录不存在请手动创建目录。一台服务器上可以配置多个AliUid，例如：

        ```
        touch /etc/ilogtail/users/1**************
        touch /etc/ilogtail/users/1**************
        ```

        当不需要 Logtail 收集数据到该用户的日志服务 Project 后，可删除AliUid用户标识：

        ```
        rm /etc/ilogtail/users/1**************
        ```

    -   **Windows系统**

        创建AliUid同名文件到目录 C:\\LogtailData\\users以配置AliUid作为用户标识。如需删除AliUid用户标识，请直接删除此文件。

        例如，C:\\LogtailData\\users\\1\*\*\*\*\*\*\*\*\*\*\*\*\*\*。

        **说明：** 

        -   服务器置AliUid作为用户标识后，表示该账号有权限通过 Logtail 收集该服务器日志数据。请及时清理服务器上多余的账号AliUid用户标识文件。
        -   新增、删除AliUid用户标识后，1分钟之内即可生效。

[创建IP地址机器组](intl.zh-CN/用户指南/Logtail采集/机器组/创建IP地址机器组.md)或[创建自定义标识机器组](intl.zh-CN/用户指南/Logtail采集/机器组/创建用户自定义标识机器组.md)

