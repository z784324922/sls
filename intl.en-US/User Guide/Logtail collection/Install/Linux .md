# Linux  {#concept_u5y_3lv_vdb .concept}

## Supported systems  {#section_tjc_wlv_vdb .section}

Logtail supports the Linux x86-64 \(64 bit\) servers in the following releases: 

-   Aliyun Linux 
-   Ubuntu 
-   Debian 
-   CentOS 
-   OpenSUSE

## Install Logtail  {#section_h2x_fmv_vdb .section}

Install Logtail in overwrite mode. If you have installed Logtail before, the installer uninstalls the Logtail and deletes the `/usr/local/ilogtail` directory before installing Logtail. By default, Logtail is started after the installation and at startup. 

Download the installer based on the network environment of your machine and the region of Log Service. Select different parameters for installation. 

Follow the steps in Installation method of this document to install Logtail.  If the installation fails,[open a ticket](https://selfservice.console.aliyun.com/ticket/category/sls/today).

**Installation method**

To install Logtail, download and run the installation script. You must select the **installation parameters** based on the region and network type.

**Installation parameters **

**Note:** To install Logtail in Docker or Kubernetes, the `${your_region_name}` is the parameter in the following table.  Copy the corresponding installation statement directly. 

The installation parameters for different regions and network types are as follows \(we recommend that you copy the corresponding installation statement directly\). 

|Region|Classic Network and VPC|Internet \(self-built IDCs\)|
|:-----|:----------------------|:---------------------------|
|China North 1 \(Qingdao\) |cn-qingdao|cn-qingdao-internet|
|China North 2 \(Beijing\)  |cn-beijing|cn-beijing-internet|
|China East 1 \(Hangzhou\)|cn-hangzhou|cn-hangzhou-internet|
|China East 2 \(Shanghai\) |cn-shanghai|cn-shanghai-internet|
|China North 3 \(Zhangjiakou\)|cn-zhangjiakou|cn-zhangjiakou-internet|
|China North 5 \(Huhehaote\) |cn-huhehaote|cn-huhehaote-internet|
|China South 1 \(Shenzhen\)|cn-shenzhen|cn-shenzhen-internet|
|China \(Chengdu\)|cn-chengdu|cn-chengdu-internet|
|Hong Kong|cn-hongkong|cn-hongkong-internet|
|US West 1 \(Silicon Valley\) |us-west-1|us-west-1-internet|
|East US 1 \(Virginia\)|us-east-1|us-east-1-internet|
|Asia Pacific SE 1 \(Singapore\) |Ap-southeast-1 |ap-southeast-1-internet|
|Asia Pacific SE 2 \(Sydney\) |ap-southeast-2 |ap-southeast-2-internet|
|Asia Pacific SE 3 \(Kuala Lumpur\) |ap-southeast-3|ap-southeast-3-internet|
|Asia Pacific SE 5 \(Jakarta\) |ap-southeast-5|ap-southeast-5-internet|
|Asia Pacific SOU 1 \(Mumbai\) |ap-south-1|ap-south-1-internet|
|Asia Pacific NE 1 \(Japan\) |ap-northeast-1|ap-northeast-1-internet|
|EU Central 1 \(Frankfurt\)  |eu-central-1|eu-central-1-internet|
|Middle East 1 \(Dubai\) |me-east-1|me-east-1-internet|
|China East 1 \(Hangzhou\) \(financial cloud\)  |cn-hangzhou-finance|None  |
|China East 2 \(Shanghai\) \(financial cloud\) |cn-shanghai-finance|None  |
|China South 1 \(Shenzhen\) \(financial cloud\) |cn-shenzhen-finance|None  |

## ECS（Classic Network、VPC） {#section_m32_rmv_vdb .section}

Data on ECS is written to Log Service by means of the Alibaba Cloud intranet without consuming Internet bandwidth.

**Automatically select the region parameter**:

If you cannot determine the region where the ECS is located or its identity, you can use the auto parameter of the Logtail installer to install the Logtail. When the parameter is specified, the Logtail installer will get your [Metadata](../../../../intl.en-US/User Guide/Instances/User-defined data and metadata/Metadata.md)through the server and automatically determine the region.

The procedure is as follows:

1.  Obtain the Logtail installer through the public network. This step involves accessing the public network, which consumes about 10 KB of public network traffic.

    ```
    $ wget http://logtail-release-cn-hangzhou.oss-cn-hangzhou.aliyuncs.com/linux64/logtail.sh -O logtail.sh;chmod 755 logtail.sh
    ```

2.  Use the auto parameter for installation. This step automatically downloads the installation program for the corresponding region without consuming public network traffic.

    ```
    $ ./logtail.sh install auto
    ```


**Manually select the region parameter**

If the installation fails with the auto parameter, you can choose manual installation. Execute the following command to install directly.

**Note:** 

-   Replace `${your_region_name}` in the following command with the region in which your ECS is located, for example, `cn-beijing` and `cn-hangzhou`.
-   The installer is obtained through the internal network without consuming public network traffic.

```
wget http://logtail-release-$\{your\_region\_name\}.oss-$\{your\_region\_name\}-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install $\{your\_region\_name\}
```

You can also perform installation by executing one of the following command corresponding to the region where your ECS is located:

-   China North 2 \(Beijing\)  

    ```
    wget http://logtail-release-cn-beijing.oss-cn-beijing-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-beijing
    ```

-   China North 1 \(Qingdao\) 

    ```
    wget http://logtail-release-cn-qingdao.oss-cn-qingdao-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-qingdao
    ```

-   China East 1 \(Hangzhou\) 

    ```
    wget http://logtail-release-cn-hangzhou.oss-cn-hangzhou-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-hangzhou
    ```

-   China East 2 \(Shanghai\) 

    ```
    wget http://logtail-release-cn-shanghai.oss-cn-shanghai-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-shanghai
    ```

-   China South 1 \(Shenzhen\) 

    ```
    wget http://logtail-release-cn-shenzhen.oss-cn-shenzhen-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-shenzhen
    ```

-   China North 3 \(Zhangjiakou\)

    ```
    wget http://logtail-release-cn-zhangjiakou.oss-cn-zhangjiakou-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-zhangjiakou
    ```

-   North China 5 \(Hohhot\)

    ```
    wget http://logtail-release-cn-huhehaote.oss-cn-huhehaote-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-huhehaote
    ```

-   China \(Chengdu\)

    ```
    wget http://logtail-release-cn-chengdu.oss-cn-chengdu-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-chengdu
    ```

-   Hong Kong

    ```
    wget http://logtail-release-cn-hongkong.oss-cn-hongkong-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-hongkong
    ```

-   US \(Silicon Valley\)

    ```
    wget http://logtail-release-us-west-1.oss-us-west-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install us-west-1
    ```

-   East US 1 \(Virginia\)

    ```
    wget http://logtail-release-us-east-1.oss-us-east-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install us-east-1
    ```

-   Asia Pacific SE 1 \(Singapore\) 

    ```
    wget http://logtail-release-ap-southeast-1.oss-ap-southeast-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-1
    ```

-   Asia Pacific SE 2 \(Sydney\)

    ```
    wget http://logtail-release-ap-southeast-2.oss-ap-southeast-2-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-2
    ```

-   Asia Pacific SE 3 \(Kuala Lumpur\) 

    ```
    wget http://logtail-release-ap-southeast-3.oss-ap-southeast-3-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-3
    ```

-   Asia Pacific SE 5 \(Jakarta\) 

    ```
    wget http://logtail-release-ap-southeast-5.oss-ap-southeast-5-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-5
    ```

-   Asia Pacific NE 1 \(Japan\) 

    ```
    wget http://logtail-release-ap-northeast-1.oss-ap-northeast-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-northeast-1
    ```

-   Asia Pacific SOU 1 \(Mumbai\) 

    ```
    wget http://logtail-release-ap-south-1.oss-ap-south-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-south-1
    ```

-   EU Central 1 \(Frankfurt\) 

    ```
    wget http://logtail-release-eu-central-1.oss-eu-central-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install eu-central-1
    ```

-   Middle East 1 \(Dubai\) 

    ```
    wget http://logtail-release-me-east-1.oss-me-east-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install me-east-1
    ```


## Internet \(self-built IDCs or other cloud hosts\) {#section_xc3_5mv_vdb .section}

Data is written to Log Service by means of Internet, which occupies Internet bandwidth  and is applicable to non-Alibaba Cloud virtual machines or other IDCs.

**Note:** Log Service cannot obtain the owner information of non-Alibaba Cloud machines. Therefore, you must manually configure the user identification after installing Logtail. For more information, see [Collect logs from non-Alibaba Cloud ECS instances or ECS instances not in your account](intl.en-US/User Guide/Logtail collection/Machine Group/Collect logs from non-Alibaba Cloud ECS instances or ECS instances not in your account.md). Otherwise, Logtail has abnormal heartbeat and cannot collect logs.

Replace `${your_region_name}`in the following command with the region in which your Log Service project is located, for example, `cn-beijing` and `cn-hangzhou`.

```
wget http://logtail-release-$\{your\_region\_name\}.oss-$\{your\_region\_name\}.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install $\{your\_region\_name\}-internet
```

You can also perform installation by executing one of the following command corresponding to the region where your Log Service project is located:

-   China North 2 \(Beijing\)  

    ```
    wget http://logtail-release-cn-beijing.oss-cn-beijing.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-beijing-internet
    ```

-   China North 1 \(Qingdao\) 

    ```
    wget http://logtail-release-cn-qingdao.oss-cn-qingdao.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-qingdao-internet
    ```

-   China East 1 \(Hangzhou\) 

    ```
    wget http://logtail-release-cn-hangzhou.oss-cn-hangzhou.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-hangzhou-internet
    ```

-   China East 2 \(Shanghai\) 

    ```
    wget http://logtail-release-cn-shanghai.oss-cn-shanghai.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-shanghai-internet
    ```

-   China South 1 \(Shenzhen\) 

    ```
    wget http://logtail-release-cn-shenzhen.oss-cn-shenzhen.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-shenzhen-internet
    ```

-   China North 3 \(Zhangjiakou\)

    ```
    wget http://logtail-release-cn-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-zhangjiakou-internet
    ```

-   North China 5 \(Hohhot\)

    ```
    wget http://logtail-release-cn-huhehaote.oss-cn-huhehaote.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-huhehaote-internet
    ```

-   China \(Chengdu\)

    ```
    wget http://logtail-release-cn-chengdu.oss-cn-chengdu.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-chengdu-internet
    ```

-   Hong Kong

    ```
    wget http://logtail-release-cn-hongkong.oss-cn-hongkong.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-hongkong-internet
    ```

-   US \(Silicon Valley\)

    ```
    wget http://logtail-release-us-west-1.oss-us-west-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install us-west-1-internet
    ```

-   US \(Virginia\)

    ```
    wget http://logtail-release-us-east-1.oss-us-east-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install us-east-1-internet
    ```

-   Asia Pacific SE 1 \(Singapore\) 

    ```
    wget http://logtail-release.oss-cn-hangzhou.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; sh logtail.sh install ap-southeast-1-internet
    ```

-   Asia Pacific SE 2 \(Sydney\)

    ```
    wget http://logtail-release-ap-southeast-2.oss-ap-southeast-2.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-2-internet
    ```

-   Asia Pacific SE 3 \(Kuala Lumpur\) 

    ```
    wget http://logtail-release-ap-southeast-3.oss-ap-southeast-3.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-3-internet
    ```

-   Asia Pacific SE 5 \(Jakarta\)

    ```
    wget http://logtail-release-ap-southeast-5.oss-ap-southeast-5.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-5-internet
    ```

-   Asia Pacific NE 1 \(Japan\) 

    ```
    wget http://logtail-release-ap-northeast-1.oss-ap-northeast-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-northeast-1-internet
    ```

-   EU Central 1 \(Frankfurt\) 

    ```
    wget http://logtail-release-eu-central-1.oss-eu-central-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install eu-central-1-internet
    ```

-   Middle East 1 \(Dubai\) 

    ```
    wget http://logtail-release-me-east-1.oss-me-east-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install me-east-1-internet
    ```

-   South Asia Pacific 1 \(Mumbai\)

    ```
    wget http://logtail-release-ap-south-1.oss-ap-south-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-south-1-internet
    ```


## View Logtail version {#section_t5v_xyq_zdb .section}

Logtail records its version information in the `logtail_version` field of the `/usr/local/ilogtail/app_info.json` file. For example:

```
$cat /usr/local/ilogtail/app_info.json
{
   "UUID" : "0DF18E97-0F2D-486F-B77F-*********",
   "hostname" : "david*******",
   "instance_id" : "F4FAFADA-F1D7-11E7-846C-00163E30349E_*********_1515129548",
   "ip" : "**********",
   "logtail_version" : "0.16.0",
   "os" : "Linux; 2.6.32-220.23.2.ali1113.el5.x86_64; #1 SMP Thu Jul 4 20:09:15 CST 2013; x86_64",
   "update_time" : "2018-01-05 13:19:08"
}
```

## Update Logtail {#section_jcz_xmv_vdb .section}

The procedure of updating Logtail is the same as that of installing Logtail. When you update the Logtail, the Logtail is automatically uninstalled first and then the latest version of Logtail is installed.

## Manually start and stop Logtail {#section_fvl_zmv_vdb .section}

-   Start Logtail

    Run the following command as an administrator: 

    ```
    /etc/init.d/ilogtaild start
    ```

-   Stop Logtail

    Run the following command as an administrator:

    ```
    /etc/init.d/ilogtaild stop.
    ```


## Uninstall Logtail {#section_c1m_1nv_vdb .section}

Download the installer **logtail.sh**. For more information, see [Install Logtail ](#section_h2x_fmv_vdb). Run the following command as an administrator in shell mode:

```
wget http://logtail-release-cn-hangzhou.oss-cn-hangzhou.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh uninstall
```

