# 源Logstore读取错误 {#concept_2055105 .concept}

本文档为您介绍数据加工服务读取源Logstore错误的原因以及排查处理方法。

在加工引擎成功启动后，下一步是读取源Logstore的数据。数据加工引擎对源日志库采用流式读取，在加工过程中会持续不断的读取源Logstore中的数据。

![源Logstore读取错误](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1631539/156862372359308_zh-CN.png)

本环节产生错误主要是由于对源logstore的访问异常。可能的原因如下：

-   源Logstore信息配置错误。
-   源Logstore信息发生变化。
-   网络错误。

错误影响：

-   读取源Logstore产生错误时，加工任务会一直重试，直到重试成功或被手动停止。重试成功后，加工任务会继续正常工作。
-   加工任务会保存之前成功的断点并一直重试。重试成功后，从断点处继续读取，不会产生数据的丢失和冗余。

## 常见错误排查 {#section_o1i_q5a_9hf .section}

-   源Logstore配置了非法的AccessKey。

    非法的AccessKey主要分为两种：非法的AccessKeyId和非法的AccessKeySecret。

    -   错误日志：

        ``` {#codeblock_s54_0ff_jv1}
        #非法的AccessKeyId
        {
          "errorCode": "Unauthorized", 
          "errorMessage": "AccessKeyId not found: LTAIL3gUus8AEu11"
        }
        #非法的AccessKeySecret
        { 
          "errorCode": "SignatureNotMatch", 
          "errorMessage": "signature uJfAJbc0ji04gb+cXhh0qWtajpM= not match"
        }
        ```

    -   排查方法：

        检查任务配置项中，源Logstore的AccessKeyID和AccessKeySecret是否存在且正确。

-   源Logstore信息发生变化。

    用户配置了正确的源Logstore信息，可能也已经进行了部分加工任务，但是在数据加工的过程中，源Logstore信息发生了变化，导致原有的配置信息无法访问源Logstore。

    -   错误日志：

        源Logstore信息发生变化有主要是以下两种情况：

        -   源Logstore被删除。

            ``` {#codeblock_z01_cnp_psc}
            {
              "errorMessage": "Logstore [logstore_name] does not exist."
            }
            ```

        -   源Logstore的AccessKey信息发生变化。

            ``` {#codeblock_224_ry4_m19}
            #非法的AccessKeyId
            {
              "errorCode": "Unauthorized", 
              "errorMessage": "AccessKeyId not found: LTAIL3gUus8AEu11"
            }
            #非法的AccessKeySecret
            { 
              "errorCode": "SignatureNotMatch", 
              "errorMessage": "signature uJfAJbc0ji04gb+cXhh0qWtajpM= not match"
            }
            ```

    -   排查方法：
        -   检查源Logstore是否被删除。
        -   检查源Logstore的AccessKey信息是否发生改变。
-   网络错误。
    -   错误日志：

        ``` {#codeblock_9ga_0uk_tiu}
        {
          "errorCode": "LogRequestError",
          "errorMessage": "HTTPConnectionPool(host='your_host', port=80): Max retries exceeded with url: your_url (Caused by NewConnectionError: Failed to establish a new connection: [Errno 11001] getaddrinfo failed'"
        }
        ```

    -   排查方法：

        检查网络连接是否正常。

-   读取不到源Logstore数据。

    此条不属于错误，没有报错信息。具体请参见[通用错误排查](cn.zh-CN/数据加工/FAQ/错误排查概述.md#section_8q0_aw4_iz6)。


