# Console sharing embedment {#task_yzh_sxf_f2b .task}

After configuring the collection and query analysis functions for Log Service, you may want to directly use the log query analysis and dashboard functions, or share these log-related functions with other users. In this case, using RAM for sharing may generate management costs for many subaccounts. To avoid this, Log Service allows you to log on to embedded pages through a single point for integrated query analysis and dashboard.

**Benefits**

You can embed a specific Logstore query page and dashboard page into a self-built website. This gives you access to the analysis and visualization features of Log Service without logging on to Alibaba Cloud.

-   The independent query page and dashboard page can be easily embedded into any website.
-   You can generate a logon link by using the security token service \(STS\) and control the operation permissions, such as ready-only permission, by using remote access management \(RAM\).

1.  Log on to your self-built website. After logon, the Web server STS obtains a temporary identity for you.
    -   For more information on STS, see [Overview](../../../../intl.en-US/API Reference/STS access mode/Overview.md).
    -   Grant the user access to specified Logstores. For details, see [Grant RAM sub-accounts permissions to access Log Service](intl.en-US/User Guide/Access control RAM/Grant RAM sub-accounts permissions to access Log Service.md).
2.  Request Alibaba Cloud logon service for the logon token. After getting the temporary Access Key pair and security token from STS, call the logon service interface to obtain the logon token. Request example:

    ```
    http://signin.aliyun.com/federation?Action=GetSigninToken
                        &AccessKeyId=<Temporary Access Key pair returned by the STS>
                        &AccessKeySecret=<Temporary secret returned by the STS>
                        &SecurityToken=<Security token returned by the STS>
    ```

3.  Generate a logon-free link. 
    1.  Generate an access link along with the link to the embedded page after getting the logon token. The token is valid for three hours. Therefore, we recommend you generate a new logon token and redirect each access request to an embedded link to your self-built website through a 302 message. Request example:

        ```
        http://signin.aliyun.com/federation?Action=Login
                                    &LoginUrl=<Address to which a logon request is redirected upon a logon failure, which is usually configured to the URL on your self-built website through a 302 message;>
                                    &Destination=<Log Service page to be accessed. Pages for query and dashboard are supported.>
                                    &SigninToken=<Logon token obtained>
        ```

    2.  Embedded page. 
        -   A complete page for query and analysis \(multiple tags are allowed\):

            ```
            https://sls4service.console.aliyun.com/next/project/<Project name>/logsearch/<Logstore name>?hideTopbar=true&hideSidebar=true
            ```

        -   Query page:

            ```
            https://sls4service.console.aliyun.com/next/project/<Project name>/logsearch/<Logstore name>?isShare=true&hideTopbar=true&hideSidebar=true
            ```

        -   Dashboard page:

            ```
            https://sls4service.console.aliyun.com/next/project/<Project name>/dashboard/<Dashboard name>?isShare=true&hideTopbar=true&hideSidebar=true
            ```


The sample code in Java, PHP, and Python is as follows:

-   [Java](https://samplecode.oss-cn-hangzhou.aliyuncs.com/slsconsole.java?spm=a2c4g.11186623.2.6.LewJJX&file=slsconsole.java)：

    ```
    <dependency>
                        <groupId>com.aliyun</groupId>
                        <artifactId>aliyun-java-sdk-sts</artifactId>
                        <version>3.0.0</version>
                        </dependency>
                        <dependency>
                        <groupId>com.aliyun</groupId>
                        <artifactId>aliyun-java-sdk-core</artifactId>
                        <version>3.5.0</version>
                        </dependency>
                        <dependency>
                        <groupId>org.apache.httpcomponents</groupId>
                        <artifactId>httpclient</artifactId>
                        <version>4.5.5</version>
                        </dependency>
                        <dependency>
                        <groupId>com.alibaba</groupId>
                        <artifactId>fastjson</artifactId>
                        <version>1.2.47</version>
                        </dependency>
    ```

-   [PHP](https://samplecode.oss-cn-hangzhou.aliyuncs.com/slsconsole.php?spm=a2c4g.11186623.2.7.LewJJX)
-   [Python](https://samplecode.oss-cn-hangzhou.aliyuncs.com/slsconsole.py?spm=a2c4g.11186623.2.8.LewJJX&file=slsconsole.py)

