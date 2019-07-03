# 安装Logstash {#task_k2z_gp5_vdb .task}

日志服务提供Logstash插件，支持通过Logstash上传日志数据。

Logstash是一款流行的开源采集软件，您可以通过安装logstash-output-logservice插件上传数据到日志服务。插件的GitHub项目地址：[Logstash插件](https://github.com/aliyun/logstash-output-logservice)。

1.  安装Java。 
    1.  下载安装包。

        请进入[Java 官网](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 下载JDK并双击进行安装。

    2.  设置环境变量。

        打开高级系统设置，新增或修改环境变量。

        -   **PATH**：C:\\Program Files\\Java\\jdk1.8.0\_73\\bin
        -   **CLASSPATH**：C:\\Program Files\\Java\\jdk1.8.0\_73\\lib;C:\\Program Files\\Java\\jdk1.8.0\_73\\lib\\tools.jar
        -   **JAVA\_HOME**：C:\\Program Files\\Java\\jdk1.8.0\_73
    3.  验证。

        执行 `PowerShell` 或 `cmd.exe` 进行验证：

        ``` {#codeblock_xwt_gst_4t1}
        PS C:\Users\Administrator> java -version
        java version "1.8.0_73"
        Java(TM) SE Runtime Environment (build 1.8.0_73-b02)
        Java HotSpot(TM) 64-Bit Server VM (build 25.73-b02, mixed mode)
        PS C:\Users\Administrator> javac -version
        javac 1.8.0_73
        ```

2.  安装Logstash。 
    1.  下载安装包。

        官网下载：[Logstash主页](https://www.elastic.co/downloads/logstash) 。

        **说明：** 

        -   建议下载Logstash 5.0及以上版本以运行插件。

        -   经测试，在Mac OS 10.14.1、Windows 7、Linux （CentOS 7）三种系统环境下，Logstash 6.4.3版本安装插件正常、工作正常。

    2.  安装。

        解压安装包到安装目录。

3.  安装Logstash写日志服务插件。 请根据机器所处网络环境决定在线或离线安装模式：
    -   在线安装。

        该插件托管于 RubyGems，更多信息请[单击查看](https://rubygems.org/gems/logstash-output-logservice) 。

        执行 `PowerShell` 或 `cmd.exe`，进入Logstash安装目录。执行以下命令安装Logstash：

        ``` {#codeblock_56s_q2o_543}
        PS C:\logstash-6.4.3> .\bin\logstash-plugin install logstash-output-logservice
        ```

    -   离线安装。

        官网下载：进入 [logstash-output-logservice 页面](https://rubygems.org/gems/logstash-output-logservice)，单击右下角 **下载** 。

        如采集日志机器无法访问公网，请拷贝下载的 gem 包到本地目录。执行 PowerShell 或 cmd.exe，进入 Logstash 安装目录。执行以下命令安装Logstash：

        ``` {#codeblock_3cf_a3b_xaz}
        PS C:\logstash-6.4.3> .\bin\logstash-plugin install C:\logstash-6.4.3\logstash-output-logservice-0.4.0.gem
        ```

    -   验证。

        ``` {#codeblock_981_xlr_w5u}
        PS C:\logstash-6.4.3> .\bin\logstash-plugin list
        ```

        在本机已安装的插件列表中可以找到 logstash-output-logservice。


