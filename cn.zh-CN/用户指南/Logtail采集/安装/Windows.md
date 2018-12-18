# Windows {#concept_j22_xnv_vdb .concept}

Logtail客户端是日志服务提供的日志采集Agent，请参考本文档，在Windows服务器上安装Logtail客户端。

## 支持的系统 {#section_ppj_ynv_vdb .section}

支持Windows Server2003（含）以后 32/64 位系统，例如：

-   Windows 7 \(Client\) 32bit
-   Windows 7 \(Client\) 64bit
-   Windows Server 2003 32bit
-   Windows Server 2003 64bit
-   Windows Server 2008 32bit
-   Windows Server 2008 64bit
-   Windows Server 2012 64bit

## 前提条件 {#section_t52_5zm_1fb .section}

1.  已拥有一台及以上的服务器。
2.  已根据服务器类型和所在Region，确定日志采集流量的网络类型。详细说明请参考[选择网络](intl.zh-CN/用户指南/Logtail采集/选择网络.md)。

    ![](images/12057_zh-CN.png "选择网络")


## 安装Logtail {#section_d1z_znv_vdb .section}

1.  下载安装包。

    下载地址：

    -   中国大陆：单击下载[Logtail安装包](http://logtail-release.oss-cn-hangzhou.aliyuncs.com/win/logtail_installer.zip)。
    -   海外：单击下载[Logtail安装包](http://logtail-release-global.log-global.aliyuncs.com/win/logtail_installer.zip)。
2.  解压缩 `logtail_installer.zip` 到当前目录。
3.  根据服务器类型和所在区域[选择网络类型](intl.zh-CN/用户指南/Logtail采集/选择网络.md)后，按照日志服务所在区域安装Logtail。

    打开Windows Powershell或cmd进入 `logtail_installer` 目录，并根据地域和网络环境执行相应的安装命令。

    安装命令：

    |区域|阿里云内网（经典网络、VPC）|公网|全球加速|
    |:-|:--------------|:-|:---|
    |**华北 1（青岛）**|.\\logtail\_installer.exe install cn-qingdao|.\\logtail\_installer.exe install cn-qingdao-internet|.\\logtail\_installer.exe install cn-qingdao-acceleration|
    |**华北 2（北京）**|.\\logtail\_installer.exe install cn-beijing|.\\logtail\_installer.exe install cn-beijing-internet|.\\logtail\_installer.exe install cn-beijing-acceleration|
    |**华北 3 （张家口）**|.\\logtail\_installer.exe install cn-zhangjiakou|.\\logtail\_installer.exe install cn-zhangjiakou-internet|.\\logtail\_installer.exe install cn-zhangjiakou-acceleration|
    |**华北 5 （呼和浩特）**|.\\logtail\_installer.exe install cn-huhehaote|.\\logtail\_installer.exe install cn-huhehaoteinternet|.\\logtail\_installer.exe install cn-huhehaote-acceleration|
    |**华东 1（杭州）**|.\\logtail\_installer.exe install cn-hangzhou|.\\logtail\_installer.exe install cn-hangzhou-internet|.\\logtail\_installer.exe install cn-hangzhou-acceleration|
    |**华东 2（上海）**|.\\logtail\_installer.exe install cn-shanghai|.\\logtail\_installer.exe install cn-shanghai-internet|.\\logtail\_installer.exe install cn-shanghai-acceleration|
    |**华南 1（深圳）**|.\\logtail\_installer.exe install cn-shenzhen|.\\logtail\_installer.exe install cn-shenzhen-internet|.\\logtail\_installer.exe install cn-shenzhen-acceleration|
    |**西南 1（成都）**|.\\logtail\_installer.exe install cn-chengdu|.\\logtail\_installer.exe install cn-chengdu-internet|.\\logtail\_installer.exe install cn-chengdu-acceleration|
    |**香港**|.\\logtail\_installer.exe install cn-hongkong|.\\logtail\_installer.exe install cn-hongkong-internet|.\\logtail\_installer.exe install cn-hongkong-acceleration|
    |**美国西部 1 （硅谷）**|.\\logtail\_installer.exe install us-west-1|.\\logtail\_installer.exe install us-west-1-internet|.\\logtail\_installer.exe install us-west-1-acceleration|
    |**美国东部 1（弗吉尼亚）**|.\\logtail\_installer.exe install us-east-1|.\\logtail\_installer.exe install us-east-1-internet|.\\logtail\_installer.exe install us-east-1-acceleration|
    |**亚太东南 1（新加坡）**|.\\logtail\_installer.exe install ap-southeast-1|.\\logtail\_installer.exe install ap-southeast-1-internet|.\\logtail\_installer.exe install ap-southeast-1-acceleration|
    |**亚太东南 2（悉尼）**|.\\logtail\_installer.exe install ap-southeast-2|.\\logtail\_installer.exe install ap-southeast-2-internet|.\\logtail\_installer.exe install ap-southeast-2-acceleration|
    |**亚太东南 3（吉隆坡）**|.\\logtail\_installer.exe install ap-southeast-3|.\\logtail\_installer.exe install ap-southeast-3-internet|.\\logtail\_installer.exe install ap-southeast-3-acceleration|
    |**亚太东南 5（雅加达）**|.\\logtail\_installer.exe install ap-southeast-5|.\\logtail\_installer.exe install ap-southeast-5-internet|.\\logtail\_installer.exe install ap-southeast-5-acceleration|
    |**亚太南部 1（孟买）**|.\\logtail\_installer.exe install ap-south-1|.\\logtail\_installer.exe install ap-south-1-internet|.\\logtail\_installer.exe install ap-south-1-acceleration|
    |**亚太东北 1（日本）**|.\\logtail\_installer.exe install ap-northeast-1|.\\logtail\_installer.exe install ap-northeast-1-internet|.\\logtail\_installer.exe install ap-northeast-1-acceleration|
    |**欧洲中部 1（法兰克福）**|.\\logtail\_installer.exe install eu-central-1|.\\logtail\_installer.exe install eu-central-1-internet|.\\logtail\_installer.exe install eu-central-1-acceleration|
    |**中东东部 1（迪拜）**|.\\logtail\_installer.exe install me-east-1|.\\logtail\_installer.exe install me-east-1-internet|.\\logtail\_installer.exe install me-east-1-acceleration|
    |**英国（伦敦）**|.\\logtail\_installer.exe install eu-west-1|.\\logtail\_installer.exe install eu-west-1-internet|.\\logtail\_installer.exe install eu-west-1-acceleration|

    **说明：** 在自建IDC或其他云厂商服务器使用Logtail时，由于日志服务无法获取非本账号下ECS、其他服务器的属主信息，请在安装Logtail后手动配置用户标识（AliUid），否则Logtail心跳异常、无法收集日志。详细说明请参见 [为非本账号ECS、自建IDC配置AliUid](intl.zh-CN/用户指南/Logtail采集/机器组/为非本账号ECS、自建IDC配置AliUid.md)。


## 升级Logtail {#section_fks_lwg_cfb .section}

Windows版Logtail支持自动升级。

## 手动启动和停止Logtail {#section_u2t_4wg_cfb .section}

打开**控制面板**中的**管理工具**，打开**服务**，找到LogtailWorker。

-   **手动启动**：右键单击**停止**。

-   **停止**：右键单击**启动**。

-   **重启**：右键单击**重新启动**。


## 卸载Logtail {#section_bk1_14v_vdb .section}

打开 Windows Powershell或cmd进入 `logtail_installer` 目录，执行命令：

```
.\logtail_installer.exe uninstall
```

