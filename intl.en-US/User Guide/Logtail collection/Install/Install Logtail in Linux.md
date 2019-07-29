# Install Logtail in Linux {#concept_u5y_3lv_vdb .concept}

The Logtail client is a log collection agent provided by Log Service. This topic describes how to install the Logtail client on a Linux server.

## Supported systems {#section_tjc_wlv_vdb .section}

The Logtail client for Linux supports the following x86-64 \(64-bit\) Linux systems:

-   Aliyun Linux
-   Ubuntu
-   Debian
-   CentOS
-   OpenSUSE
-   Red Hat

## Prerequisites {#section_t52_5zm_1fb .section}

1.  One or more servers are available.
2.  The network type for log collection is determined based on the type and region of the server. For more information, see [Select a network type](reseller.en-US/User Guide/Logtail collection/Select a network type.md).

    ![](images/12057_en-US.png "Select a network type")


## Precautions {#section_wgl_xvn_1fb .section}

-   Logtail is installed in overwrite mode. If you have installed Logtail before, the installer will uninstall your current version of Logtail, delete the `/usr/local/ilogtail` directory, and reinstall Logtail. By default, Logtail is started after the installation and at startup.

-   The `${your_region_name}` parameter is one of the installation parameters used for the installation of Docker and Kubernetes. Copy the value of the parameter from the [region name table](#).

-   If the installation fails, click [here](https://selfservice.console.aliyun.com/ticket/category/sls/today) to open a ticket.


## Select an installation method {#section_h2x_fmv_vdb .section}

Select one of the following installation methods according to the [network type](reseller.en-US/User Guide/Logtail collection/Select a network type.md) you selected.

-   [Install Logtail through the Alibaba Cloud internal network](#)
-   [Install Logtail through the Internet](#)
-   [Install Logtail with Global Acceleration enabled](#)

Before running the installation command, replace $\{your\_region\_name\} with the actual region name. The following table lists the names of different regions. You can also copy and run the installation commands for the corresponding region and network type.

|Region|Region name|Region|Region name|
|:-----|:----------|:-----|:----------|
|**China \(Hangzhou\)**|cn-hangzhou|**Australia \(Sydney\)**|ap-southeast-2|
|**China \(Shanghai\)**|cn-shanghai|**Malaysia \(Kuala Lumpur\)**|ap-southeast-3|
|**China \(Qingdao\)**|cn-qingdao|**Indonesia \(Jakarta\)**|ap-southeast-5|
|**China \(Beijing\)**|cn-beijing|**India \(Mumbai\)**|ap-south-1|
|**China \(Zhangjiakou\)**|cn-zhangjiakou|**Japan \(Tokyo\)**|ap-northeast-1|
|**China \(Hohhot\)**|cn-huhehaote|**Germany \(Frankfurt\)**|eu-central-1|
|**China \(Shenzhen\)**|cn-shenzhen|**UAE \(Dubai\)**|me-east-1|
|**China \(Chengdu\)**|cn-chengdu|**UK \(London\)**|eu-west-1|
|**Hong Kong**|cn-hongkong|****|****|
|**US \(Silicon Valley\)**|us-west-1|****|****|
|**US \(Virginia\)**|us-east-1|****|****|
|**Singapore**|ap-southeast-1|-|-|

## Install Logtail through the Alibaba Cloud internal network {#section_inb_fbn_1fb .section}

The Alibaba Cloud internal network is a shared gigabit network, which provides faster and more stable data transfer than the Internet and does not consume Internet bandwidth.

You can install Logtail through the Alibaba Cloud internal network when the following conditions are met:

-   Alibaba Cloud ECS instances are deployed.
-   The ECS instances and the Log Service project are located in the same region.

When running the installation command, you need to specify the region. You can **use the auto parameter** or **manually specify the region**.

-   **Use the auto parameter** 

    If you are not sure about the region of the ECS instance, you can use the auto parameter of the installer to install Logtail. The Logtail installer obtains the [metadata](../../../../reseller.en-US/Instances/Manage instances/User-defined data and metadata/Metadata.md) from the server and automatically determines the region of the ECS instance.

    1.  Download the Logtail installer through the Internet. This operation requires access to the Internet and consumes about 10 KB of Internet traffic.

        ``` {#codeblock_gks_4wn_9uh}
        wget http://logtail-release-cn-hangzhou.oss-cn-hangzhou.aliyuncs.com/linux64/logtail.sh -O logtail.sh;chmod 755 logtail.sh
        ```

    2.  Use the auto parameter for installation. This operation does not consume Internet traffic. The installation program of the corresponding region will be automatically downloaded.

        ``` {#codeblock_gyr_3ny_8io}
        ./logtail.sh install auto
        ```

-   **Manually specify the region** 

    You can also manually install Logtail. Downloading the Logtail installer through the internal network does not consume Internet traffic.

    1.  **Obtain the name of the region where the Log Service project is located.** 

        In the installation command, `${your_region_name}` indicates the name of the region where the Log Service project is located. Select the region name according to the [region name table](#). For example, the name of the China \(Hangzhou\) region is `cn-hangzhou`.

    2.  **Run the installation command after replacing $\{your\_region\_name\} with the actual region name.** 

        Replace `${your_region_name}` with the actual region name, and then run the installation command.

        ``` {#codeblock_0mj_tma_uzo}
        wget http://logtail-release-$\{your\_region\_name\}.oss-$\{your\_region\_name\}-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install $\{your\_region\_name\}
        ```

        **The following table lists the installation commands for different regions. You can also install Logtail by running the command corresponding to the region where your Log Service project is located.** 

        |Region|Installation command|
        |:-----|:-------------------|
        |**China \(Hangzhou\)**|         ``` {#codeblock_4i6_7sf_tad}
wget http://logtail-release-cn-hangzhou.oss-cn-hangzhou-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-hangzhou
        ```

 |
        |**China \(Shanghai\)**|         ``` {#codeblock_7yv_a7g_go2}
wget http://logtail-release-cn-shanghai.oss-cn-shanghai-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-shanghai
        ```

 |
        |**China \(Qingdao\)**|         ``` {#codeblock_93f_8rz_v5i}
wget http://logtail-release-cn-qingdao.oss-cn-qingdao-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-qingdao
        ```

 |
        |**China \(Beijing\)**|         ``` {#codeblock_eu4_g11_up0}
wget http://logtail-release-cn-beijing.oss-cn-beijing-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-beijing
        ```

 |
        |**China \(Zhangjiakou\)**|         ``` {#codeblock_s3n_lwm_nj3}
wget http://logtail-release-cn-zhangjiakou.oss-cn-zhangjiakou-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-zhangjiakou
        ```

 |
        |**China \(Hohhot\)**|         ``` {#codeblock_6ei_tzc_xq9}
wget http://logtail-release-cn-huhehaote.oss-cn-huhehaote-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-huhehaote
        ```

 |
        |**China \(Shenzhen\)**|         ``` {#codeblock_hta_k84_icb}
wget http://logtail-release-cn-shenzhen.oss-cn-shenzhen-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-shenzhen
        ```

 |
        |**China \(Chengdu\)**|         ``` {#codeblock_bzw_ra6_5io}
wget http://logtail-release-cn-chengdu.oss-cn-chengdu-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-chengdu
        ```

 |
        |**Hong Kong**|         ``` {#codeblock_fya_8b2_14m}
wget http://logtail-release-cn-hongkong.oss-cn-hongkong-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-hongkong
        ```

 |
        |**US \(Silicon Valley\)**|         ``` {#codeblock_p33_71k_y3j}
wget http://logtail-release-us-west-1.oss-us-west-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install us-west-1
        ```

 |
        |**US \(Virginia\)**|         ``` {#codeblock_to1_ksq_xdp}
wget http://logtail-release-us-east-1.oss-us-east-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install us-east-1
        ```

 |
        |**Singapore**|         ``` {#codeblock_asc_m7n_hzz}
wget http://logtail-release-ap-southeast-1.oss-ap-southeast-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-1
        ```

 |
        |**Australia \(Sydney\)**|         ``` {#codeblock_6fe_c32_xez}
wget http://logtail-release-ap-southeast-2.oss-ap-southeast-2-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-2
        ```

 |
        |**Malaysia \(Kuala Lumpur\)**|         ``` {#codeblock_m7l_ys0_r4a}
wget http://logtail-release-ap-southeast-3.oss-ap-southeast-3-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-3
        ```

 |
        |**Indonesia \(Jakarta\)**|         ``` {#codeblock_8uv_u4o_yr8}
wget http://logtail-release-ap-southeast-5.oss-ap-southeast-5-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-5
        ```

 |
        |**Japan \(Tokyo\)**|         ``` {#codeblock_42o_wkr_txf}
wget http://logtail-release-ap-northeast-1.oss-ap-northeast-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-northeast-1
        ```

 |
        |**India \(Mumbai\)**|         ``` {#codeblock_tr9_d8f_02m}
wget http://logtail-release-ap-south-1.oss-ap-south-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-south-1
        ```

 |
        |**Germany \(Frankfurt\)**|         ``` {#codeblock_w3x_44t_c2e}
wget http://logtail-release-eu-central-1.oss-eu-central-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install eu-central-1
        ```

 |
        |**UAE \(Dubai\)**|         ``` {#codeblock_wyq_7j3_1om}
wget http://logtail-release-me-east-1.oss-me-east-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install me-east-1
        ```

 |
        |**UK \(London\)**|         ``` {#codeblock_qa1_121_2s1}
wget http://logtail-release-eu-west-1.oss-eu-west-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install eu-west-1
        ```

 |


## Install Logtail through the Internet {#section_lhn_hbn_1fb .section}

Data is written to Log Service through the Internet, which consumes Internet bandwidth. **You can use this method to install Logtail on a server deployed in an on-premises IDC or provided by another cloud service vendor.** 

**Note:** Log Service cannot obtain the owner information about other types of servers. In this case, you must manually configure AliUids after installing Logtail. Otherwise, Logtail has abnormal heartbeats and cannot collect logs. For more information about AliUids, see [Configure AliUids for ECS servers under other Alibaba Cloud accounts or on-premises IDCs](reseller.en-US/User Guide/Logtail collection/Machine Group/Configure AliUids for ECS servers under other Alibaba Cloud accounts or on-premises IDCs.md).

1.  **Obtain the name of the region where the Log Service project is located.** 

    In the installation command, `${your_region_name}` indicates the name of the region where the Log Service project is located. Select the region name according to the [region name table](#). For example, the name of the China \(Hangzhou\) region is `cn-hangzhou`.

2.  **Run the installation command after replacing $\{your\_region\_name\} with the actual region name.** 

    Replace `${your_region_name}` with the actual region name, and then run the installation command.

    ``` {#codeblock_21q_viq_37c}
    wget http://logtail-release-$\{your\_region\_name\}.oss-$\{your\_region\_name\}.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install $\{your\_region\_name\}-internet
    ```

    **The following table lists the installation commands for different regions. You can also install Logtail by running the command corresponding to the region where your Log Service project is located.** 

    |Region|Installation command|
    |:-----|:-------------------|
    |**China \(Hangzhou\)**|     ``` {#codeblock_m2h_3in_o8t}
wget http://logtail-release-cn-hangzhou.oss-cn-hangzhou.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-hangzhou-internet
    ```

 |
    |**China \(Shanghai\)**|     ``` {#codeblock_euf_59p_pk6}
wget http://logtail-release-cn-shanghai.oss-cn-shanghai.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-shanghai-internet
    ```

 |
    |**China \(Qingdao\)**|     ``` {#codeblock_xoo_f52_t7c}
wget http://logtail-release-cn-qingdao.oss-cn-qingdao.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-qingdao-internet
    ```

 |
    |**China \(Beijing\)**|     ``` {#codeblock_cyp_qwt_ygn}
wget http://logtail-release-cn-beijing.oss-cn-beijing.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-beijing-internet
    ```

 |
    |**China \(Zhangjiakou\)**|     ``` {#codeblock_rqu_nxp_2x0}
wget http://logtail-release-cn-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-zhangjiakou-internet
    ```

 |
    |**China \(Hohhot\)**|     ``` {#codeblock_t9o_alj_zyt}
wget http://logtail-release-cn-huhehaote.oss-cn-huhehaote.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-huhehaote-internet
    ```

 |
    |**China \(Shenzhen\)**|     ``` {#codeblock_s1r_ekl_hpt}
wget http://logtail-release-cn-shenzhen.oss-cn-shenzhen.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-shenzhen-internet
    ```

 |
    |**China \(Chengdu\)**|     ``` {#codeblock_owm_tag_sgq}
wget http://logtail-release-cn-chengdu.oss-cn-chengdu.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-chengdu-internet
    ```

 |
    |**Hong Kong**|     ``` {#codeblock_rp0_xkp_n7r}
wget http://logtail-release-cn-hongkong.oss-cn-hongkong.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-hongkong-internet
    ```

 |
    |**US \(Silicon Valley\)**|     ``` {#codeblock_9mh_drg_g7g}
wget http://logtail-release-us-west-1.oss-us-west-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install us-west-1-internet
    ```

 |
    |**US \(Virginia\)**|     ``` {#codeblock_hqu_n2h_m7m}
wget http://logtail-release-us-east-1.oss-us-east-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install us-east-1-internet
    ```

 |
    |**Singapore**|     ``` {#codeblock_ctf_ddr_zb0}
wget http://logtail-release.oss-cn-hangzhou.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; sh logtail.sh install ap-southeast-1-internet
    ```

 |
    |**Australia \(Sydney\)**|     ``` {#codeblock_dzx_9v6_br5}
wget http://logtail-release-ap-southeast-2.oss-ap-southeast-2.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-2-internet
    ```

 |
    |**Malaysia \(Kuala Lumpur\)**|     ``` {#codeblock_90v_6gg_snn}
wget http://logtail-release-ap-southeast-3.oss-ap-southeast-3.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-3-internet
    ```

 |
    |**Indonesia \(Jakarta\)**|     ``` {#codeblock_6hd_s82_u7q}
wget http://logtail-release-ap-southeast-5.oss-ap-southeast-5.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-5-internet
    ```

 |
    |**Japan \(Tokyo\)**|     ``` {#codeblock_vkq_uz1_3vk}
wget http://logtail-release-ap-northeast-1.oss-ap-northeast-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-northeast-1-internet
    ```

 |
    |**Germany \(Frankfurt\)**|     ``` {#codeblock_djo_2lu_vpg}
wget http://logtail-release-eu-central-1.oss-eu-central-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install eu-central-1-internet
    ```

 |
    |**UAE \(Dubai\)**|     ``` {#codeblock_pxl_au0_rwl}
wget http://logtail-release-me-east-1.oss-me-east-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install me-east-1-internet
    ```

 |
    |**India \(Mumbai\)**|     ``` {#codeblock_dq9_iy1_25j}
wget http://logtail-release-ap-south-1.oss-ap-south-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-south-1-internet
    ```

 |
    |**UK \(London\)**|     ``` {#codeblock_rbq_osw_rks}
wget http://logtail-release-eu-west-1.oss-eu-west-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install eu-west-1-internet
    ```

 |


## Install Logtail with Global Acceleration enabled {#section_xpy_tvs_s2b .section}

If your servers are deployed in on-premises IDCs outside Mainland China or provided by cloud service vendors outside Mainland China, using the Internet to transmit data may cause problems such as high latency and unstable transmission. In this case, you can enable [Global Acceleration](reseller.en-US/User Guide/Data Collection/Collection acceleration/Overview.md). [Global Acceleration](reseller.en-US/User Guide/Data Collection/Collection acceleration/Overview.md) accelerates log collection by using the edge nodes of Alibaba Cloud CDN. Compared with data transmission through the Internet, Global Acceleration offers a more stable network with minimal transmission latency.

1.  **Obtain the name of the region where the Log Service project is located.** 

    In the installation command, $\{your\_region\_name\} indicates the name of the region where the Log Service project is located. Select the region name according to the [region name table](#). For example, the name of the China \(Hangzhou\) region is `cn-hangzhou`.

2.  **Run the installation command after replacing $\{your\_region\_name\} with the actual region name.** 

    Replace `${your_region_name}` with the actual region name, and then run the installation command.

    ``` {#codeblock_417_tqa_bsz}
    wget http://logtail-release-$\{your\_region\_name\}.oss-$\{your\_region\_name\}.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install $\{your\_region\_name\}-acceleration
    ```

    **The following table lists the installation commands for different regions. You can also install Logtail by running the command corresponding to the region where your Log Service project is located.** 

    | | |
    |:-|:-|
    |**China \(Beijing\)**|     ``` {#codeblock_sea_quv_poy}
wget http://logtail-release-cn-beijing.oss-cn-beijing.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-beijing-acceleration
    ```

 |
    |**China \(Qingdao\)**|     ``` {#codeblock_jai_gnz_x62}
wget http://logtail-release-cn-qingdao.oss-cn-qingdao.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-qingdao-acceleration
    ```

 |
    |**China \(Hangzhou\)**|     ``` {#codeblock_e4e_hh1_2er}
wget http://logtail-release-cn-hangzhou.oss-cn-hangzhou.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-hangzhou-acceleration
    ```

 |
    |**China \(Shanghai\)**|     ``` {#codeblock_clf_m0d_cnf}
wget http://logtail-release-cn-shanghai.oss-cn-shanghai.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-shanghai-acceleration
    ```

 |
    |**China \(Shenzhen\)**|     ``` {#codeblock_3se_cmw_5jm}
wget http://logtail-release-cn-shenzhen.oss-cn-shenzhen.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-shenzhen-acceleration
    ```

 |
    |**China \(Zhangjiakou\)**|     ``` {#codeblock_2ng_xax_jlw}
wget http://logtail-release-cn-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-zhangjiakou-acceleration
    ```

 |
    |**China \(Hohhot\)**|     ``` {#codeblock_vkp_hlp_3zy}
wget http://logtail-release-cn-huhehaote.oss-cn-huhehaote.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-huhehaote-acceleration
    ```

 |
    |**China \(Chengdu\)**|     ``` {#codeblock_17g_64t_ytd}
wget http://logtail-release-cn-chengdu.oss-cn-chengdu.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-chengdu-acceleration
    ```

 |
    |**Hong Kong**|     ``` {#codeblock_hf5_o9v_a6c}
wget http://logtail-release-cn-hongkong.oss-cn-hongkong.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-hongkong-acceleration
    ```

 |
    |**US \(Silicon Valley\)**|     ``` {#codeblock_mea_0on_h90}
wget http://logtail-release-us-west-1.oss-us-west-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install us-west-1-acceleration
    ```

 |
    |**US \(Virginia\)**|     ``` {#codeblock_asr_pj0_ae1}
wget http://logtail-release-us-east-1.oss-us-east-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install us-east-1-acceleration
    ```

 |
    |**Singapore**|     ``` {#codeblock_3cu_awx_ubb}
wget http://logtail-release-ap-southeast-1.oss-ap-southeast-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-1-acceleration
    ```

 |
    |**Australia \(Sydney\)**|     ``` {#codeblock_ydn_zeb_bbn}
wget http://logtail-release-ap-southeast-2.oss-ap-southeast-2.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-2-acceleration
    ```

 |
    |**Malaysia \(Kuala Lumpur\)**|     ``` {#codeblock_efv_e7l_ijn}
wget http://logtail-release-ap-southeast-3.oss-ap-southeast-3.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-3-acceleration
    ```

 |
    |**Indonesia \(Jakarta\)**|     ``` {#codeblock_i1y_fdy_04c}
wget http://logtail-release-ap-southeast-5.oss-ap-southeast-5.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-5-acceleration
    ```

 |
    |**Japan \(Tokyo\)**|     ``` {#codeblock_984_glw_ck2}
wget http://logtail-release-ap-northeast-1.oss-ap-northeast-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-northeast-1-acceleration
    ```

 |
    |**Germany \(Frankfurt\)**|     ``` {#codeblock_rrl_j2h_y6m}
wget http://logtail-release-eu-central-1.oss-eu-central-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install eu-central-1-acceleration
    ```

 |
    |**UAE \(Dubai\)**|     ``` {#codeblock_h2y_mhv_fyz}
wget http://logtail-release-me-east-1.oss-me-east-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install me-east-1-acceleration
    ```

 |
    |**India \(Mumbai\)**|     ``` {#codeblock_zxj_3sm_5l5}
wget http://logtail-release-ap-south-1.oss-ap-south-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-south-1-acceleration
    ```

 |
    |**UK \(London\)**|     ``` {#codeblock_b4z_3g1_1qi}
wget http://logtail-release-eu-west-1.oss-eu-west-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install eu-west-1-acceleration
    ```

 |


## View the Logtail version {#section_t5v_xyq_zdb .section}

Logtail records version information in the `logtail_version` field in the `/usr/local/ilogtail/app_info.json` file.

``` {#codeblock_lyn_o17_470}
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

You can use the Logtail installer \(logtail.sh\) to upgrade Logtail. The installer automatically selects an appropriate upgrade method based on the configuration information of the installed Logtail.

**Note:** During the upgrade, Logtail will be temporarily stopped. Only necessary files are overwritten. The configuration file, checkpoint file, and logs are retained.

Run the following commands to upgrade Logtail:

``` {#codeblock_9hs_owa_nxs}
# Download the Logtail installer.
wget http://logtail-release-cn-hangzhou.oss-cn-hangzhou.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh
# Upgrade Logtail.
sudo ./logtail.sh upgrade
```

Response:

``` {#codeblock_2ru_su2_en0}
# The upgrade is successful.
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

# The upgrade fails because the current version is the latest version.
[Error]:    Already up to date.
```

## Manually start or stop Logtail {#section_fvl_zmv_vdb .section}

-   Start Logtail

    Run the following command as an administrator to start Logtail:

    ``` {#codeblock_b61_u6o_2cq}
    /etc/init.d/ilogtaild start
    ```

-   Stop Logtail

    Run the following command as an administrator to stop Logtail:

    ``` {#codeblock_e0t_k20_f2w}
    /etc/init.d/ilogtaild stop
    ```


## Uninstall Logtail {#section_c1m_1nv_vdb .section}

Download the Logtail installer **logtail.sh**, and then run the following commands to uninstall Logtail:

``` {#codeblock_3pa_1qa_xpi}
wget http://logtail-release-cn-hangzhou.oss-cn-hangzhou.aliyuncs.com/linux64/logtail.sh -O logtail.sh
chmod 755 logtail.sh; ./logtail.sh uninstall
```

