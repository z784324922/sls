# ThinkPHP logs {#reference_h4y_vv5_vdb .reference}

ThinkPHP is a Web application development framework based on the PHP language.

## Log format {#section_wx1_vbc_ry .section}

Logs are printed in the following format in ThinkPHP:

```
<? php
Think\Log::record('D method instantiation does not find the model class' );

```

## Log example {#section_dgj_3h3_dbb .section}

```
[ 2016-05-11T21:03:05+08:00 ] 10.10.10.1 /index.php
INFO: [ app_init ] --START--
INFO: Run Behavior\BuildLiteBehavior [ RunTime:0.000014s ]
INFO: [ app_init ] --END-- [ RunTime:0.000091s ]
Info: [app_begin] -- start --
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
ERR: D method instantiation does not find the model class
```

## Configure Logtail to collect ThinkPHP logs {#section_yt4_1cc_ry .section}

For the complete process of collecting ThinkPHP logs by using Logtail, see [Python logs](intl.en-US/User Guide/Data Collection/Common log formats/Python logs.md). Select the corresponding configuration based on your network deployment and actual situation. 

The automatically generated regular expression is only based on the log sample and does not cover all the situations of logs. Therefore, you must adjust the regular expression slightly after it is automatically generated. 

ThinkPHP logs are multiline logs whose mode is not fixed. The following fields can be extracted from the ThinkPHP logs: time, access IP, accessed URL, and printed  message.  The message field contains multiple lines of information and can only be packaged to one field  because the mode is not fixed.

**Logtail collects configuration parameters of ThinkPHP logs**

Regular expression at the beginning of the line:

```
\[\s\d+-\d+-\w+:\d+:\d+\+\d+:\d+\s.
```

Regular expression:

```
\[\s(\d+-\d+-\w+:\d+:\d+)[^:]+:\d+\s]\s+(\S+)\s(\S+)\s+(.
```

Time expression:

```
%Y-%m-%dT%H:%M:%S
```

