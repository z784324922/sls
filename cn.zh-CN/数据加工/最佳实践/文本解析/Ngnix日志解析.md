# Ngnix日志解析 {#concept_2022820 .concept}

本实践案例旨在通过一种场景多种解决方案的对比，选择出一种最快最好的解决方案。本文档主要介绍正则解析方面的场景实践。

## 解析Nginx正确日志 {#section_jwo_u2t_fmr .section}

以下以一条Nginx日志为例，向大家介绍如何解析Nginx日志的多种方案。

``` {#codeblock_std_446_nan}
203.208.xx.xx - - [04/Jan/2019:16:06:38 +0800] "GET /atom.xml HTTP/1.1" 200 273932 "-" "Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)"
```

解析需求

1.  从Nginx日志中提取出clientip、ident、auth、timestamp、verb、request、url、httpversion、response、bytes、referrer、agent信息。
2.  对解析出来的url进行再提取，提取出url\_proto、url\_host、url\_param。
3.  对解析出来的url\_param进行再提取，提取出url\_path、url\_query信息。

原始日志在控制台收集到的日志格式是string格式，如下所示：

``` {#codeblock_9nu_we9_xct}
__source__:  30.43.xx.xx
__tag__:__client_ip__:  12.120.xx.xx
__tag__:__receive_time__:  1563443076
content: 203.208.xx.xx - - [04/Jan/2019:16:06:38 +0800] "GET http://cdn1cdedge0001.coxlab.net/_astats?application=&inf.name=eth0 HTTP/1.1" 200 273932 "-" "Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)"
```

LOG DSL编排

-   方案一：正则解析
    1.  针对需求1解析Nginx日志的加工编排如下：

        ``` {#codeblock_86a_ooy_5g1}
        e_regex("content",r'(?P<ip>\d+\.\d+\.\d+\.\d+)( - - \[)(?P<datetime>[\s\S]+)\] \"(?P<verb>[A-Z]+) (?P<request>[\S]*) (?P<protocol>[\S]+)["] (?P<code>\d+) (?P<sendbytes>\d+) ["](?P<refere>[\S]*)["] ["](?P<useragent>[\S\s]+)["]')
        ```

        预览处理日志：

        ``` {#codeblock_kfw_g6t_3nq}
        ip: 203.208.xx.xx
        datetime: 04/Jan/2019:16:06:38 +0800
        verb: GET
        request: http://cdn1cdedge0001.coxlab.net/_astats?application=&inf.name=eth0
        protocol: HTTP/1.1
        code: 200
        sendbytes: 273932
        refere: -
        useragent: Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)
        ```

    2.  针对需求2解析第一步加工后得到的url，加工编排如下：

        ``` {#codeblock_6op_mm0_rlv}
        e_regex('url',r'(?P<url_proto>(\w+)):\/\/(?P<url_domain>[a-z0-9.]*[^\/])(?P<uri_param>(.+)$)')
        ```

        预览处理日志：

        ``` {#codeblock_wn6_asz_cig}
        url_proto: http
        url_domain: cdn1cdedge0001.coxlab.net
        uri_param: /_astats?application=&inf.name=eth0
        ```

    3.  针对需求3解析第二步得到的url，加工编排如下：

        ``` {#codeblock_2am_l5t_7qz}
        e_regex('uri_param',r'(?P<uri_path>\/\_[a-z]+[^?])\?(?<uri_query>(.+)$)')
        ```

        预览处理日志：

        ``` {#codeblock_etm_jds_tx8}
        uri_path: /_astats
        uri_query: application=&inf.name=eth0
        ```

    4.  综上LOG DSL规则为如以下形式：

        ``` {#codeblock_qzv_mhu_ixy}
        """第一步：初步解析Nginx日志"""
        e_regex("content",r'(?P<ip>\d+\.\d+\.\d+\.\d+)( - - \[)(?P<datetime>[\s\S]+)\] \"(?P<verb>[A-Z]+) (?P<request>[\S]*) (?P<protocol>[\S]+)["] (?P<code>\d+) (?P<sendbytes>\d+) ["](?P<refere>[\S]*)["] ["](?P<useragent>[\S\s]+)["]')
        """第二步：解析第一步得到的url"""
        e_regex('url',r'(?P<url_proto>(\w+)):\/\/(?P<url_domain>[a-z0-9.]*[^\/])(?P<uri_param>(.+)$)')
        """第三步：解析第二步的到的url参数"""
        e_regex('uri_param',r'(?P<uri_path>\/\_[a-z]+[^?])\?(?<uri_query>(.+)$)')
        ```

        预览处理后的日志如下：

        ``` {#codeblock_pti_oce_nlj}
        __source__:  30.43.xx.xx
        __tag__:__client_ip__:  12.120.xx.xx
        __tag__:__receive_time__:  1563443076
        content: 203.208.xx.xx - - [04/Jan/2019:16:06:38 +0800] "GET http://cdn1cdedge0001.coxlab.net/_astats?application=&inf.name=eth0 HTTP/1.1" 200 273932 "-" "Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)"
        ip: 203.208.xx.xx
        datetime: 04/Jan/2019:16:06:38 +0800
        verb: GET
        request: http://cdn1cdedge0001.coxlab.net/_astats?application=&inf.name=eth0
        protocol: HTTP/1.1
        code: 200
        sendbytes: 273932
        refere: -
        useragent: Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)
        url_proto: http
        url_domain: cdn1cdedge0001.coxlab.net
        uri_param: /_astats?application=&inf.name=eth0
        uri_path: /_astats
        uri_query: application=&inf.name=eth0
        ```

