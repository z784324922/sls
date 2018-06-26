# Install Logtail on Windows {#concept_j22_xnv_vdb .concept}

## Supported systems  {#section_ppj_ynv_vdb .section}

Logtail supports the following systems:

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

2.  Install Logtail based on the network environment of your machine and the region of Log Service

    Extract `logtail.zip` to the current directory  and use Windows PowerShell or cmd.exe to enter the `logtail_installer` directory. 

    |Region of Log Service |Network environment of your machine |Installation command |
    |:---------------------|:-----------------------------------|:--------------------|
    |China North 1 \(Qingdao\)|Elastic Compute Service \(ECS\) instances of classic network|.\\logtail\_installer.exe install cn\_qingdao |
    | |ECS instances of VPC |.\\logtail\_installer.exe install  cn\_qingdao\_vpc |
    | |Internet \(self-built IDCs or other cloud hosts\)|.\\logtail\_installer.exe install cn\_qingdao\_internet |
    |China North 2 \(Beijing\)|ECS instances of classic network |.\\logtail\_installer.exe install cn\_beijing |
    | |ECS instances of VPC |.\\logtail\_installer.exe install  cn\_beijing\_vpc |
    | |Internet \(self-built IDCs or other cloud hosts\)|.\\logtail\_installer.exe install  cn\_beijing\_internet |
    |China North 3 \(Zhangjiakou\)|ECS instances of classic network|.\\logtail\_installer.exe install  cn-zhangjiakou |
    | |ECS instances of VPC |.\\logtail\_installer.exe install  cn-zhangjiakou\_vpc |
    | |Internet \(self-built IDCs or other cloud hosts\)|.\\logtail\_installer.exe install  cn-zhangjiakou\_internet |
    |China North 5 \(Huhehaote\)|ECS instances of VPC |.\\logtail\_installer.exe install cn-huhehaote |
    | |Internet \(self-built IDCs or other cloud hosts\) |.\\logtail\_installer.exe install  cn-huhehaote\_internet |
    |China East 1 \(Hangzhou\) |ECS instances of classic network |.\\logtail\_installer.exe install cn\_hangzhou |
    | |ECS instances of VPC |.\\logtail\_installer.exe install  cn\_hangzhou\_vpc |
    | |Internet \(self-built IDCs or other cloud hosts\) |.\\logtail\_installer.exe install  cn\_hangzhou\_internet |
    | |ECS instances of AntCloud |.\\logtail\_installer.exe install  cn\_hangzhou\_finance |
    |China East 2 \(Shanghai\) |ECS instances of classic network |.\\logtail\_installer.exe install cn\_shanghai |
    | |ECS instances of VPC |.\\logtail\_installer.exe install  cn\_shanghai\_vpc |
    | |Internet \(self-built IDCs or other cloud hosts\) |.\\logtail\_installer.exe install  cn\_shanghai\_internet|
    | |ECS instances of AntCloud |.\\logtail\_installer.exe install  cn-shanghai-finance |
    |China South 1 \(Shenzhen\) |ECS instances of classic network |.\\logtail\_installer.exe install cn\_shenzhen |
    | |ECS instances of VPC |.\\logtail\_installer.exe install  cn\_shenzhen\_vpc |
    | |Internet \(self-built IDCs or other cloud hosts\) |.\\logtail\_installer.exe install  cn\_shenzhen\_internet |
    | |ECS instances of AntCloud |.\\logtail\_installer.exe install  cn\_shenzhen\_finance |
    |Hong Kong |ECS instances of classic network |.\\logtail\_installer.exe install cn-hongkong |
    | |ECS instances of VPC |.\\logtail\_installer.exe install  cn-hongkong\_vpc |
    | |Internet \(self-built IDCs or other cloud hosts\) |.\\logtail\_installer.exe install  cn-hongkong\_internet |
    |US West 1 \(Silicon Valley\)|ECS instances of classic network |.\\logtail\_installer.exe install us-west-1 |
    | |ECS instances of VPC |.\\logtail\_installer.exe install us-west-1\_vpc |
    | |Internet \(self-built IDCs or other cloud hosts\) |.\\logtail\_installer.exe install  us-west-1\_internet |
    |US East 1 \(Virginia\) |ECS instances of VPC |.\\logtail\_installer.exe install us-east-1 |
    | |Internet \(self-built IDCs or other cloud hosts\) |.\\logtail\_installer.exe install  us-east-1\_internet|
    |Asia Pacific SE 1 \(Singapore\) |ECS instances of classic network |.\\logtail\_installer.exe install ap-southeast-1|
    | |ECS instances of VPC |.\\logtail\_installer.exe install ap-southeast-1\_vpc |
    | |Internet \(self-built IDCs or other cloud hosts\) |.\\logtail\_installer.exe install  ap-southeast-1\_internet |
    |Asia Pacific SE 2 \(Sydney\)|ECS instances of classic network |.\\logtail\_installer.exe install  ap-southeast-2|
    | |ECS instances of VPC |.\\logtail\_installer.exe install  ap-southeast-2\_vpc |
    | |Internet \(self-built IDCs or other cloud hosts\) |.\\logtail\_installer.exe install  ap-southeast-2\_internet |
    |Asia Pacific SE 3 \(Kuala Lumpur\) |ECS instances of VPC |.\\logtail\_installer.exe install  ap-southeast-3|
    | |Internet \(self-built IDCs or other cloud hosts\) |.\\logtail\_installer.exe install  ap-southeast-3\_internet |
    |Asia Pacific NE 1 \(Japan\) |ECS instances of classic network |.\\logtail\_installer.exe install  ap-northeast-1|
    | |ECS instances of VPC |.\\logtail\_installer.exe install  ap-northeast-1\_vpc|
    | |Internet \(self-built IDCs or other cloud hosts\) |.\\logtail\_installer.exe install  ap-northeast-1\_internet|
    |EU Central 1 \(Frankfurt\) |ECS instances of classic network|.\\logtail\_installer.exe install eu-central-1 |
    | |ECS instances of VPC |.\\logtail\_installer.exe install  eu-central-1\_vpc|
    | |Internet \(self-built IDCs or other cloud hosts\) |.\\logtail\_installer.exe install  eu-central-1\_internet|
    |Middle East 1 \(Dubai\) |ECS instances of classic network |.\\logtail\_installer.exe install me-east-1 |
    | |ECS instances of VPC |.\\logtail\_installer.exe install me-east-1\_vpc |
    | |Internet \(self-built IDCs or other cloud hosts\) |.\\logtail\_installer.exe install me-east-1\_internet|

    **Note:** Log Service cannot obtain the owner information of non-Alibaba Cloud machines. Therefore, you must manually configure the user identification after installing Logtail when Logtail is used by self-built IDCs or other cloud hosts. [Collect logs from non-Alibaba Cloud ECS instances or ECS instances not in your account](intl.en-US/User Guide/Logtail collection/Machine Group/Collect logs from non-Alibaba Cloud ECS instances or ECS instances not in your account.md)For more information, see Configure user identification for non-Alibaba Cloud ECS instances. Otherwise, Logtail has abnormal heartbeats and cannot collect logs.


## Uninstall Logtail {#section_bk1_14v_vdb .section}

Use Windows PowerShell or cmd.exe to enter the `logtail_installer`  directory and run the following command: 

```
.\logtail_installer.exe uninstall 
```

