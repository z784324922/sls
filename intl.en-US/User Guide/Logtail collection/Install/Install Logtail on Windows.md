# Install Logtail on Windows {#concept_j22_xnv_vdb .concept}

## Supported systems  {#section_ppj_ynv_vdb .section}

Logtail supports Windows Server 2003 32/64bit and later version, such as:

-   Windows 7 \(Client\) 32bit 
-   Windows 7 \(Client\) 64bit 
-   Windows Server 2003 32bit 
-   Windows Server 2003 64bit 
-   Windows Server 2008 32bit 
-   Windows Server 2008 64bit 
-   Windows Server 2012 64bit 

## Install Logtail {#section_d1z_znv_vdb .section}

1.  Download the installation package

    [You can download the installation package at http://logtail-release.oss-cn-hangzhou.aliyuncs.com/win/logtail\_installer.zip. ](http://logtail-release.oss-cn-hangzhou.aliyuncs.com/win/logtail_installer.zip)

2.  Extract `logtail.zip` to the current directory.
3.  Install Logtail based on the network environment of your machine and the region of Log Service.

    Use Windows PowerShell or cmd.exe to enter the `logtail_installer` directory. Run the corresponding command based on the network environment of your machine and the region.

    Installation command:

    |Region|Classic Network and VPC|Internet \(self-built IDCs\)|
    |:-----|:----------------------|:---------------------------|
    |China North 1 \(Qingdao\) |.\\logtail\_installer.exe install cn-qingdao|.\\logtail\_installer.exe install cn-qingdao-internet|
    |China North 2 \(Beijing\)  |.\\logtail\_installer.exe install cn-beijing|.\\logtail\_installer.exe install cn-beijing-internet|
    |China North 3 \(Zhangjiakou\)|.\\logtail\_installer.exe install cn-zhangjiakou|.\\logtail\_installer.exe install cn-zhangjiakou-internet|
    |China North 5 \(Hohhot\)|.\\logtail\_installer.exe install cn-huhehaote |.\\logtail\_installer.exe install cn-huhehaote-internet|
    |China East 1 \(Hangzhou\) |.\\logtail\_installer.exe install cn-hangzhou|.\\logtail\_installer.exe install cn-hangzhou-internet|
    |China East 2 \(Shanghai\) |.\\logtail\_installer.exe install cn-shanghai|.\\logtail\_installer.exe install cn-shanghai-internet|
    |China South 1 \(Shenzhen\) |.\\logtail\_installer.exe install cn-shenzhen|.\\logtail\_installer.exe install cn-shenzhen-internet|
    |China \(Chengdu\)|.\\logtail\_installer.exe install cn-chengdu|.\\logtail\_installer.exe install cn-chengdu-internet|
    |Hong Kong|.\\logtail\_installer.exe install cn-hongkong|.\\logtail\_installer.exe install cn-hongkong-internet|
    |US \(Silicon Valley\)|.\\logtail\_installer.exe install us-west-1|.\\logtail\_installer.exe install us-west-1-internet|
    |East US 1 \(Virginia\)|.\\logtail\_installer.exe install us-east-1|.\\logtail\_installer.exe install us-east-1-internet|
    |Southeast Asia Pacific 1 \(Singapore\)|.\\logtail\_installer.exe install ap-southeast-1|.\\logtail\_installer.exe install ap-southeast-1-internet|
    |Southeast Asia Pacific 2 \(Sydney\)|.\\logtail\_installer.exe install ap-southeast-2|.\\logtail\_installer.exe install ap-southeast-2-internet|
    |Asia Pacific SE 3 \(Kuala Lumpur\) |.\\logtail\_installer.exe install ap-southeast-3|.\\logtail\_installer.exe install ap-southeast-3-internet|
    |Asia Pacific SE 5 \(Jakarta\) |.\\logtail\_installer.exe install ap-southeast-5|.\\logtail\_installer.exe install ap-southeast-5-internet|
    |Asia Pacific SOU 1 \(Mumbai\) |.\\logtail\_installer.exe install ap-south-1|.\\logtail\_installer.exe install ap-south-1-internet|
    |Asia Pacific NE 1 \(Japan\) |.\\logtail\_installer.exe install ap-northeast-1|.\\logtail\_installer.exe install ap-northeast-1-internet|
    |Central Europe 1 \(Frankfurt\)|.\\logtail\_installer.exe install eu-central-1|.\\logtail\_installer.exe install eu-central-1-internet|
    |Eastern Middle East 1 \(Dubai\)|.\\logtail\_installer.exe install me-east-1|.\\logtail\_installer.exe install me-east-1-internet|
    |China East 1 \(Hangzhou\) \(financial cloud\)  |.\\logtail\_installer.exe install cn-hangzhou-finance|None  |
    |China East 2 \(Shanghai\) \(financial cloud\) |.\\logtail\_installer.exe install cn-shanghai-finance|None  |
    |China South 1 \(Shenzhen\) \(financial cloud\) |.\\logtail\_installer.exe install cn-shenzhen-finance|None  |

    **Note:** Log Service cannot obtain the owner information of non-Alibaba Cloud machines. Therefore, you must manually configure the user identification after installing Logtail when Logtail is used by self-built IDCs or other cloud hosts. Otherwise, Logtail has abnormal heartbeats and cannot collect logs. For more information, see [Collect logs from non-Alibaba Cloud ECS instances or ECS instances not in your account](reseller.en-US/User Guide/Logtail collection/Machine Group/Collect logs from non-Alibaba Cloud ECS instances or ECS instances not in your account.md).


## Uninstall Logtail {#section_bk1_14v_vdb .section}

Use Windows PowerShell or cmd.exe to enter the `logtail_installer`  directory and run the following command: 

```
.\logtail_installer.exe uninstall
```

