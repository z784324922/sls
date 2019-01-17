# Logtail 机器无心跳 {#concept_nfs_hs3_bfb .concept}

配置Logtail采集日志数据后，如果Logtail机器组心跳状态不正常，可使用Logtail自动诊断工具或人工诊断的方式排查问题。

如果使用Logtail采集日志，在服务器上安装Logtail之后，Logtail会定时向服务端发送心跳包。如果机器组状态页面显示机器无心跳，说明客户端和服务端未成功联通。日志服务提供[Logtail自动诊断工具](intl.zh-CN/常见问题/日志采集/Logtail 快速诊断工具.md)和人工诊断步骤，您可以根据需求选择排查方式。

-   **自动诊断**：日志服务提供Linux版Logtail自动诊断工具，排查步骤请参考[Logtail自动诊断工具](intl.zh-CN/常见问题/日志采集/Logtail 快速诊断工具.md)。

-   **人工诊断**：Logtail诊断工具未检查出问题、或服务器为Windows服务器，请参考本文档逐步排查。


![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13294/154770978911589_zh-CN.png)

## 1. 检查是否已安装Logtail {#section_ntl_4tj_bfb .section}

请执行以下命令查看客户端状态，如未安装Logtail客户端，请参考[Linux](../intl.zh-CN/用户指南/Logtail采集/安装/Linux.md)和[Windows](../intl.zh-CN/用户指南/Logtail采集/安装/Windows.md)，务必按照您日志服务Project所属Region以及网络类型进行安装。

查看Logtail安装状态：

-   Linux：

    ```
    sudo /etc/init.d/ilogtaild status 
    ```

    如果显示`ilogtail is running`，表示已安装Logtail，例如：

    ```
    [root@****************~]# sudo /etc/init.d/ilogtaild status 
    ilogtail is running
    ```

-   Windows：
    1.  在控制面板中单击**管理工具**，并单击**服务**。
    2.  查看LogtailDaemon、LogtailWorker两个Windows Service的运行状态。如果正在运行，表示已安装Logtail。

如果Logtail正在运行，请执行下一步检查。

## 2. 检查Logtail安装参数是否正确 {#section_cgq_f5j_bfb .section}

