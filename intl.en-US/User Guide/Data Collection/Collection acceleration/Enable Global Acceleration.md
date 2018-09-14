# Enable Global Acceleration {#concept_bfn_n54_p2b .concept}

To enable Global Acceleration for Log Service, see the following steps.

## Prerequisite {#section_r43_1sz_q2b .section}

-   You have enabled Log Service and created the project and Logstore.
-   You have enabled [Dynamic Route for CDN](https://dcdn.console.aliyun.com/).
-   To [Enable HTTPS acceleration](#), [Enable HTTP acceleration](#) first.

## Configuration {#section_emc_bw1_r2b .section}

After HTTP Global Acceleration is enabled for the project, you can also configure Global Acceleration of Logtail, SDK, and other methods according to your needs.

1.  [Enable HTTP acceleration](#).
2.  Enable Global Acceleration of Logtail, SDK, and other methods.
    -   HTTPS

        If you use HTTPS to access Log Service, make sure that HTTPS acceleration is enabled. To configure HTTPS acceleration, see [Enable HTTPS acceleration](#).

    -   Logtail log collection

        When you install Logtail, select the **Global Acceleration** network type at the page prompt. Then you can obtain global acceleration when you collect logs by using Logtail.

    -   SDK, Producer, and Consumer

        Other ways to access Log Service such as SDK, Producer, and Consumer, can be accelerated by replacing the endpoint with `log-global.aliyuncs.com`.


## Enable HTTP acceleration {#section_sst_dsz_q2b .section}

1.  Log on to the [Dynamic Route for CDN Console](https://dcdn.console.aliyun.com/). Click **Domain Names** in the left-side navigation pane to enter the Domain Names page.
2.  Click **Add Domain Name** in the upper left corner to enter the Add Domain Name page.
3.  Enter the DCDN Domain and other information, and click **Next**.

    |Configuration|Description|
    |:------------|:----------|
    |Accelerated domain name|`project\_name.log-global.aliyuncs.com` Replace project\_name with your project name.|
    |Origin site type|Select Origin Domain.|
    |Domain name|Enter the public network endpoint for the region to which your project belongs. For information about endpoints, see [Service endpoint](../../../../intl.en-US/API Reference/Service endpoint.md).|
    |Port|Please select port 80. If you have an HTTPS acceleration requirement, see **Enable HTTPS acceleration**.|
    |Accelerated area|By default, this configuration item is not displayed and the acceleration area is Domesticate acceleration.If you have a demand for Global Acceleration, open a ticket for the Dynamic Route for CDN product to apply for a whitelist.

After your application is approved, you can select an acceleration region based on your needs.|

    For more information about adding domain names, see 8.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16815/15368967288063_en-US.png)

4.  Go to the Domain management page as prompted.

    You can view the **CNAME** of each corresponding domain name in the Domain name management page.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16815/15368967298064_en-US.png)

5.  Log on to the Log Service console and click **Global Acceleration** at the right of a specified project in the Project list.
6.  Enter the **CNAME** corresponding to the accelerated domain name in the dialog box. Click **Enable acceleration**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16815/15368967298065_en-US.png)

    After you complete the preceding steps, Global Acceleration for Log Service is enabled.


## Enable HTTPS acceleration {#section_ugs_nhm_q2b .section}

After enabling HTTP acceleration, if you have HTTPS access requirements, you can use the following steps to enable HTTPS acceleration.

1.  Log on to the [Dynamic Route for CDN Console](https://dcdn.console.aliyun.com/). Click **Domain Names** in the left-side navigation pane to enter the Domain Names page.
2.  Click **Configure** to the right of a specified domain name.
3.  Click **HTTPS Settings** in the left-side navigation pane and click **Modify** in the column of **SSL Certificate** to enter the HTTPS Settings page.
4.  Configure SSL Acceleration and Certificate Type.

    -   Enable SSL Acceleration.
    -   Select Free Certificate for Certificate Type.
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16815/15368967298066_en-US.png)

    After the configuration is completed, select **Agree to grant Alibaba Cloud permission to apply for a free certificate.**, and click **Confirm**.

    For more information about HTTPS settings, see 10.


## Verify if the acceleration configuration takes effect {#section_tgs_nhm_q2b .section}

## FAQ {#section_xgs_nhm_q2b .section}

-   **How to verify if the acceleration configuration takes effect**？

    After the configuration is completed, you can verify if the acceleration takes effect by accessing your accelerated domain name.

    For example, if Global Acceleration is enabled for the test-project project, you can use `curl` to send a request to the accelerated domain name. If the following type of output is returned, the acceleration takes effect.

    ```
    $curl test-project.log-global.aliyuncs.com
    {"Error":{"Code":"OLSInvalidMethod","Message":"The script name is invalid : /","RequestId":"5B55386A2CE41D1F4FBCF7E7"}}
    
    ```

    For more information about checking methods, see [How to verify if the acceleration takes effect](https://help.aliyun.com/knowledge_detail/65163.html).

-   **How to handle the error of** `project not exist` reported in accessing accelerated domain name?

    This problem is caused usually by an invalid source site address. Log on to the Dynamic Route for CDN console and change the source site address to  the public network address of the region to which your project belongs. For information about address list, see 12.

    **Note:** Changing the source site address has a synchronization delay of several minutes.


