# 非本人ECS（或线下机器） {#task_arl_ynt_qy .task}

如果您需要通过Logtail采集日志的服务器并非阿里云ECS，或非属于本账号下创建ECS时，则需要在服务器上安装Logtail之后，配置用户标识（账号 ID），证明这台机器有选项被该账号访问联通。否则会显示心跳失败、无法采集数据到日志服务。请按照以下步骤配置。

1.   安装Logtail。 

    在您需要采集日志的服务器上安装Logtail客户端，详细步骤请参见[Linux](intl.zh-CN/用户指南/Logtail 采集/安装/Linux.md) 和[Windows](intl.zh-CN/用户指南/Logtail 采集/安装/Windows.md)。

2.   配置用户标识。 
    1.   查看账号 ID。 

        登陆账号管理页面，查看日志服务 Project 所属账号的 ID。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13081/5286_zh-CN.png "查看账号ID")

    2.   登录机器配置账号 ID 标识文件。 
        -   **Linux系统**

            创建账号ID同名文件到 /etc/ilogtail/users 目录，如目录不存在请手动创建，一台机器上可以配置多个账号ID，例如：

            ```
            touch /etc/ilogtail/users/1559122535028493
            touch /etc/ilogtail/users/1329232535020452
            ```

            当不需要 Logtail 收集数据到该用户的日志服务 Project 后，可删除用户标识：

            ```
            rm /etc/ilogtail/users/1559122535028493
            ```

        -   **Windows系统**

            创建账号ID同名文件到目录 C:\\LogtailData\\users以配置用户标识。如需删除用户标识，请直接删除此文件。

            例如，C:\\LogtailData\\users\\1559122535028493。

            **说明：** 

            -   机器上配置账号 ID 标识后，标识该云账号有权限通过 Logtail 收集该机器上的日志数据。机器上不必要的账号标识文件请及时清理。
            -   新增、删除用户标识后，1分钟之内即可生效。

