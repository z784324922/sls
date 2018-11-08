# HTTP方式 {#concept_pkp_cyc_wdb .concept}

HTTP输入可根据您的配置定期请求指定URL，将请求返回body值作为输入源上传到日志服务。

**说明：** 此功能目前仅支持Linux，依赖Logtail 0.16.0及以上版本，版本查看与升级参见[Linux](cn.zh-CN/用户指南/Logtail采集/安装/Linux.md)。

## 功能 {#section_bgy_jmx_pdb .section}

-   支持配置多个URL。
-   支持请求间隔配置。
-   支持HTTP方法配置。
-   支持自定义header。
-   支持设置method。
-   支持HTTPS。
-   支持检测body是否匹配固定pattern。

## 实现原理 {#section_dx3_mmx_pdb .section}

![](images/2931_zh-CN.png "实现原理")

如上图所示，Logtail内部会根据用户配置的HTTP请求url、method、header、body等信息定期对指定URL发起请求，将请求的返回的状态码、body内容以及响应时间做为数据上传到日志服务。

## 应用场景 {#section_w3d_4mx_pdb .section}

-   应用状态监控（以HTTP方式提供监控接口），例如：

    -   Nginx
    -   Docker（以HTTP方式）
    -   Elastic Search
    -   Haproxy
    -   其他以HTTP提供监控接口的服务
-   服务可用性检测，定期请求服务，通过状态码以及请求延迟做可用性监控。

-   数据定期拉取，例如微博评论、粉丝数等。


## 参数说明 {#section_vpl_pmx_pdb .section}

**参数说明**

该输入源类型为：`metric_http`。

|参数|类型|必选或可选|参数说明|
|:-|:-|:----|:---|
|Addresses|string 数组|必选|URL列表。**说明：** 必须以`http`或`https`开头。

|
|IntervalMs|int|必选|每次请求间隔，单位ms。|
|Method|string|可选|请求方法名，大写，默认为`GET`。|
|Body|string|可选|http body字段内容，默认为空。|
|Headers|key:string value:string map|可选|http header内容，默认为空。|
|PerAddressSleepMs|int|可选|Addresses列表中每个url请求间隔时间，单位ms，默认100ms。|
|ResponseTimeoutMs|int|可选|请求超时时间，单位ms，默认5000ms。|
|IncludeBody|bool|可选|是否采集请求的body，若为true，则将body以`content`为key存放，默认为false。|
|FollowRedirects|bool|可选|是否自动处理重定向，默认为false。|
|InsecureSkipVerify|bool|可选|是否跳过https安全检查，默认为false。|
|ResponseStringMatch|string|可选|对于返回body进行正则表达式检查，检查结果以`_response_match_`为key存放：若匹配，value为`yes`；若不匹配，value为`false`。|

## 使用限制 {#section_fmx_vmx_pdb .section}

-   此功能目前仅支持Linux，依赖Logtail 0.16.0及以上版本，版本查看与升级参见[Linux](cn.zh-CN/用户指南/Logtail采集/安装/Linux.md)。
-   URL必须以`http`或`https`开头。
-   目前不支持自定义证书。
-   不支持交互式通信方式。

## 默认字段 {#section_vwc_dnx_pdb .section}

每次请求默认会上传以下字段。

|字段名|说明|示例|
|:--|:-|:-|
| `_address_` |请求地址。|“[http://127.0.0.1/ngx\_status](http://127.0.0.1/ngx_status)“|
| `_method_` |请求方法。|“GET”|
| `_response_time_ms_` |响应延迟，单位为ms。|“1.320”|
| `_http_response_code_` |状态码。|“200”|
| `_result_` |是否成功，取值范围:`success`、`invalid_body`、`match_regex_invalid`、`mismatch`、`timeout`。|“success”|
| `_response_match_` |返回body是否匹配`ResponseStringMatch`字段，若不存在`ResponseStringMatch`字段则为空，取值范围:`yes`、`no`。|“yes”|

## 操作步骤 {#section_jr4_qnx_pdb .section}

每隔1000ms请求一次nginx status模块，URL为 `http://127.0.0.1/ngx_status`，将返回的body使用正则提取出其中的状态信息。详细配置如下：

1.  选择输入源。

    单击**数据接入向导**图标或**创建配置**，进入数据接入向导。并在**选择数据库类型**步骤中选择**Logtail自定义插件**。

2.  填写输入配置。

    进入输入源配置页面，填写**插件配置**。

    `inputs`部分为采集配置，是必选项；`processors`部分为处理配置，是可选项。采集配置部分需要按照您的数据源配置对应的采集语句，处理配置部分请参考[处理采集数据](cn.zh-CN/用户指南/Logtail采集/自定义插件/处理采集数据.md)配置一种或多种采集方式。

    示例配置如下：

    ```
    {
     "inputs": [
         {
             "type": "metric_http",
             "detail": {
                 "IntervalMs": 1000,
                 "Addresses": [
                     "http://127.0.0.1/ngx_status"
                 ],
                 "IncludeBody": true
             }
         }
     ],
     "processors" : [
         {
             "type": "processor_regex",
             "detail" : {
                 "SourceKey": "content",
                 "Regex": "Active connections: (\\d+)\\s+server accepts handled requests\\s+(\\d+)\\s+(\\d+)\\s+(\\d+)\\s+Reading: (\\d+) Writing: (\\d+) Waiting: (\\d+).*",
                 "Keys": [
                     "connection",
                     "accepts",
                     "handled",
                     "requests",
                     "reading",
                     "writing",
                     "waiting"
                 ],
                 "FullMatch": true,
                 "NoKeyError": true,
                 "NoMatchError": true,
                 "KeepSource": false
             }
         }
     ]
    }
    ```

3.  应用到机器组。

    进入应用到机器组页面。请在此处勾选**支持此插件的Logtail机器组**。

    如您之前没有创建过机器组，单击**+创建机器组**以创建一个新的机器组。


## 示例 {#section_zmp_b4x_pdb .section}

按照以上操作步骤配置处理方式后，即可在日志服务控制台查看采集处理过的日志数据。

解析后上传的日志数据除包含正则解析后的数据外，还包括HTTP请求附加的method、address、time、code、result信息。

```
"Index" : "7"  
"connection" : "1"  
"accepts" : "6079"  
"handled" : "6079"  
"requests" : "11596"  
"reading" : "0"  
"writing" : "1"  
"waiting" : "0"
"_method_" : "GET"  
"_address_" : "http://127.0.0.1/ngx_status"  
"_response_time_ms_" : "1.320"  
"_http_response_code_" : "200"  
"_result_" : "success"
```

