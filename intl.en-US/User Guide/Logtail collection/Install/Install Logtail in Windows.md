# Install Logtail in Windows {#concept_j22_xnv_vdb .concept}

This topic describes how to install the Logtail client on Windows servers.

## Supported systems {#section_ppj_ynv_vdb .section}

The following Windows systems are supported:

-   Windows 7 \(Client\) 32-bit
-   Windows 7 \(Client\) 64-bit
-   Windows Server 2008 32-bit
-   Windows Server 2008 64-bit
-   Windows Server 2012 64-bit
-   Windows Server 2016 64-bit

## Prerequisites {#section_t52_5zm_1fb .section}

-   Your account has at least one server under it.
-   You have determined the required network type for log collection according to the server type and region to which the server belongs. For more information, see [Select a network type](reseller.en-US/User Guide/Logtail collection/Select a network type.md).

    ![](images/38900_en-US.png "Select a network type")


## Install Logtail {#section_d1z_znv_vdb .section}

1.  Download the required Logtail installation package as follows:
    -   For users in Mainland China: [Logtail installation package](http://logtail-release.oss-cn-hangzhou.aliyuncs.com/win/logtail_installer.zip)
    -   For users in all other areas: [Logtail installation package](http://logtail-release-global.log-global.aliyuncs.com/win/logtail_installer.zip)
2.  Decompress `logtail_installer.zip` to the current directory.
3.  Install Logtail according to the region to which the Log Service project belongs.

    Run Windows PowerShell or CMD as the admin user to enter the `logtail_installer` directory, where you decompress the Logtail installation package. Then, run the installation command according to the region and network type.

    The following table describes installation commands you can use according to the network type.

    |Region|Alibaba Cloud intranet|Internet|Global acceleration|
    |:-----|:---------------------|:-------|:------------------|
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

    **Note:** When you use Logtail on a server deployed in an on-premises IDC or provided by another cloud product vendor, Log Service cannot obtain the owner information about ECS servers under other Alibaba Cloud accounts or other types of servers. In this case, after installing Logtail, you need to manually configure AliUids for the servers by following the instructions provided in [Configure AliUids for ECS servers under other Alibaba Cloud accounts and on-premises IDCs](reseller.en-US/User Guide/Logtail collection/Machine Group/Collect logs from non-Alibaba Cloud ECS instances or ECS instances not in your account.md). Otherwise, Logtail heartbeats become abnormal, or Logtail cannot collect logs.


## Installation path {#section_r5z_y32_qgb .section}

By default, Logtail is installed in the specified path, which cannot be changed. In this path, you can [view the Logtail version](#) in the app\_info.json file or [uninstall Logtail](#).

The installation path is:

-   C:\\Program Files\\Alibaba\\Logtail in Windows 32-bit systems
-   C:\\Program Files \(x86\)\\Alibaba\\Logtail in Windows 64-bit systems

**Note:** Logtail is a 32-bit program. For Windows 64-bit systems, Logtail will be installed in the Program Files \(x86\) directory.

## View the Logtail version {#section_dxc_1gf_ggb .section}

Logtail is automatically installed to the [default directory](#). You can open the app\_info.json file to view the `logtail_version` field, which indicates your Logtail version.

The following examples shows that the Logtail version is 1.0.0.0:

```
{
	"logtail_version" : "1.0.0.0"
}
```

## Upgrade Logtail {#section_fks_lwg_cfb .section}

-   **Automatic upgrade**

    In normal cases, Windows supports automatic Logtail upgrades.

-   **Manual upgrade**

    You need to manually upgrade Logtail if the source version is earlier than 1.0.0.0 and the target version is 1.0.0.0 or later. For more information about how to manually upgrade Logtail, see [Install Logtail](#).

    **Note:** Manually upgrading Logtail means that Logtail will be automatically uninstalled and then reinstalled. In this case, files in the original installation path will be deleted. We recommend that you back up the files before manually upgrading Logtail.


## Manually start and stop Logtail {#section_u2t_4wg_cfb .section}

In the **Control Panel**, choose **System and Security** \> **Administrative Tools**, and then open the **Services** program.

Find the target service according to your Logtail version:

-   For Logtail 0.x.x.x: **LogtailWorker**
-   For Logtail 1.0.0.0 and later: **LogtailDaemon**

Then, perform the following operations as needed:

-   **Manually start Logtail**: Right-click Logtail and click **Start** in the shortcut menu.
-   **Stop Logtail**: Right-click Logtail and click **Stop** in the shortcut menu.
-   **Restart Logtail**: Right-click Logtail and click **Restart** in the shortcut menu.

## Uninstall Logtail {#section_bk1_14v_vdb .section}

Run Windows PowerShell or CMD as the admin user to enter the `logtail_installer` directory, namely, the directory for decompressing the installation package you have downloaded. Then, run the following command:

```
.\logtail_installer.exe uninstall
```

After Logtail is deleted successfully, the Logtail [installation path](#) will be deleted. However, some residual configuration information will be retained in the C:\\LogtailData directory. You can manually delete the information as needed. The information includes the following:

-   checkpoint: contains checkpoint information of all agents \(for example, the Windows event log agent\).
-   logtail\_check\_point: contains major checkpoint information of Logtail.
-   users: contains all AliUids.

