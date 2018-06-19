# Windows {#concept_j22_xnv_vdb .concept}

## 支持的系统 {#section_ppj_ynv_vdb .section}

支持Windows Server2003（含）以后 32/64 位系统

-   Windows 7 \(Client\) 32bit
-   Windows 7 \(Client\) 64bit
-   Windows Server 2003 32bit
-   Windows Server 2003 64bit
-   Windows Server 2008 32bit
-   Windows Server 2008 64bit
-   Windows Server 2012 64bit

## 安装Logtail {#section_d1z_znv_vdb .section}

1.  下载安装包。

    [http://logtail-release.oss-cn-hangzhou.aliyuncs.com/win/logtail\_installer.zip](http://logtail-release.oss-cn-hangzhou.aliyuncs.com/win/logtail_installer.zip)

2.  按机器网络环境和日志服务所在Region进行安装。

    解压缩 `logtail.zip` 到当前目录，打开Windows Powershell或cmd进入 `logtail_installer` 目录。

    |日志服务 Region|机器网络环境|安装命令|
    |:----------|:-----|:---|
    |华北 1（青岛）|ECS 经典网络|.\\logtail\_installer.exe install cn\_qingdao|
    | |ECS 专有网络 VPC|.\\logtail\_installer.exe install cn\_qingdao\_vpc|
    | |公网（自建 IDC 或其它云主机）|.\\logtail\_installer.exe install cn\_qingdao\_internet|
    |华北 2（北京）|ECS 经典网络|.\\logtail\_installer.exe install cn\_beijing|
    | |ECS 专有网络 VPC|.\\logtail\_installer.exe install cn\_beijing\_vpc|
    | |公网（自建 IDC 或其它云主机）|.\\logtail\_installer.exe install cn\_beijing\_internet|
    |华北 3 （张家口）|ECS 经典网络|.\\logtail\_installer.exe install cn-zhangjiakou|
    | |ECS 专有网络 VPC|.\\logtail\_installer.exe install cn-zhangjiakou\_vpc|
    | |公网（自建 IDC 或其它云主机）|.\\logtail\_installer.exe install cn-zhangjiakou\_internet|
    |华北 5 （呼和浩特）|ECS 专有网络 VPC|.\\logtail\_installer.exe install cn-huhehaote|
    | |公网（自建 IDC 或其它云主机）|.\\logtail\_installer.exe install cn-huhehaote\_internet|
    |华东 1（杭州）|ECS 经典网络|.\\logtail\_installer.exe install cn\_hangzhou|
    | |ECS 专有网络 VPC|.\\logtail\_installer.exe install cn\_hangzhou\_vpc|
    | |公网（自建 IDC 或其它云主机）|.\\logtail\_installer.exe install cn\_hangzhou\_internet|
    | |ECS 金融云|.\\logtail\_installer.exe install cn\_hangzhou\_finance|
    |华东 2（上海）|ECS 经典网络|.\\logtail\_installer.exe install cn\_shanghai|
    | |ECS 专有网络 VPC|.\\logtail\_installer.exe install cn\_shanghai\_vpc|
    | |公网（自建 IDC 或其它云主机）|.\\logtail\_installer.exe install cn\_shanghai\_internet|
    | |ECS 金融云|.\\logtail\_installer.exe install cn-shanghai-finance|
    |华南 1（深圳）|ECS 经典网络|.\\logtail\_installer.exe install cn\_shenzhen|
    | |ECS 专有网络 VPC|.\\logtail\_installer.exe install cn\_shenzhen\_vpc|
    | |公网（自建 IDC 或其它云主机）|.\\logtail\_installer.exe install cn\_shenzhen\_internet|
    | |ECS 金融云|.\\logtail\_installer.exe install cn\_shenzhen\_finance|
    |香港|ECS 经典网络|.\\logtail\_installer.exe install cn-hongkong|
    | |ECS 专有网络 VPC|.\\logtail\_installer.exe install cn-hongkong\_vpc|
    | |公网（自建 IDC 或其它云主机）|.\\logtail\_installer.exe install cn-hongkong\_internet|
    |美国西部 1 （硅谷）|ECS 经典网络|.\\logtail\_installer.exe install us-west-1|
    | |ECS 专有网络 VPC|.\\logtail\_installer.exe install us-west-1\_vpc|
    | |公网（自建 IDC 或其它云主机）|.\\logtail\_installer.exe install us-west-1\_internet|
    |美国东部 1 （弗吉尼亚）|ECS 专有网络 VPC|.\\logtail\_installer.exe install us-east-1|
    | |公网（自建 IDC 或其它云主机）|.\\logtail\_installer.exe install us-east-1\_internet|
    |亚太东南 1 （新加坡）|ECS 经典网络|.\\logtail\_installer.exe install ap-southeast-1|
    | |ECS 专有网络 VPC|.\\logtail\_installer.exe install ap-southeast-1\_vpc|
    | |公网（自建 IDC 或其它云主机）|.\\logtail\_installer.exe install ap-southeast-1\_internet|
    |亚太东南 2 （悉尼）|ECS 经典网络|.\\logtail\_installer.exe install ap-southeast-2|
    | |ECS 专有网络 VPC|.\\logtail\_installer.exe install ap-southeast-2\_vpc|
    | |公网（自建 IDC 或其它云主机）|.\\logtail\_installer.exe install ap-southeast-2\_internet|
    |亚太东南 3 （吉隆坡）|ECS 专有网络 VPC|.\\logtail\_installer.exe install ap-southeast-3|
    | |公网（自建 IDC 或其它云主机）|.\\logtail\_installer.exe install ap-southeast-3\_internet|
    |亚太东北 1 （日本）|ECS 经典网络|.\\logtail\_installer.exe install ap-northeast-1|
    | |ECS 专有网络 VPC|.\\logtail\_installer.exe install ap-northeast-1\_vpc|
    | |公网（自建 IDC 或其它云主机）|.\\logtail\_installer.exe install ap-northeast-1\_internet|
    |欧洲中部 1 （法兰克福）|ECS 经典网络|.\\logtail\_installer.exe install eu-central-1|
    | |ECS 专有网络 VPC|.\\logtail\_installer.exe install eu-central-1\_vpc|
    | |公网（自建 IDC 或其它云主机）|.\\logtail\_installer.exe install eu-central-1\_internet|
    |中东东部 1 （迪拜）|ECS 经典网络|.\\logtail\_installer.exe install me-east-1|
    | |ECS 专有网络 VPC|.\\logtail\_installer.exe install me-east-1\_vpc|
    | |公网（自建 IDC 或其它云主机）|.\\logtail\_installer.exe install me-east-1\_internet|

    **说明：** 在自建IDC或其它云主机使用Logtail时，由于日志服务无法获取非阿里云机器的属主信息，请在安装Logtail后手动配置用户标识，参见 [非本人ECS（或线下机器）](intl.zh-CN/用户指南/Logtail 采集/机器组/非本人ECS（或线下机器）.md)，否则Logtail心跳异常、无法收集日志。


## 卸载Logtail {#section_bk1_14v_vdb .section}

打开 Windows Powershell或cmd进入 `logtail_installer` 目录，执行命令：

```
.\logtail_installer.exe uninstall
```

