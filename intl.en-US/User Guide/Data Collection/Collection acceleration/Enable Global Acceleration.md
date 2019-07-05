# Enable Global Acceleration {#concept_bfn_n54_p2b .concept}

This topic describes how to enable Global Acceleration for Log Service.

## Prerequisites {#section_r43_1sz_q2b .section}

-   Log Service is activated. A project and a Logstore are created.
-   [Dynamic Route for CDN](https://dcdn.console.aliyun.com/) is activated.
-   [HTTP acceleration](#) is enabled before you enable [HTTPS acceleration](#) as needed.

## Procedure {#section_emc_bw1_r2b .section}

After you enable HTTP Global Acceleration for the target project, you can also configure the Logtail, SDK, and other methods as required to collect logs by using Global Acceleration.

1.  [Enable HTTP acceleration](#).
2.  \(Optional\) [Enable HTTPS acceleration](#).

    If you use HTTPS to access Log Service, ensure that HTTPS acceleration is enabled. For more information about how to configure HTTPS acceleration, see [Enable HTTPS acceleration](#).

3.  Collect logs by using Global Acceleration.
    -   Logtail
        -   To install Logtail after Global Acceleration is enabled, you can follow the installation procedure for **Global Acceleration** in [Install Logtail in Linux](reseller.en-US/User Guide/Logtail collection/Install/Install Logtail in Linux.md#). Then, Global Acceleration is automatically enabled when Log Service collects logs through Logtail.
        -   If Logtail is installed before you enable Global Acceleration, you need to manually switch the Logtail collection mode to global acceleration. For more information, see [Install Logtail in Windows](reseller.en-US/User Guide/Logtail collection/Install/Install Logtail in Windows.md#).
    -   SDK, Producer, or Consumer

        If you use other methods such as the SDK, Producer, and Consumer to access Log Service, you can replace the configured endpoint with `log-global.aliyuncs.com` to achieve global acceleration.


## Enable HTTP acceleration {#section_sst_dsz_q2b .section}

1.  Log on to the [Dynamic Route for CDN console](https://dcdn.console.aliyun.com/). In the left-side navigation pane, click **Domain Names** to go to the Domain Names page.
2.  Click **Add Domain Name** in the upper-left corner to go to the Add Domain Name page.
3.  Set DCDN Domain Name, enter other information as required, and then click **Next**.

    |Parameter|Description|
    |:--------|:----------|
    |DCDN Domain Name|Enter `project\_name.log-global.aliyuncs.com`, where project\_name is replaced with your project name.|
    |Origin Type|Select Origin Domain.|
    |Domain Name|Enter the Internet service endpoint for the region of your project. For more information about endpoints, see [Service endpoint](../../../../reseller.en-US/API Reference/Service endpoint.md#).|
    |Port|Select Port 80. If you need HTTPS acceleration, you can configure HTTPS separately. For more information, see **Enable HTTPS acceleration** in this topic.|
    |Acceleration Region|Mainland China is selected by default. If you need to use Global Acceleration, open a ticket to apply for a whitelist from Dynamic Route for CDN.

 After your application is approved, you can select an acceleration region based on your needs.|

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16815/15623087398063_en-US.png)

4.  Go to the Domain Names page as prompted.

    You can view the **CNAME** of the added accelerating domain name on the Domain Names page.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16815/15623087398064_en-US.png)

5.  Log on to the Log Service console. On the Projects page, click **Global Acceleration** in the Actions column of the target project.
6.  In the dialog box that appears, enter the **CNAME** of the accelerating domain name. Click **Enable Acceleration**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16815/15623087398065_en-US.png)

    After you complete the preceding steps, Global Acceleration is enabled for Log Service.


## Enable HTTPS acceleration {#section_ugs_nhm_q2b .section}

After HTTP acceleration is enabled, if you need to use HTTPS to access Log Service, you can enable HTTPS acceleration. The procedure is as follows:

1.  Log on to the [Dynamic Route for CDN console](https://dcdn.console.aliyun.com/). In the left-side navigation pane, click **Domain Names** to go to the Domain Names page.
2.  Click **Configure** in the Actions column of the target domain name.
3.  In the left-side navigation pane, click **HTTPS Settings**. On the page that appears, click **Modify** in the **SSL Certificate** section. The HTTPS Settings dialog box appears.
4.  Configure SSL Acceleration and Certificate Type.

    -   Enable SSL Acceleration.
    -   Select Free Certificate for Certificate Type.
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16815/15623087398066_en-US.png)

    After the configuration is completed, select **Agree to grant Alibaba Cloud permission to apply for a free certificate.**, and click **OK**.


## FAQ {#section_xgs_nhm_q2b .section}

-   **How do I check whether the acceleration configuration takes effect?** 

    After the configuration is completed, you can access your accelerating domain name to check whether the acceleration configuration takes effect.

    For example, if Global Acceleration is enabled for the test-project project, you can run a `curl` command to send a request to the accelerating domain name. The following response indicates that the acceleration configuration takes effect:

    ``` {#codeblock_3cv_85e_4ga}
    $curl test-project.log-global.aliyuncs.com
    {"Error":{"Code":"OLSInvalidMethod","Message":"The script name is invalid : /","RequestId":"5B55386A2CE41D1F4FBCF7E7"}}
    ```

-   **What can I do if the**`project not exist`**error is reported for access to an accelerating domain name?** 

    This error is caused usually by an invalid origin domain name. You need to log on to the Dynamic Route for CDN console and change the origin domain name of the accelerating domain name to the Internet service endpoint for the region of your project. For more information, see [Service endpoint](../../../../reseller.en-US/API Reference/Service endpoint.md#).

    **Note:** The change of the origin domain name takes several minutes. You can wait patiently.


