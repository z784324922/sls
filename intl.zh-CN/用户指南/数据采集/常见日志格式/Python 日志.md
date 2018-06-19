# Python 日志 {#reference_bj1_ss5_vdb .reference}

Python 的 logging 模块提供了通用的日志系统，可以方便第三方模块或者是应用使用。这个模块提供不同的日志级别，并可以采用不同的方式记录日志，比如：文件、HTTP GET/POST、SMTP、Socket等，甚至可以自己实现具体的日志记录方式。logging 模块与 log4j 的机制是一样的，只是具体的实现细节不同。模块提供 logger、handler、filter、formatter。

采集Python日志，推荐您直接使用Logging Handler：

-   [使用Log Handler自动上传Python日志](https://aliyun-log-python-sdk.readthedocs.io/tutorials/tutorial_logging_handler.html)
-   [Log Handler自动解析KV格式的日志](https://aliyun-log-python-sdk.readthedocs.io/tutorials/tutorial_logging_handler_kv.html)
-   [Log Handler自动解析JSON格式的日志](https://aliyun-log-python-sdk.readthedocs.io/tutorials/tutorial_logging_handler_json.html)

## Python日志格式 {#section_pc1_m5b_ry .section}

日志的格式在 formatter 中指定日志记录输出的具体格式。formatter 的构造方法需要两个参数：消息的格式字符串和日期字符串，这两个参数都是可选的。

Python 日志格式：

```
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

```
2015-03-04 23:21:59,682 - log_test.py:16 - tst - first info message   
2015-03-04 23:21:59,682 - log_test.py:17 - tst - first debug message
```

## 配置Logtail收集Python日志 {#section_akh_2d3_dbb .section}

配置Logtail收集Python日志的详细操作步骤请参考[五分钟快速入门](../../../../intl.zh-CN/快速入门/五分钟快速入门.md)和[Apache 日志](intl.zh-CN/用户指南/数据采集/常见日志格式/Apache 日志.md)，根据您的网络部署和实际情况选择对应配置。

在生成正则式的部分，由于自动生成的正则式只参考了日志样例，无法覆盖所有的日志情况，所以需要用户在自动生成之后做一些微调。

常见的Python日志及其正则表达式：

-   日志样例：

    ```
    2016-02-19 11:03:13,410 - test.py:19 - tst - first debug message
    ```

    正则表达式：

    ```
    (\d+-\d+-\d+\s\S+)\s+-\s+([^:]+):(\d+)\s+-\s+(\w+)\s+-\s+(.*)
    ```

-   日志格式：

    ```
    %(asctime)s - %(filename)s:%(lineno)s - %(levelno)s %(levelname)s %(pathname)s %(module)s %(funcName)s %(created)f %(thread)d %(threadName)s %(process)d %(name)s - %(message)s
    ```

    日志样例：

    ```
    2016-02-19 11:06:52,514 - test.py:19 - 10 DEBUG test.py test <module> 1455851212.514271 139865996687072 MainThread 20193 tst - first debug message
    ```

    正则表达式：

    ```
    (\d+-\d+-\d+\s\S+)\s-\s([^:]+):(\d+)\s+-\s+(\d+)\s+(\w+)\s+(\S+)\s+(\w+)\s+(\S+)\s+(\S+)\s+(\d+)\s+(\w+)\s+(\d+)\s+(\w+)\s+-\s+(.*)
    ```