安装Logtail时，需要为客户端指定正确的服务端访问入口，即根据日志服务Project所在地域选择[表 1](../intl.zh-CN/用户指南/Logtail采集/安装/Linux.md#table_eyz_pmv_vdb)，并根据[网络类型](../intl.zh-CN/用户指南/Logtail采集/选择网络.md)选择不同的安装方式。如果安装参数或安装脚本错误，可能会导致Logtail机器无心跳。

Logtail配置文件ilogtail\_config.json中记录了Logtail安装参数及所选的安装方式，该文件的路径为：

-   Linux：/usr/local/ilogtail/ilogtail\_config.json
-   Windows x64：C:\\Program Files \(x86\)\\Alibaba\\Logtail\\ilogtail\_config.json
-   Windows x32：C:\\Program Files\\Alibaba\\Logtail\\ilogtail\_config.json

1.  检查安装参数。

    检查文件ilogtail\_config.json中客户端连接的网络入口所属Region是否与您Project所在Region一致。

    例如以下回显信息表明Logtail安装在华东一（杭州）地域的ECS中。

    ![](images/21881_zh-CN.png "检查安装参数")

2.  检查安装方式。

    Telnet测试ilogtail\_config.json中配置的域名，检查是否根据服务器所属网络环境选择了正确的安装方式。

    例如，ilogtail\_config.json中记录Logtail配置的域名为`cn-hangzhou-intranet`，则可以执行`telnet logtail.cn-hangzhou-intranet.log.aliyuncs.com 80`检查连通性，如果已联通，说明安装方式正确。

    例如，查看Linux ECS的网络连接性：

    ```
    [root@*********** ~]# telnet logtail.cn-hangzhou-intranet.log.aliyuncs.com 80
    Trying 100*0*7*5...
    Connected to logtail.cn-hangzhou-intranet.log.aliyuncs.com.
    Escape character is '^]'. 
    ```

    如果telenet失败，说明安装时选择了错误的参数，以至于执行了错误的安装命令。请参考[Linux](../intl.zh-CN/用户指南/Logtail采集/安装/Linux.md)和[Windows](../intl.zh-CN/用户指南/Logtail采集/安装/Windows.md)选择正确的安装参数。


如果Logtail已正确安装，请执行下一步检查。

## 3. 检查机器组配置的IP地址是否正确 {#section_nxh_bxj_bfb .section}

机器组中配置的IP地址必须和Logtail获取到的服务器地址一致，否则机器组无心跳、或无法采集到日志数据。

Logtail在机器上获取IP的方式：

-   如果没有设置主机名绑定，会取服务器的第一块网卡IP。

-   如果在文件/etc/hosts中设置了主机名绑定，需要确认绑定的IP。执行命令hostname可以查看主机名。


**排查步骤**

1.  查看Logtail获取的IP地址。

    文件app\_info.json的`ip`字段中记录了Logtail获取的IP地址，该文件的路径为：

    -   Linux：/usr/local/ilogtail/app\_info.json
    -   Windows x64：C:\\Program Files \(x86\)\\Alibaba\\Logtail\\app\_info.json
    -   Windows x32：C:\\Program Files\\Alibaba\\Logtail\\app\_info.json
    **说明：** 

    -   如果`app_info.json`文件中`ip`字段为空，Logtail无法工作。此时需为服务器设置IP地址并重启Logtail。
    -   文件`app_info.json`仅做记录，修改该文件并不会改变Logtail获取的IP地址。
    ![](images/11585_zh-CN.png "查看Logtail获取的IP地址")

2.  查看机器组中配置的地址。

    机器组中配置的IP地址，请在日志服务控制台机器组列表页面单击**查看状态**。

     ![](images/11586_zh-CN.png "查看机器组") 

    如果服务端机器组内填写的IP与客户端获取的IP不一致，则需要修改。

    -   若服务端机器组填写了错误IP，请修改机器组内IP地址并保存，等待1分钟再查看心跳状态。

    -   若修改了机器上的网络配置（如修改/etc/hosts），请重新启动Logtail以获取新的IP，并根据`app_info.json`文件中的`ip`字段修改机器组内设置的IP地址。


**重启Logtail的方式**：

-   Linux：

    ```
    sudo /etc/init.d/ilogtaild stop
    sudo /etc/init.d/ilogtaild start
    ```

-   Windows：在控制面板中单击**管理工具** \> **服务**，找到并重启LogtailWorker。

机器组配置的IP地址的确为Logtail获取的IP地址，请执行下一步检查。

## 4. 检查非本账号下ECS是否已配置主账号AliUid {#section_ww2_yzj_bfb .section}

如果您的ECS服务器和日志服务Project不在同一账号下、或服务器为其他云厂商服务器、自建IDC，则需要在服务器上[配置主账号AliUid](../intl.zh-CN/用户指南/Logtail采集/机器组/为非本账号ECS、自建IDC配置AliUid.md)，为安装Logtail的机器授权。

检查/etc/ilogtail/users目录下是否有账号ID同名文件。

如果没有，请参考文档[为非本账号ECS、自建IDC配置AliUid](../intl.zh-CN/用户指南/Logtail采集/机器组/为非本账号ECS、自建IDC配置AliUid.md)进行操作。

**说明：** 

-   必须是主账号的ID。

-   账号ID请在阿里云控制台个人信息中查看。

    ![](../DNSLS11850791/images/5286_zh-CN.png "查看账号ID")


如果您的问题仍未解决，请[提工单](https://selfservice.console.aliyun.com/ticket/category/sls/today)到日志服务。工单中请提供您的Project、Logstore、机器组、app\_info.json、ilogtail\_config.json以及自助诊断工具的输出内容。