-   方案二：GROK解析

    使用GROK模式解析Nginx日志，只需要`COMBINEDAPACHELOG`模式即可。

    |模式|规则|说明|
    |--|--|--|
    |COMMONAPACHELOG| `%{IPORHOST:clientip} %`

 `{HTTPDUSER:ident} %`

 `{USER:auth} \[%`

 `{HTTPDATE:timestamp}\] "(?:%`

 `{WORD:verb} %`

 `{NOTSPACE:request}(?: HTTP/%`

 `{NUMBER:httpversion})?|%`

 `{DATA:rawrequest})" %`

 `{NUMBER:response} (?:%`

 `{NUMBER:bytes}|-)`

 |解析出clientip、ident、auth、timestamp、verb、request、httpversion、response、bytes字段内容。|
    |COMBINEDAPACHELOG| `%{COMMONAPACHELOG} %`

 `{QS:referrer} %{QS:agent}`

 |解析出上一行中所有字段，另外还解析出referrer、agent字段。|

    1.  针对需求1解析Nginx日志的加工编排如下：

        ``` {#codeblock_dvl_ysh_meq}
        e_regex('content',grok('%{COMBINEDAPACHELOG}'))
        ```

        预览处理日志：

        ``` {#codeblock_r6q_emu_w3c}
        clientip: 203.208.xx.xx
        ident: -
        auth: -
        timestamp: 04/Jan/2019:16:06:38 +0800
        verb: GET
        request: http://cdn1cdedge0001.coxlab.net/_astats?application=&inf.name=eth0
        httpversion: 1.1
        response: 200
        bytes: 273932
        referrer: "-"
        agent: "Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)"
        ```

    2.  只需要使用GROK的以下几种模式组合即可对request完成解析：

        |模式|规则|说明|
        |--|--|--|
        |URIPROTO|`[A-Za-z]+(\+[A-Za-z+]+)?`|匹配url中的头部分。例如`http://hostname.domain.tld/_astats?application=&inf.name=eth0`会匹配到http。|
        |USER|`[a-zA-Z0-9._-]+`|匹配字母、数字和`._-`组合。|
        |URIHOST|`%{IPORHOST}(?::%`|匹配IPORHOST和POSINT。|
        |URIPATHPARAM|`%{URIPATH}(?:%{URIPARAM})?`|匹配url参数部分。|

        针对需求2解析第一步加工后得到的request，加工编排如下：

        ``` {#codeblock_8j3_irf_75n}
        e_regex('request',grok("%{URIPROTO:uri_proto}://(?:%{USER:user}(?::[^@]*)?@)?(?:%{URIHOST:uri_domain})?(?:%{URIPATHPARAM:uri_param})?"))
        ```

        预览处理日志：

        ``` {#codeblock_c9h_bfe_jn7}
        url_proto: http
        url_domain: cdn1cdedge0001.coxlab.net
        uri_param: /_astats?application=&inf.name=eth0
        ```

    3.  使用GROK的以下模式即可完成对url\_param的解析：

        |模式|规则|说明|
        |--|--|--|
        |GREEDYDATA|`.*`|匹配任意或多个除换行符。|

        针对需求3解析第二步得到的url，加工编排如下：

        ``` {#codeblock_123_whs_8yf}
        e_regex('url_param',grok("%{GREEDYDATA:uri_path}\?%{GREEDYDATA:uri_query}"))
        ```

        预览处理日志：

        ``` {#codeblock_b3w_azb_kbc}
        uri_path: /_astats
        uri_query: application=&inf.name=eth0
        ```

    4.  综上LOG DSL规则为如以下形式：

        ``` {#codeblock_1lu_cjb_pes}
        """第一步：初步解析Nginx日志"""
        e_regex('content',grok('%{COMBINEDAPACHELOG}'))
        """第二步：解析第一步得到的url"""
        e_regex('request',grok("%{URIPROTO:uri_proto}://(?:%{USER:user}(?::[^@]*)?@)?(?:%{URIHOST:uri_domain})?(?:%{URIPATHPARAM:uri_param})?"))
        """第三步：解析第二步的到的url参数"""
        e_regex('url_param',grok("%{GREEDYDATA:uri_path}\?%{GREEDYDATA:uri_query}"))
        ```

        预览处理后的日志如下：

        ``` {#codeblock_yuk_wnb_2kd}
        __source__:  30.43.xx.xx
        __tag__:__client_ip__:  12.120.xx.xx
        __tag__:__receive_time__:  1563443076
        content: 203.208.xx.xx - - [04/Jan/2019:16:06:38 +0800] "GET http://cdn1cdedge0001.coxlab.net/_astats?application=&inf.name=eth0 HTTP/1.1" 200 273932 "-" "Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)"
        clientip: 203.208.xx.xx
        ident: -
        auth: -
        timestamp: 04/Jan/2019:16:06:38 +0800
        verb: GET
        request: http://cdn1cdedge0001.coxlab.net/_astats?application=&inf.name=eth0
        httpversion: 1.1
        response: 200
        bytes: 273932
        referrer: "-"
        agent: "Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)"
        url_proto: http
        url_domain: cdn1cdedge0001.coxlab.net
        uri_param: /_astats?application=&inf.name=eth0
        uri_path: /_astats
        uri_query: application=&inf.name=eth0
        ```


