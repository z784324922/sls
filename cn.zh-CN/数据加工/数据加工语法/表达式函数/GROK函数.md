# GROK函数 {#concept_1180778 .concept}

本文档主要介绍GROK函数的语法规则，包括参数解释、函数示例等。

由于正则表达式较为复杂，推荐您优先使用GROK函数。

**说明：** 您也可以将GROK函数与正则表达式函数混合使用，例如：

``` {#codeblock_9tk_fcc_vdn}
# 会匹配 abc: 192.168.1.1  或者 xyz: 192.168.2.2 等形式
e_match("content", grok(r"\w+: (%{IP})"))
# 不会匹配 abc: 192.168.1.1, 而是匹配\w+: 192.168.1.1
e_match("content", grok(r"\w+: (%{IP})", escape=True))
```

-   功能介绍

    根据正则表达式提取特定的值。

-   函数格式

    ``` {#codeblock_ghl_oqj_lsk}
    grok(pattern, escape=False, extend=None)
    ```

-   GROK语法

    ``` {#codeblock_tc7_9i1_h9x}
    %{SYNTAX} 
    %{SYNTAX:NAME}
    ```

    其中`SYNTAX`表示预定义正则模式，`NAME`表示分组。如下：

    ``` {#codeblock_6hy_iht_aul}
    "%{IP}"               # 等价于 r"(?:\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3})"
    "%{IP:source_id}"     # 等价于 r"(?P<source_id>\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3})"
    ("%{IP}")             # 等价于 r"(\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3})"
    ```

    GROK模式中有些是自带命名分组捕获的，所以针对这种模式只能使用`%{SYNTAX}`这种方式的语法。此类模式常见于语句解析，例如：

    ``` {#codeblock_3rh_6dz_kfg}
    "%{SYSLOGBASE}"        
    "%{COMMONAPACHELOG}" 
    "%{COMBINEDAPACHELOG}"
    "%{HTTPD20_ERRORLOG}"
    "%{HTTPD24_ERRORLOG}"
    "%{HTTPD_ERRORLOG}"
    ...
    ```

    具体请参见[GROK模式参考](../../../../cn.zh-CN/数据加工/数据加工语法/通用参考/GROK模式参考.md#)中的Log formats模块。

    此外，在GROK模式中有些是非捕获分组模式，例如：

    ``` {#codeblock_kk6_bnk_ozc}
    "%{INT}"    
    "%{YEAR}"
    "%{HOUR}"
    ...
    ```

-   参数说明

    |参数名称|参数类型|是否必填|说明|
    |----|----|----|--|
    |pattern|String|是|GROK语法。具体的GROK模式，请参见[GROK模式参考](../../../../cn.zh-CN/数据加工/数据加工语法/通用参考/GROK模式参考.md#)。|
    |escape|Bool|否|是否将其他非GROK pattern中的正则相关特殊字符做转义，默认不转义。|
    |extend|Dict|否|用户自定义的GROK表达式。|

-   函数示例
    -   示例1：提取日期和引用内容。

        原始日志：

        ``` {#codeblock_p50_025_2px}
        content: 2019 June 24 "I am iron man"
        ```

        加工规则：

        ``` {#codeblock_psu_0su_q0y}
        e_regex('content',grok('%{YEAR:year} %{MONTH:month} %{MONTHDAY:day} %{QUOTEDSTRING:motto}'))
        ```

        加工结果：

        ``` {#codeblock_psh_6wg_ab1}
        content: 2019 June 24 "I am iron man"
        year: 2019
        month: June
        day: 24
        motto: "I am iron man"
        ```

    -   示例2：提取http请求日志。

        原始日志：

        ``` {#codeblock_36m_9kc_2ul}
        content: 55.3.244.1 GET /index.html 15824 0.043
        ```

        加工规则：

        ``` {#codeblock_uje_ju5_c4s}
        e_regex('content',grok('%{IP:client} %{WORD:method} %{URIPATHPARAM:request} %{NUMBER:bytes} %{NUMBER:duration}'))
        ```

        加工结果：

        ``` {#codeblock_0hf_6fh_0o3}
        content: 55.3.244.1 GET /index.html 15824 0.043
        client: 55.3.244.1
        method: GET
        request: /index.html
        bytes: 15824
        duration: 0.043
        ```

    -   示例3：提取Apache日志。

        原始日志：

        ``` {#codeblock_fz3_ke7_oih}
        content: 127.0.0.1 - - [13/Apr/2015:17:22:03 +0800] "GET /router.php HTTP/1.1" 404 285 "-" "curl/7.19.7 (x86_64-redhat-linux-gnu) libcurl/7.19.7 NSS/3.15.3 zlib/1.2.3 libidn/1.18 libssh2/1.4.2"
        ```

        加工规则：

        ``` {#codeblock_6ni_cmi_b5g}
        e_regex('content',grok('%{COMBINEDAPACHELOG}'))
        ```

        加工结果：

        ``` {#codeblock_iey_zf0_pgl}
        content: 127.0.0.1 - - [13/Apr/2015:17:22:03 +0800] "GET /router.php HTTP/1.1" 404 285 "-" "curl/7.19.7 (x86_64-redhat-linux-gnu) libcurl/7.19.7 NSS/3.15.3 zlib/1.2.3 libidn/1.18 libssh2/1.4.2"
        clientip: 127.0.0.1
        ident: -
        auth: -
        timestamp: 13/Apr/2015:17:22:03 +0800
        verb: GET
        request: /router.php
        httpversion: 1.1
        response: 404
        bytes: 285
        referrer: "-"
        agent: "curl/7.19.7 (x86_64-redhat-linux-gnu) libcurl/7.19.7 NSS/3.15.3 zlib/1.2.3 libidn/1.18 libssh2/1.4.2"
        ```

    -   示例4：Syslog默认格式日志。

        原始日志：

        ``` {#codeblock_m5r_foi_b89}
        content: May 29 16:37:11 sadness logger: hello world
        ```

        加工规则：

        ``` {#codeblock_swl_79y_p5c}
        e_regex('content',grok('%{SYSLOGBASE} %{DATA:message}'))
        ```

        加工结果：

        ``` {#codeblock_oph_wja_nwy}
        content: May 29 16:37:11 sadness logger: hello world
        timestamp: May 29 16:37:11
        logsource: sadness
        program: logger
        message: hello world
        ```

    -   示例5：转义特殊字符。

        原始日志：

        ``` {#codeblock_iye_aan_uys}
        content: Nov  1 21:14:23 scorn kernel: pid 84558 (expect), uid 30206: exited on signal 3
        ```

        加工规则：

        ``` {#codeblock_omc_mmk_8ya}
        e_regex('content',grok(r'%{SYSLOGBASE} pid %{NUMBER:pid} \(%{WORD:program}\), uid %{NUMBER:uid}: exited on signal %{NUMBER:signal}'))
        """因为包含了一个正则特殊字符 "(" 与 ")", 也可以这么写:"""
        e_regex('content',grok('%{SYSLOGBASE} pid %{NUMBER:pid} (%{WORD:program}), uid %{NUMBER:uid}: exited on signal %{NUMBER:signal}', escape=True))
        ```

        加工结果：

        ``` {#codeblock_esg_j4e_z6y}
        content: Nov  1 21:14:23 scorn kernel: pid 84558 (expect), uid 30206: exited on signal 3
        timestamp: Nov  1 21:14:23
        logsource: scorn
        program: expect
        pid: 84558
        uid: 30206
        signal: 3
        ```

    -   示例6：用户自定义GROK表达式。

        原始日志：

        ``` {#codeblock_rsb_7j1_wtn}
        content: Beijing-1104,gary 25 "never quit"
        ```

        加工规则：

        ``` {#codeblock_an5_820_d3k}
        e_regex('content',grok('%{ID:user_id},%{WORD:name} %{INT:age} %{QUOTEDSTRING:motto}',extend={'ID': '%{WORD}-%{INT}'}))
        ```

        加工结果：

        ``` {#codeblock_ixd_whq_e5m}
        content: Beijing-1104,gary 25 "never quit"
        user_id: Beijing-1104
        name: gary
        age: 25
        motto: "never quit"
        ```


