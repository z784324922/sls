# Install Logtail in Linux {#concept_u5y_3lv_vdb .concept}

This topic describes how to install the Logtail client on Linux servers.

## Supported systems {#section_tjc_wlv_vdb .section}

The following Linux x86-64 systems are supported:

-   Aliyun Linux
-   Ubuntu
-   Debian
-   CentOS
-   OpenSUSE
-   Red Hat

## Prerequisites {#section_t52_5zm_1fb .section}

-   Your account has at least one server under it.
-   You have determined the required network type for log collection according to the server type and region to which the server belongs. For more information, see [Select a network type](reseller.en-US/User Guide/Logtail collection/Select a network type.md).

    ![](images/38676_en-US.png "Select a network type")


## Precautions {#section_wgl_xvn_1fb .section}

-   If Logtail is already installed, the Logtail installer will uninstall your current version of Logtail, delete the `/usr/local/ilogtail` directory, and reinstall Logtail. Then, Logtail is started and registered by default.
-   `${your_region_name}` is an [installation parameter](#) used for Docker and Kubernetes installation, and you can copy it from the installation parameter table.
-   If the installation fails, [open a ticket](https://selfservice.console.aliyun.com/ticket/category/sls/today) for technical support.

## Installation methods {#section_h2x_fmv_vdb .section}

Choose one of the following installation methods according to the [network type](reseller.en-US/User Guide/Logtail collection/Select a network type.md) you selected.

-   [Install Logtail through the Alibaba Cloud intranet](#).
-   [Install Logtail through the Internet](#).
-   [Install Logtail with global acceleration enabled](#).

Before running the installation command, you need to replace the value of $\{your\_region\_name\} with the actual installation parameter required. You can copy the required installation parameter based on the corresponding region from the following table.

|Region|Installation parameter|
|:-----|:---------------------|
|**China \(Hangzhou\)**|cn-hangzhou|
|**China \(Shanghai\)**|cn-shanghai|
|**China \(Qingdao\)**|cn-qingdao|
|**China \(Beijing\)**|cn-beijing|
|**China \(Zhangjiakou\)**|cn-zhangjiakou|
|**China \(Hohhot\)**|cn-huhehaote|
|**China \(Shenzhen\)**|cn-shenzhen|
|**China \(Chengdu\)**|cn-chengdu|
|**Hong Kong**|cn-hongkong|
|**US \(Silicon Valley\)**|us-west-1|
|**US \(Virginia\)**|us-east-1|
|**Singapore**|ap-southeast-1|
|**Australia \(Sydney\)**|ap-southeast-2|
|**Malaysia \(Kuala Lumpur\)**|ap-southeast-3|
|**Indonesia \(Jakarta\)**|ap-southeast-5|
|**India \(Mumbai\)**|ap-south-1|
|**Japan \(Tokyo\)**|ap-northeast-1|
|**Germany \(Frankfurt\)**|eu-central-1|
|**UAE \(Dubai\)**|me-east-1|
|**UK \(London\)**|eu-west-1|

## Install Logtail through the Alibaba Cloud intranet {#section_inb_fbn_1fb .section}

The Alibaba Cloud intranet is a GiB-level shared network, which provides a faster and more stable data transfer than the Internet and does not consume Internet bandwidth.

The Alibaba Cloud intranet applies to Alibaba Cloud ECS servers that are deployed in the region to which the Log Service project belongs.

You can use either of the following methods to install Logtail:

-   **Enable automatic installation parameter selection.**

    If you are unsure about the region of the ECS server, you can specify the auto parameter of the Logtail installer. Then, the Logtail installer will obtain the [metadata](../../../../../reseller.en-US/User Guide/Instances/User-defined data and metadata/Metadata.md) from the server to automatically locate the region of the ECS server.

    To enable automatic installation parameter selection, follow these steps:

    1.  Download the Logtail installer through the Internet by running the following command:

        **Note:** This operation requires access to the Internet and consumes about 10 KB of Internet traffic.

        ```
        wget http://logtail-release-cn-hangzhou.oss-cn-hangzhou.aliyuncs.com/linux64/logtail.sh -O logtail.sh;chmod 755 logtail.sh
        ```

    2.  Use the auto parameter by running the following command:

        **Note:** This operation does not consume Internet traffic. The installation program will be automatically downloaded.

        ```
        ./logtail.sh install auto
        ```

-   **Manually install Logtail.**

    You can manually install the Logtail installer through the intranet, which does not consume Internet traffic.

    To install Logtail, choose the required installation parameter and run the installation command. Specifically, you need to choose the correct parameter from the [installation parameter](#) table, replace `${your_region_name}` with the required parameter, and run the following installation command:

    **Note:** In the installation command, `${your_region_name}` indicates the region to which the Log Service project belongs. For example, the installation parameter for the China \(Hangzhou\) region is `cn-hangzhou`.

    ```
    wget http://logtail-release-$\{your\_region\_name\}.oss-$\{your\_region\_name\}-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install $\{your\_region\_name\}
    ```

    **Alternatively, after confirming the region to which the Log Service project belongs, you can run the corresponding installation command listed in the following table to install Logtail:**

    |Region|Installation command|
    |:-----|:-------------------|
    |**China \(Hangzhou\)**|     ```
wget http://logtail-release-cn-hangzhou.oss-cn-hangzhou-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-hangzhou
    ```

 |
    |**China \(Shanghai\)**|     ```
wget http://logtail-release-cn-shanghai.oss-cn-shanghai-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-shanghai
    ```

 |
    |**China \(Qingdao\)**|     ```
wget http://logtail-release-cn-qingdao.oss-cn-qingdao-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-qingdao
    ```

 |
    |**China \(Beijing\)**|     ```
wget http://logtail-release-cn-beijing.oss-cn-beijing-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-beijing
    ```

 |
    |**China \(Zhangjiakou\)**|     ```
wget http://logtail-release-cn-zhangjiakou.oss-cn-zhangjiakou-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-zhangjiakou
    ```

 |
    |**China \(Hohhot\)**|     ```
wget http://logtail-release-cn-huhehaote.oss-cn-huhehaote-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-huhehaote
    ```

 |
    |**China \(Shenzhen\)**|     ```
wget http://logtail-release-cn-shenzhen.oss-cn-shenzhen-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-shenzhen
    ```

 |
    |**China \(Chengdu\)**|     ```
wget http://logtail-release-cn-chengdu.oss-cn-chengdu-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-chengdu
    ```

 |
    |**Hong Kong**|     ```
wget http://logtail-release-cn-hongkong.oss-cn-hongkong-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-hongkong
    ```

 |
    |**US \(Silicon Valley\)**|     ```
wget http://logtail-release-us-west-1.oss-us-west-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install us-west-1
    ```

 |
    |**US \(Virginia\)**|     ```
wget http://logtail-release-us-east-1.oss-us-east-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install us-east-1
    ```

 |
    |**Singapore**|     ```
wget http://logtail-release-ap-southeast-1.oss-ap-southeast-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-1
    ```

 |
    |**Australia \(Sydney\)**|     ```
wget http://logtail-release-ap-southeast-2.oss-ap-southeast-2-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-2
    ```

 |
    |**Malaysia \(Kuala Lumpur\)**|     ```
wget http://logtail-release-ap-southeast-3.oss-ap-southeast-3-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-3
    ```

 |
    |**Indonesia \(Jakarta\)**|     ```
wget http://logtail-release-ap-southeast-5.oss-ap-southeast-5-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-5
    ```

 |
    |**Japan \(Tokyo\)**|     ```
wget http://logtail-release-ap-northeast-1.oss-ap-northeast-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-northeast-1
    ```

 |
    |**India \(Mumbai\)**|     ```
wget http://logtail-release-ap-south-1.oss-ap-south-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-south-1
    ```

 |
    |**Germany \(Frankfurt\)**|     ```
wget http://logtail-release-eu-central-1.oss-eu-central-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install eu-central-1
    ```

 |
    |**UAE \(Dubai\)**|     ```
wget http://logtail-release-me-east-1.oss-me-east-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install me-east-1
    ```

 |
    |**UK \(London\)**|     ```
wget http://logtail-release-eu-west-1.oss-eu-west-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install eu-west-1
    ```

 |


## Install Logtail through the Internet {#section_lhn_hbn_1fb .section}

Data is written into Log Service through the Internet, which consumes Internet bandwidth. **This method applies to on-premises IDCs and servers provided by other cloud product vendors.**

**Note:** Log Service cannot obtain owner information about non-ECS servers, so you must manually configure AliUids after installing Logtail. Otherwise, Logtail heartbeats become abnormal or Logtail cannot collect logs. For details about how to configure AliUids, see [Configure AliUids for ECS servers under other Alibaba Cloud accounts and on-premises IDCs](reseller.en-US/User Guide/Logtail collection/Machine Group/Collect logs from non-Alibaba Cloud ECS instances or ECS instances not in your account.md).

To install Logtail, choose the required installation parameter and run the installation command. Specifically, you need to choose the correct parameter from the [installation parameter](#) table, replace `${your_region_name}` with the required parameter, and run the following installation command:

**Note:** In the installation command, `${your_region_name}` indicates the region to which the Log Service project belongs. For example, the installation parameter for the China \(Hangzhou\) region is `cn-hangzhou`.

```
wget http://logtail-release-$\{your\_region\_name\}.oss-$\{your\_region\_name\}.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install $\{your\_region\_name\}-internet
```

**Alternatively, after confirming the region to which the Log Service project belongs, you can run the corresponding installation command listed in the following table to install Logtail:**

|Region|Installation command|
|:-----|:-------------------|
|**China \(Hangzhou\)**| ```
wget http://logtail-release-cn-hangzhou.oss-cn-hangzhou.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-hangzhou-internet
```

 |
|**China \(Shanghai\)**| ```
wget http://logtail-release-cn-shanghai.oss-cn-shanghai.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-shanghai-internet
```

 |
|**China \(Qingdao\)**| ```
wget http://logtail-release-cn-qingdao.oss-cn-qingdao.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-qingdao-internet
```

 |
|**China \(Beijing\)**| ```
wget http://logtail-release-cn-beijing.oss-cn-beijing.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-beijing-internet
```

 |
|**China \(Zhangjiakou\)**| ```
wget http://logtail-release-cn-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-zhangjiakou-internet
```

 |
|**China \(Hohhot\)**| ```
wget http://logtail-release-cn-huhehaote.oss-cn-huhehaote.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-huhehaote-internet
```

 |
|**China \(Shenzhen\)**| ```
wget http://logtail-release-cn-shenzhen.oss-cn-shenzhen.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-shenzhen-internet
```

 |
|**China \(Chengdu\)**| ```
wget http://logtail-release-cn-chengdu.oss-cn-chengdu.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-chengdu-internet
```

 |
|**Hong Kong**| ```
wget http://logtail-release-cn-hongkong.oss-cn-hongkong.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-hongkong-internet
```

 |
|**US \(Silicon Valley\)**| ```
wget http://logtail-release-us-west-1.oss-us-west-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install us-west-1-internet
```

 |
|**US \(Virginia\)**| ```
wget http://logtail-release-us-east-1.oss-us-east-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install us-east-1-internet
```

 |
|**Singapore**| ```
wget http://logtail-release.oss-cn-hangzhou.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; sh logtail.sh install ap-southeast-1-internet
```

 |
|**Australia \(Sydney\)**| ```
wget http://logtail-release-ap-southeast-2.oss-ap-southeast-2.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-2-internet
```

 |
|**Malaysia \(Kuala Lumpur\)**| ```
wget http://logtail-release-ap-southeast-3.oss-ap-southeast-3.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-3-internet
```

 |
|**Indonesia \(Jakarta\)**| ```
wget http://logtail-release-ap-southeast-5.oss-ap-southeast-5.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-5-internet
```

 |
|**Japan \(Tokyo\)**| ```
wget http://logtail-release-ap-northeast-1.oss-ap-northeast-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-northeast-1-internet
```

 |
|**Germany \(Frankfurt\)**| ```
wget http://logtail-release-eu-central-1.oss-eu-central-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install eu-central-1-internet
```

 |
|**UAE \(Dubai\)**| ```
wget http://logtail-release-me-east-1.oss-me-east-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install me-east-1-internet
```

 |
|**India \(Mumbai\)**| ```
wget http://logtail-release-ap-south-1.oss-ap-south-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-south-1-internet
```

 |
|**UK \(London\)**| ```
wget http://logtail-release-eu-west-1.oss-eu-west-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install eu-west-1-internet
```

 |

## Install Logtail with global acceleration enabled {#section_xpy_tvs_s2b .section}

If your server is deployed in an overseas on-premises IDC or provided by another cloud product vendor, data transfer through the Internet may be unstable or delayed. In this case, you can enable the [global acceleration](reseller.en-US/User Guide/Data Collection/Collection acceleration/Overview.md) function. Global acceleration accelerates log collection with the help of Alibaba Cloud CND edge nodes and provides greater advantages in terms of data transfer effectiveness and network stability for log collection through the Internet.

To install Logtail, choose the required installation parameter and run the installation command. Specifically, you need to choose the correct parameter from the [installation parameter](#) table, replace `${your_region_name}` with the required parameter, and run the following installation command:

**Note:** In the installation command, `${your_region_name}` indicates the region to which the Log Service project belongs. For example, the installation parameter for the China \(Hangzhou\) region is `cn-hangzhou`.

```
wget http://logtail-release-$\{your\_region\_name\}.oss-$\{your\_region\_name\}.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install $\{your\_region\_name\}-acceleration
```

**Alternatively, after confirming the region to which the Log Service project belongs, you can run the corresponding installation command listed in the following table to install Logtail:**

|Region|Installation command|
|:-----|:-------------------|
|**China \(Beijing\)**| ```
wget http://logtail-release-cn-beijing.oss-cn-beijing.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-beijing-acceleration
```

 |
|**China \(Qingdao\)**| ```
wget http://logtail-release-cn-qingdao.oss-cn-qingdao.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-qingdao-acceleration
```

 |
|**China \(Hangzhou\)**| ```
wget http://logtail-release-cn-hangzhou.oss-cn-hangzhou.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-hangzhou-acceleration
```

 |
|**China \(Shanghai\)**| ```
wget http://logtail-release-cn-shanghai.oss-cn-shanghai.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-shanghai-acceleration
```

 |
|**China \(Shenzhen\)**| ```
wget http://logtail-release-cn-shenzhen.oss-cn-shenzhen.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-shenzhen-acceleration
```

 |
|**China \(Zhangjiakou\)**| ```
wget http://logtail-release-cn-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-zhangjiakou-acceleration
```

 |
|**China \(Hohhot\)**| ```
wget http://logtail-release-cn-huhehaote.oss-cn-huhehaote.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-huhehaote-acceleration
```

 |
|**China \(Chengdu\)**| ```
wget http://logtail-release-cn-chengdu.oss-cn-chengdu.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-chengdu-acceleration
```

 |
|**Hong Kong**| ```
wget http://logtail-release-cn-hongkong.oss-cn-hongkong.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-hongkong-acceleration
```

 |
|**US \(Silicon Valley\)**| ```
wget http://logtail-release-us-west-1.oss-us-west-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install us-west-1-acceleration
```

 |
|**US \(Virginia\)**| ```
wget http://logtail-release-us-east-1.oss-us-east-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install us-east-1-acceleration
```

 |
|**Singapore**| ```
wget http://logtail-release-ap-southeast-1.oss-ap-southeast-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-1-acceleration
```

 |
|**Australia \(Sydney\)**| ```
wget http://logtail-release-ap-southeast-2.oss-ap-southeast-2.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-2-acceleration
```

 |
|**Malaysia \(Kuala Lumpur\)**| ```
wget http://logtail-release-ap-southeast-3.oss-ap-southeast-3.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-3-acceleration
```

 |
|**Indonesia \(Jakarta\)**| ```
wget http://logtail-release-ap-southeast-5.oss-ap-southeast-5.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-5-acceleration
```

 |
|**Japan \(Tokyo\)**| ```
wget http://logtail-release-ap-northeast-1.oss-ap-northeast-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-northeast-1-acceleration
```

 |
|**Germany \(Frankfurt\)**| ```
wget http://logtail-release-eu-central-1.oss-eu-central-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install eu-central-1-acceleration
```

 |
|**UAE \(Dubai\)**| ```
wget http://logtail-release-me-east-1.oss-me-east-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install me-east-1-acceleration
```

 |
|**India \(Mumbai\)**| ```
wget http://logtail-release-ap-south-1.oss-ap-south-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-south-1-acceleration
```

 |
|**UK \(London\)**| ```
wget http://logtail-release-eu-west-1.oss-eu-west-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install eu-west-1-acceleration
```

 |

## View the Logtail version {#section_t5v_xyq_zdb .section}

Logtail records version information in the `logtail_version` field in the `/usr/local/ilogtail/app_info.json` file. For example:

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

## Upgrade Logtail {#section_jcz_xmv_vdb .section}

You can use the Logtail installer \(logtail.sh\) to upgrade Logtail. The installer automatically chooses an appropriate upgrade method according to the Logtail settings.

**Note:** During the upgrade, Logtail is stopped, and only necessary files are overwritten. The configuration file, the Checkpoint file, and all logs are retained.

Upgrade Logtail by running the following command:

```
# Download the Logtail installer.
wget http://logtail-release-cn-hangzhou.oss-cn-hangzhou.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh
# Run the upgrade command.
sudo ./logtail.sh upgrade
```

Response:

```
# The upgrade succeeded.
Stop logtail successfully.
ilogtail is running
Upgrade logtail success
{
   "UUID" : "***",
   "hostname" : "***",
   "instance_id" : "***",
   "ip" : "***",
   "logtail_version" : "0.16.11",
   "os" : "Linux; 3.10.0-693.2.2.el7.x86_64; #1 SMP Tue Sep 12 22:26:13 UTC 2017; x86_64",
   "update_time" : "2018-08-29 15:01:36"
}

# The upgrade failed: The current version is the latest version.
[Error]:    Already up to date.
```

## Manually start and stop Logtail {#section_fvl_zmv_vdb .section}

-   Start Logtail as the admin user by running the following command:

    ```
    /etc/init.d/ilogtaild start
    ```

-   Stop Logtail as the admin user by running the following command:

    ```
    /etc/init.d/ilogtaild stop
    ```


## Uninstall Logtail {#section_c1m_1nv_vdb .section}

Download the Logtail installer **logtail.sh**, and then uninstall Logtail by running the following command:

```
wget http://logtail-release-cn-hangzhou.oss-cn-hangzhou.aliyuncs.com/linux64/logtail.sh -O logtail.sh
chmod 755 logtail.sh; ./logtail.sh uninstall
```