## 方案对比 {#section_dcl_3c9_his .section}

结合上述示例，可以分析出正则解析和Grok模式解析Nginx日志两种方案优缺点。

-   正则方案

    对不是很熟悉正则的开发人员，使用正则解析日志效率会比较低，而且学习成本会比较大。还有就是灵活性不够，例如将request内容改成：

    ``` {#codeblock_fye_ma1_qcn}
    http://twiss@cdn1cdedge0001.coxlab.net/_astats?application=&inf.name=eth0
    ```

    使用正则为：

    ``` {#codeblock_44t_qzv_ss7}
    (?P<url_proto>(\w+)):\/\/(?P<url_domain>[a-z0-9.]*[^\/])(?P<uri_param>(.+)$)
    ```

    request则会解析成：

    ``` {#codeblock_ve0_lup_fd4}
    url_proto: http
    url_domain: twiss@
    uri_param: cdn1cdedge0001.coxlab.net/_astats?application=&inf.name=eth0
    ```

    很显然，如果还使用原来的正则模式的话，解析出来的内容是不符合要求的。因此，还需要修改正则模式才能正常解析。由此可见，灵活的使用正则进行解析的难度比较高。

-   GROK方案

    GROK学习成本低，只需要了解不同模式所代表的字段类型，就可以轻松解析日志内容。可以通过用户文档中[GROK模式参考](cn.zh-CN/数据加工/数据加工语法/通用参考/GROK模式参考.md#)来学习实践。

    Grok灵活性高，以上述正则方案中例子为参考。request内容改成：

    ``` {#codeblock_wqp_tpg_o83}
    http://twiss@cdn1cdedge0001.coxlab.net/_astats?application=&inf.name=eth0
    ```

    GROK模式不变：

    ``` {#codeblock_yiv_tir_foj}
    e_regex('request',grok("%{URIPROTO:uri_proto}://(?:%{USER:user}(?::[^@]*)?@)?(?:%{URIHOST:uri_domain})?(?:%{URIPATHPARAM:uri_param})?"))
    ```

    request则会解析成：

    ``` {#codeblock_lzh_keo_u9d}
    url_proto: http
    user: twiss
    url_domain: cdn1cdedge0001.coxlab.net
    uri_param: /_astats?application=&inf.name=eth0
    ```

    在GROK模式不变，request添加user的情况下，还是能够正确解析出正确的日志内容。


对比结论：

从灵活性、高效性、低成本、学习曲线等方面对比，GROK都要比直接使用正则表达式有优势。当前数据加工已经提供了四百种模式，建议优先使用。GROK模式的本质其实还是正则表达式，在需要的情况下，也可以混合使用GROK与正则，甚至自行编写需要的正则。

## 场景：解析Nginx错误日志 {#section_qsi_zid_4h1 .section}

以下以一条Nginx错误日志为例，向大家讲解如何使用GROK解析Nginx错误日志。

``` {#codeblock_5tj_6l9_1jj}
__source__:  30.43.xx.xx
__tag__:__client_ip__:  12.120.xx.xx
__tag__:__receive_time__:  1563443076
content: 203.208.xx.xx - - [04/Jan/2019:16:06:38 +0800] "GET /atom.xml HTTP/1.1" 200 273932 "-" "Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)"
```

需求：解析出Nginx错误日志。本案例主要使用GROK去做解析。

在控制台收集到的日志格式是string格式，如下所示：

``` {#codeblock_i54_uzf_fff}
__source__:  30.43.xx.xx
__tag__:__client_ip__:  12.120.xx.xx
__tag__:__receive_time__:  1563443076
content: 2019/08/07 16:05:17 [error] 1234#1234: *1234567 attempt to send data on a closed socket: u:111111ddd, c:0000000000000000, ft:0 eof:0, client: 1.2.3.4, server: sls.aliyun.com, request: "GET /favicon.ico HTTP/1.1", host: "sls.aliyun.com", referrer: "https://sls.aliyun.com/question/answer/123.html?from=singlemessage"
```

LOG DSL编排：

``` {#codeblock_7vr_b0g_pnk}
e_regex('content',grok('%{DATESTAMP:request_time} \[%{LOGLEVEL:log_level}\] %{POSINT:pid}#%{NUMBER}: %{GREEDYDATA:errormessage}(?:, client: (?<client>%{IP}|%{HOSTNAME}))(?:, server: %{IPORHOST:server})(?:, request: "%{WORD:verb} %{NOTSPACE:request}( HTTP/%{NUMBER:http_version})")(?:, host: "%{HOSTNAME:host}")?(?:, referrer: "%{NOTSPACE:referrer}")?'))
```

输出日志：

``` {#codeblock_c5z_k0v_z90}
__source__:  30.43.xx.xx
__tag__:__client_ip__:  12.120.xx.xx
__tag__:__receive_time__:  1563443076
request_time: 19/08/07 16:05:17
log_level: error
pid: 1234
errormessage: *1234567 attempt to send data on a closed socket: u:111111ddd, c:0000000000000000, ft:0 eof:0
client: 1.2.3.4
server: sls.aliyun.com
verb: GET
request: /favicon.ico
http_version: 1.1
host: sls.aliyun.com
referrer: https://sls.aliyun.com/question/answer/123.html?from=singlemessage
```

