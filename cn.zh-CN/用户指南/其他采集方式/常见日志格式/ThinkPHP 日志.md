# ThinkPHP 日志 {#reference_h4y_vv5_vdb .reference}

ThinkPHP 是一个 PHP 语言的 WEB 应用开发框架。

## ThinkPHP 日志格式 {#section_wx1_vbc_ry .section}

在ThinkPHP中打印日志按照以下格式：

``` {#codeblock_jdq_hdg_khk}
<?php
Think\Log::record('D 方法实例化没找到模型类' );
?>
```

## 日志示例 {#section_dgj_3h3_dbb .section}

``` {#codeblock_og9_44t_4sy}
[ 2016-05-11T21:03:05+08:00 ] 10.10.10.1 /index.php
INFO: [ app_init ] --START--
INFO: Run Behavior\BuildLiteBehavior [ RunTime:0.000014s ]
INFO: [ app_init ] --END-- [ RunTime:0.000091s ]
INFO: [ app_begin ] --START--
INFO: Run Behavior\ReadHtmlCacheBehavior [ RunTime:0.000038s ]
INFO: [ app_begin ] --END-- [ RunTime:0.000076s ]
INFO: [ view_parse ] --START--
INFO: Run Behavior\ParseTemplateBehavior [ RunTime:0.000068s ]
INFO: [ view_parse ] --END-- [ RunTime:0.000104s ]
INFO: [ view_filter ] --START--
INFO: Run Behavior\WriteHtmlCacheBehavior [ RunTime:0.000032s ]
INFO: [ view_filter ] --END-- [ RunTime:0.000062s ]
INFO: [ app_end ] --START--
INFO: Run Behavior\ShowPageTraceBehavior [ RunTime:0.000032s ]
INFO: [ app_end ] --END-- [ RunTime:0.000070s ]
ERR: D 方法实例化没找到模型类
```

## 配置Logtail收集ThinkPHP日志 {#section_yt4_1cc_ry .section}

在生成正则式的部分，由于自动生成的正则式只参考了日志样例，无法覆盖所有的日志情况，所以需要用户在自动生成之后做一些微调。

由于ThinkPHP是多行日志，而且模式并非固定，可以从日志中提取的字段包括时间、访问IP、访问的URL、以及打印的 Message。其中Message字段包含了多行信息，由于其模式不固定，只能打包到一个字段之中。

 **ThinkPHP日志的Logtail收集配置参数**：

行首正则式

``` {#codeblock_s5v_zg9_vkc}
\[\s\d+-\d+-\w+:\d+:\d+\+\d+:\d+\s.*
```

正则表达式写成：

``` {#codeblock_xkq_iv3_xjl}
\[\s(\d+-\d+-\w+:\d+:\d+)[^:]+:\d+\s]\s+(\S+)\s(\S+)\s+(.*)
```

时间表达式写成：

``` {#codeblock_l8j_t0b_zhb}
%Y-%m-%dT%H:%M:%S
```

