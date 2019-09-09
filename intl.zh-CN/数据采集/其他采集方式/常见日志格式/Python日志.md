# Python日志 {#reference_bj1_ss5_vdb .reference}

Python的 logging 模块提供了通用的日志系统，可以方便第三方模块或者是应用使用。

Python的 logging模块提供不同的日志级别，并可以采用不同的方式记录日志，比如：文件、HTTP GET/POST、SMTP、Socket等，甚至可以自己实现具体的日志记录方式。logging 模块与 log4j 的机制相同，只是具体的实现细节不同。模块提供 logger、handler、filter、formatter。

采集Python日志，推荐您直接使用Logging Handler：

-   [使用Log Handler自动上传Python日志](https://aliyun-log-python-sdk.readthedocs.io/tutorials/tutorial_logging_handler.html)
-   [Log Handler自动解析KV格式的日志](https://aliyun-log-python-sdk.readthedocs.io/tutorials/tutorial_logging_handler_kv.html)
-   [Log Handler自动解析JSON格式的日志](https://aliyun-log-python-sdk.readthedocs.io/tutorials/tutorial_logging_handler_json.html)

## Python日志格式 {#section_pc1_m5b_ry .section}

日志的格式在 formatter 中指定日志记录输出的具体格式。formatter 的构造方法需要两个参数：消息的格式字符串和日期字符串，这两个参数都是可选的。

Python 日志格式：

``` {#codeblock_e1r_t3o_5tn}
import logging  
import logging.handlers  
LOG_FILE = 'tst.log'  
handler = logging.handlers.RotatingFileHandler(LOG_FILE, maxBytes = 1024*1024, backupCount = 5) # 实例化 handler   
fmt = '%(asctime)s - %(filename)s:%(lineno)s - %(name)s - %(message)s'  
formatter = logging.Formatter(fmt)   # 实例化 formatter  
handler.setFormatter(formatter)      # 为 handler 添加 formatter  
logger = logging.getLogger('tst')    # 获取名为 tst 的 logger  
logger.addHandler(handler)           # 为 logger 添加 handler  
logger.setLevel(logging.DEBUG)  
logger.info('first info message')  
logger.debug('first debug message')
```

## 字段含义 {#section_abx_zc3_dbb .section}

关于 formatter 的配置，采用的是 `%(key)s` 的形式，就是字典的关键字替换。提供的关键字包括：

|格式|含义|
|:-|:-|
|%\(name\)s|生成日志的Logger名称。|
|%\(levelno\)s|数字形式的日志级别，包括DEBUG, INFO, WARNING, ERROR和CRITICAL。|
|%\(levelname\)s|文本形式的日志级别，包括’DEBUG’、 ‘INFO’、 ‘WARNING’、 ‘ERROR’ 和’CRITICAL’。|
|%\(pathname\)s|输出该日志的语句所在源文件的完整路径（如果可用）。|
|%\(filename\)s|文件名。|
|%\(module\)s|输出该日志的语句所在的模块名。|
|%\(funcName\)s|调用日志输出函数的函数名。|
|%\(lineno\)d|调用日志输出函数的语句所在的代码行（如果可用）。|
|%\(created\)f|日志被创建的时间，UNIX标准时间格式，表示从1970-1-1 00:00:00 UTC计算起的秒数。|
|%\(relativeCreated\)d|日志被创建时间与日志模块被加载时间的时间差，单位为毫秒。|
|%\(asctime\)s|日志创建时间。默认格式是 “2003-07-08 16:49:45,896”，逗号后为毫秒数。|
|%\(msecs\)d|毫秒级别的日志创建时间。|
|%\(thread\)d|线程ID（如果可用）。|
|%\(threadName\)s|线程名称（如果可用）。|
|%\(process\)d|进程ID（如果可用）。|
|%\(message\)s|日志信息。|

## 日志样例 {#section_zcv_1d3_dbb .section}

日志输出样例：

``` {#codeblock_nz6_8le_dcy}
2015-03-04 23:21:59,682 - log_test.py:16 - tst - first info message   
2015-03-04 23:21:59,682 - log_test.py:17 - tst - first debug message
```

常见的Python日志及其正则表达式：

-   日志样例：

    ``` {#codeblock_7eg_1g8_9ox}
    2016-02-19 11:03:13,410 - test.py:19 - tst - first debug message
    ```

    正则表达式：

    ``` {#codeblock_yp2_03z_q6g}
    (\d+-\d+-\d+\s\S+)\s+-\s+([^:]+):(\d+)\s+-\s+(\w+)\s+-\s+(.*)
    ```

-   日志格式：

    ``` {#codeblock_w3s_59q_s93}
    %(asctime)s - %(filename)s:%(lineno)s - %(levelno)s %(levelname)s %(pathname)s %(module)s %(funcName)s %(created)f %(thread)d %(threadName)s %(process)d %(name)s - %(message)s
    ```

    日志样例：

    ``` {#codeblock_yaj_v5j_7ro}
    2016-02-19 11:06:52,514 - test.py:19 - 10 DEBUG test.py test <module> 1455851212.514271 139865996687072 MainThread 20193 tst - first debug message
    ```

    正则表达式：

    ``` {#codeblock_hgn_94r_iwt}
    (\d+-\d+-\d+\s\S+)\s-\s([^:]+):(\d+)\s+-\s+(\d+)\s+(\w+)\s+(\S+)\s+(\w+)\s+(\S+)\s+(\S+)\s+(\d+)\s+(\w+)\s+(\d+)\s+(\w+)\s+-\s+(.*)
    ```


## 配置Logtail收集Python日志 {#section_akh_2d3_dbb .section}

配置Logtail收集Python日志的详细操作步骤请参考[五分钟快速入门](../../../../intl.zh-CN/快速入门/五分钟快速入门.md)，根据您的网络部署和实际情况选择对应配置。

1.  创建Project和Logstore。详细步骤请参考[准备流程](intl.zh-CN/准备工作/准备流程.md)。
2.  单击**接入数据**按钮，并在**接入数据**页面中选择**正则-文本文件**。
3.  选择日志空间。

    可以选择已有的Logstore，也可以新建Project或Logstore。

4.  创建并配置机器组。

    在创建机器组之前，您需要首先确认已经安装了Logtail。 安装完Logtail后单击**确认安装完毕**创建机器组。如果您之前已经创建好机器组 ，请直接单击**使用现有机器组**。

    选择一个机器组，将该机器组从**源机器组**移动到**应用机器组**。

5.  数据源设置。

    1.  填写**配置名称**、**日志路径**，并选择日志收集模式为**完整正则模式**。
    2.  开启**单行模式**。
    3.  输入**日志样例**。

        ![采集模式](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13042/15680092429834_zh-CN.png)

    4.  开启**提取字段**。
    5.  配置**正则表达式**。
        -   通过划选生成正则表达式。

            如果自动生成的正则表达式不匹配您的日志样例，可以通过划选的方式生成正则表达式。日志服务支持对日志样例划词自动解析，即对您在划词时选取的字段自动生成正则表达式。请在**日志样例**中划选日志字段内容，并单击**正则**。**正则表达式**一栏中会出现划选内容的正则式。您可以通过多次划选生成日志样例的完整正则表达式。

            ![正则表达式](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13042/15680092429838_zh-CN.png)

        -   调整正则表达式。

            鉴于实际的日志数据格式可能会有细微变动，单击**手动输入正则表达式**，根据实际情况对自动生成的正则表达式做出调整，使其符合收集过程中所有可能出现的日志格式。

        -   验证正则表达式。

            正则表达式修改完成后，单击**验证** 。如果正则式没有错误，会出现字段提取的结果，如果有错误请再次调整正则式。

    6.  确认**日志内容抽取结果**。

        查看日志字段的解析结果，并为日志内容抽取结果填写对应的Key。

        分别为日志字段提取结果取一个有意义的字段名称，比如时间字段的命名为time。如果不使用系统时间，您必须指定Value为时间的字段，并将其Key命名为time。

        ![日志抽取结果](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13042/15680092429845_zh-CN.png)

    7.  开启**使用系统时间**。

        如果使用系统时间，则每条日志时间为Logtail客户端解析该条日志内容的时间。

    8.  （可选）配置**高级选项**。
    Logtail配置完成后，将此配置应用到机器组即可开始规范收集Python日志。

6.  查询分析配置。

    默认已经设置好索引，您也可以重新设置索引。


