# Install Logtail in Windows {#concept_j22_xnv_vdb .concept}

The Logtail client is a log collection agent provided by Log Service. This topic describes how to install the Logtail client on a Windows server.

## Supported systems {#section_ppj_ynv_vdb .section}

The Logtail client for Windows supports the following operating systems:

-   Windows 7 \(Client\) 32-bit
-   Windows 7 \(Client\) 64-bit
-   Windows Server 2008 32-bit
-   Windows Server 2008 64-bit
-   Windows Server 2012 64-bit
-   Windows Server 2016 64-bit

## Prerequisites {#section_t52_5zm_1fb .section}

1.  One or more servers are available.
2.  The network type for log collection is selected based on the server type and the region of the server. For more information, see [Select a network type](reseller.en-US/User Guide/Logtail collection/Select a network type.md).

    ![](images/12057_en-US.png "Select a network type")


## Install Logtail {#section_d1z_znv_vdb .section}

1.  Download the installation package.

    Download links:

    -   If you are in Mainland China, click [here](http://logtail-release.oss-cn-hangzhou.aliyuncs.com/win/logtail_installer.zip).
    -   If you are outside Mainland China, click [here](http://logtail-release-global.log-global.aliyuncs.com/win/logtail_installer.zip).
2.  Decompress the `logtail_installer.zip` package to the current directory.
3.  After [selecting a network type](reseller.en-US/User Guide/Logtail collection/Select a network type.md) based on the server type and region of the server, install Logtail based on the region of the Log Service project.

    Run Windows Powershell or CMD as an administrator to go to the `logtail_installer` directory where you decompress the Logtail installation package. Then, run the installation command based on the region and network type.

    The following table lists the installation commands for different network types in different regions.

    |Region|Alibaba Cloud intranet \(classic network or VPC\)|Internet|Global Acceleration|
    |:-----|:------------------------------------------------|:-------|:------------------|
    |**China \(Qingdao\)**|`.\logtail_installer.exe install cn-qingdao`|`.\logtail_installer.exe install cn-qingdao-internet`|`.\logtail_installer.exe install cn-qingdao-acceleration`|
    |**China \(Beijing\)**|`.\logtail_installer.exe install cn-beijing`|`.\logtail_installer.exe install cn-beijing-internet`|`.\logtail_installer.exe install cn-beijing-acceleration`|
    |**China \(Zhangjiakou\)**|`.\logtail_installer.exe install cn-zhangjiakou`|`.\logtail_installer.exe install cn-zhangjiakou-internet`|`.\logtail_installer.exe install cn-zhangjiakou-acceleration`|
    |**China \(Hohhot\)**|`.\logtail_installer.exe install cn-huhehaote`|`.\logtail_installer.exe install cn-huhehaote-internet`|`.\logtail_installer.exe install cn-huhehaote-acceleration`|
    |**China \(Hangzhou\)**|`.\logtail_installer.exe install cn-hangzhou`|`.\logtail_installer.exe install cn-hangzhou-internet`|`.\logtail_installer.exe install cn-hangzhou-acceleration`|
    |**China \(Shanghai\)**|`.\logtail_installer.exe install cn-shanghai`|`.\logtail_installer.exe install cn-shanghai-internet`|`.\logtail_installer.exe install cn-shanghai-acceleration`|
    |**China \(Shenzhen\)**|`.\logtail_installer.exe install cn-shenzhen`|`.\logtail_installer.exe install cn-shenzhen-internet`|`.\logtail_installer.exe install cn-shenzhen-acceleration`|
    |**China \(Chengdu\)**|`.\logtail_installer.exe install cn-chengdu`|`.\logtail_installer.exe install cn-chengdu-internet`|`.\logtail_installer.exe install cn-chengdu-acceleration`|
    |**Hong Kong**|`.\logtail_installer.exe install cn-hongkong`|`.\logtail_installer.exe install cn-hongkong-internet`|`.\logtail_installer.exe install cn-hongkong-acceleration`|
    |**US \(Silicon Valley\)**|`.\logtail_installer.exe install us-west-1`|`.\logtail_installer.exe install us-west-1-internet`|`.\logtail_installer.exe install us-west-1-acceleration`|
    |**US \(Virginia\)**|`.\logtail_installer.exe install us-east-1`|`.\logtail_installer.exe install us-east-1-internet`|`.\logtail_installer.exe install us-east-1-acceleration`|
    |**Singapore**|`.\logtail_installer.exe install ap-southeast-1`|`.\logtail_installer.exe install ap-southeast-1-internet`|`.\logtail_installer.exe install ap-southeast-1-acceleration`|
    |**Australia \(Sydney\)**|`.\logtail_installer.exe install ap-southeast-2`|`.\logtail_installer.exe install ap-southeast-2-internet`|`.\logtail_installer.exe install ap-southeast-2-acceleration`|
    |**Malaysia \(Kuala Lumpur\)**|`.\logtail_installer.exe install ap-southeast-3`|`.\logtail_installer.exe install ap-southeast-3-internet`|`.\logtail_installer.exe install ap-southeast-3-acceleration`|
    |**Indonesia \(Jakarta\)**|`.\logtail_installer.exe install ap-southeast-5`|`.\logtail_installer.exe install ap-southeast-5-internet`|`.\logtail_installer.exe install ap-southeast-5-acceleration`|
    |**India \(Mumbai\)**|`.\logtail_installer.exe install ap-south-1`|`.\logtail_installer.exe install ap-south-1-internet`|`.\logtail_installer.exe install ap-south-1-acceleration`|
    |**Japan \(Tokyo\)**|`.\logtail_installer.exe install ap-northeast-1`|`.\logtail_installer.exe install ap-northeast-1-internet`|`.\logtail_installer.exe install ap-northeast-1-acceleration`|
    |**Germany \(Frankfurt\)**|`.\logtail_installer.exe install eu-central-1`|`.\logtail_installer.exe install eu-central-1-internet`|`.\logtail_installer.exe install eu-central-1-acceleration`|
    |**UAE \(Dubai\)**|`.\logtail_installer.exe install me-east-1`|`.\logtail_installer.exe install me-east-1-internet`|`.\logtail_installer.exe install me-east-1-acceleration`|
    |**UK \(London\)**|`.\logtail_installer.exe install eu-west-1`|`.\logtail_installer.exe install eu-west-1-internet`|`.\logtail_installer.exe install eu-west-1-acceleration`|

    **Note:** If you use Logtail on a server deployed in an on-premises IDC or provided by another cloud product vendor, Log Service cannot obtain the owner information about ECS instances under other Alibaba Cloud accounts or other types of servers. In this case, you must manually configure AliUids after installing Logtail. Otherwise, Logtail has abnormal heartbeats and cannot collect logs. For more information, see [Configure AliUids for ECS servers under other Alibaba Cloud accounts or on-premises IDCs](reseller.en-US/User Guide/Logtail collection/Machine Group/Configure AliUids for ECS servers under other Alibaba Cloud accounts or on-premises IDCs.md).


## Installation path {#section_r5z_y32_qgb .section}

After you run the installation command, Logtail is installed in the specified path by default and cannot be changed. In this path, you can [view the Logtail version](#) in the app\_info.json file or [uninstall Logtail](#).

The installation path is as follows:

-   32-bit Windows: C:\\Program Files\\Alibaba\\Logtail
-   64-bit Windows: C:\\Program Files \(x86\)\\Alibaba\\Logtail

**Note:** You can run a 32-bit or 64-bit application in a Windows 64-bit system. However, the Windows 64-bit system stores 32-bit applications in an x86 folder to ensure compatibility.

Logtail for Windows is a 32-bit application. Therefore, it is installed in the Program Files \(x86\) folder in the Windows 64-bit system. If Logtail for 64-bit Window is available in the future, it is automatically installed in the Program Files folder.

## View the Logtail version {#section_dxc_1gf_ggb .section}

Logtail is automatically installed in the [default directory](#). To view the Logtail version, you can go to the directory and use Notepad or another text editor to open the app\_info.json file. The `logtail_version` field indicates the version of the installed Logtail.

In the following example, the Logtail version is 1.0.0.0:

```
{
	"logtail_version" : "1.0.0.0"
}
```

## Upgrade Logtail {#section_fks_lwg_cfb .section}

-   **Automatic upgrade**

    In normal cases, Logtail for Windows is automatically upgraded. However, you must manually upgrade Logtail earlier than 1.0.0.0 to Logtail 1.0.0.0 or a later version.

-   **Manual upgrade**

    You must manually upgrade Logtail earlier than 1.0.0.0 to Logtail 1.0.0.0 or a later version. The procedure for manually upgrading Logtail is the same as that for [installing Logtail](#). You only need to download and decompress the latest installation package and install Logtail by following the steps.

    **Note:** During manual upgrade, Logtail is automatically uninstalled and then reinstalled. In this case, files in the original installation directory are deleted. If necessary, we recommend that you back up the files before manually upgrading Logtail.


## Manually start and stop Logtail {#section_u2t_4wg_cfb .section}

In the **Control Panel**, choose System and Security \> **Administrative Tools**, and then double-click **Services**.

Find the target service based on your Logtail version:

-   Logtail 0.x.x.x: **LogtailWorker**.
-   Logtail 1.0.0.0 and later: **LogtailDaemon**.

Perform the following operations as required:

-   **Manually start Logtail**: Right-click Logtail and click **Start** from the drop-down menu.

-   **Stop Logtail**: Right-click Logtail and click **Stop** from the drop-down menu.

-   **Restart Logtail**: Right-click Logtail and click **Restart** from the drop-down menu.


## Uninstall Logtail {#section_bk1_14v_vdb .section}

Run Windows Powershell or CMD as an administrator to go to the `logtail_installer` directory where you decompress the Logtail installation package. Then, run the following command:

```
.\logtail_installer.exe uninstall
```

After Logtail is uninstalled, the Logtail [installation directory](#) is deleted. However, some residual configuration information is kept in the C:\\LogtailData path. You can manually delete the information as required. The residual configuration information includes:

-   checkpoint: contains checkpoint information of all plug-ins \(for example, the Windows event log plug-in\).
-   logtail\_check\_point: contains major checkpoint information of Logtail.
-   users: contains all AliUids.

