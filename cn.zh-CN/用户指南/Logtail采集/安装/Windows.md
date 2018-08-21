# Windows {#concept_j22_xnv_vdb .concept}

## 支持的系统 {#section_ppj_ynv_vdb .section}

支持Windows Server2003（含）以后 32/64 位系统，例如：

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

2.  解压缩 `logtail.zip` 到当前目录。
3.  按机器网络环境和日志服务所在Region进行安装。

    打开Windows Powershell或cmd进入 `logtail_installer` 目录，并根据地域和网络环境执行相应的安装命令。

    安装命令：

    |区域|经典网络/VPC|公网（自建IDC）|
    |:-|:-------|:--------|
    |华北 1（青岛）|.\\logtail\_installer.exe install cn-qingdao|.\\logtail\_installer.exe install cn-qingdao-internet|
    |华北 2（北京）|.\\logtail\_installer.exe install cn-beijing|.\\logtail\_installer.exe install cn-beijing-internet|
    |华北 3 （张家口）|.\\logtail\_installer.exe install cn-zhangjiakou|.\\logtail\_installer.exe install cn-zhangjiakou-internet|
    |华北 5 （呼和浩特）|.\\logtail\_installer.exe install cn-huhehaote|.\\logtail\_installer.exe install cn-huhehaote-internet|
    |华东 1（杭州）|.\\logtail\_installer.exe install cn-hangzhou|.\\logtail\_installer.exe install cn-hangzhou-internet|
    |华东 2（上海）|.\\logtail\_installer.exe install cn-shanghai|.\\logtail\_installer.exe install cn-shanghai-internet|
    |华南 1（深圳）|.\\logtail\_installer.exe install cn-shenzhen|.\\logtail\_installer.exe install cn-shenzhen-internet|
    |西南 1（成都）|.\\logtail\_installer.exe install cn-chengdu|.\\logtail\_installer.exe install cn-chengdu-internet|
    |香港|.\\logtail\_installer.exe install cn-hongkong|.\\logtail\_installer.exe install cn-hongkong-internet|
    |美国西部 1 （硅谷）|.\\logtail\_installer.exe install us-west-1|.\\logtail\_installer.exe install us-west-1-internet|
    |美国东部 1（弗吉尼亚）|.\\logtail\_installer.exe install us-east-1|.\\logtail\_installer.exe install us-east-1-internet|
    |亚太东南 1（新加坡）|.\\logtail\_installer.exe install ap-southeast-1|.\\logtail\_installer.exe install ap-southeast-1-internet|
    |亚太东南 2（悉尼）|.\\logtail\_installer.exe install ap-southeast-2|.\\logtail\_installer.exe install ap-southeast-2-internet|
    |亚太东南 3（吉隆坡）|.\\logtail\_installer.exe install ap-southeast-3|.\\logtail\_installer.exe install ap-southeast-3-internet|
    |亚太东南 5（雅加达）|.\\logtail\_installer.exe install ap-southeast-5|.\\logtail\_installer.exe install ap-southeast-5-internet|
    |亚太南部 1（孟买）|.\\logtail\_installer.exe install ap-south-1|.\\logtail\_installer.exe install ap-south-1-internet|
    |亚太东北 1（日本）|.\\logtail\_installer.exe install ap-northeast-1|.\\logtail\_installer.exe install ap-northeast-1-internet|
    |欧洲中部 1（法兰克福）|.\\logtail\_installer.exe install eu-central-1|.\\logtail\_installer.exe install eu-central-1-internet|
    |中东东部 1（迪拜）|.\\logtail\_installer.exe install me-east-1|.\\logtail\_installer.exe install me-east-1-internet|
    |华东 1金融云（杭州）|.\\logtail\_installer.exe install cn-hangzhou-finance|无此类型网络|
    |华东 2金融云（上海）|.\\logtail\_installer.exe install cn-shanghai-finance|无此类型网络|
    |华南 1金融云（深圳）|.\\logtail\_installer.exe install cn-shenzhen-finance|无此类型网络|

    **说明：** 在自建IDC或其它云主机使用Logtail时，由于日志服务无法获取非阿里云机器的属主信息，请在安装Logtail后手动配置用户标识，否则Logtail心跳异常、无法收集日志。详细说明请参见 [非本人ECS（或线下机器）](intl.zh-CN/用户指南/Logtail采集/机器组/非本人ECS（或线下机器）.md)。


## 卸载Logtail {#section_bk1_14v_vdb .section}

打开 Windows Powershell或cmd进入 `logtail_installer` 目录，执行命令：

```
.\logtail_installer.exe uninstall
```

