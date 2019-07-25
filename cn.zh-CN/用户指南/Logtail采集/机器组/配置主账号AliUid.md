# 配置主账号AliUid {#task_arl_ynt_qy .task}

其他账号购买的ECS、其他云厂商的服务器和自建IDC在安装Logtail之后，需要配置主账号AliUid作为用户标识，才能加入机器组开始采集日志。

如果您需要通过Logtail采集日志的服务器并非阿里云ECS，或非本账号购买的ECS时，需要在服务器上安装Logtail之后，配置主账号AliUid作为用户标识，证明这台服务器有权限被该账号访问、采集日志。否则在机器组中会显示服务器心跳失败，无法采集数据到日志服务。

-   需要采集日志的服务器为其他账号购买的ECS、其他云厂商的服务器和自建IDC。

-   已在服务器上安装Logtail客户端。

    详细步骤请参见[安装Logtail（Linux系统）](cn.zh-CN/用户指南/Logtail采集/安装/安装Logtail（Linux系统）.md) 和[安装Logtail（Windows系统）](cn.zh-CN/用户指南/Logtail采集/安装/安装Logtail（Windows系统）.md)。


1.  查看阿里云主账号ID，即主账号AliUid。 

    1.  登录[日志服务控制台](https://sls.console.aliyun.com)。
    2.  单击页面右上角的![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13081/156402002541507_zh-CN.png)图标，进入CloudShell（详细内容参考[云命令行](https://www.aliyun.com/product/cloudshell)）。
    3.  在页面下方的命令框中输入echo $ALIBABA\_CLOUD\_ACCOUNT\_ID获取主账号AliUid。
    ![获取主账号ID](images/5286_zh-CN.png "查看主账号AliUid")

2.  登录服务器，配置主账号AliUid作为用户标识。 
    -   **Linux系统** 

        创建主账号AliUid同名文件到 /etc/ilogtail/users 目录，如目录不存在请手动创建目录。一台服务器上可以配置多个主账号AliUid，例如：

        ``` {#codeblock_r8h_2so_fpw}
        touch /etc/ilogtail/users/1**************
        touch /etc/ilogtail/users/1**************
        ```

        当不需要 Logtail 收集数据到该用户的日志服务 Project 后，可删除主账号AliUid用户标识：

        ``` {#codeblock_xz5_zgu_q0s}
        rm /etc/ilogtail/users/1**************
        ```

    -   **Windows系统** 

        创建主账号AliUid同名文件到目录 C:\\LogtailData\\users以配置主账号AliUid作为用户标识。如需删除主账号AliUid用户标识，请直接删除此文件。

        例如，C:\\LogtailData\\users\\1\*\*\*\*\*\*\*\*\*\*\*\*\*\*。

        **说明：** 

        -   服务器配置主账号AliUid作为用户标识后，表示该账号有权限通过 Logtail 收集该服务器日志数据。请及时清理服务器上多余的主账号AliUid用户标识文件。
        -   新增、删除主账号AliUid用户标识后，1分钟之内即可生效。

[创建IP地址机器组](cn.zh-CN/用户指南/Logtail采集/机器组/创建IP地址机器组.md)或[创建自定义标识机器组](cn.zh-CN/用户指南/Logtail采集/机器组/创建用户自定义标识机器组.md)

