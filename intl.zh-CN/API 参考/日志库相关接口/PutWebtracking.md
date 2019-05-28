# PutWebtracking {#reference_354467 .reference}

[Web Tracking](../../../../intl.zh-CN/用户指南/其他采集方式/Web Tracking.md#) 采集方式，单个请求只能写入一条日志，针对日志量较大的场景，可以使用 PutWebTracking API 将多条日志合并为一次请求。与 Web Tracking 类似，使用 PutWebTracking 接口写入日志需要为 Logstore 打开 Web Tracking 开关。适用于在网页或者客户端采集日志的场景。

## 请求语法 {#section_h2h_pup_sjy .section}

``` {#codeblock_dnv_t0s_54z}
POST {project}.{endpoint}/logstores/{logstoreName}/track HTTP/1.1

x-log-apiversion: 0.6.0
x-log-bodyrawsize: 1234
x-log-compresstype: gzip

{
  "__topic__": "topic",
  "__source__": "source",
  "__logs__": [
    {
      "__time__": 1234567,
      "key1": "value1",
      "key2": "value2"
    },
    {
      "__time__": 1234567,
      "key1": "value1",
      "key2": "value2"
    }
  ],
  "__tags__": {
    "tag1": "value1",
    "tag2": "value2"
  }
}
```

## 请求参数 {#section_6yr_vka_odh .section}

**URL参数**

|参数名称|类型|是否必须|描述|
|----|--|----|--|
|logstoreName|string|是|Logstore名称。|
|endpoint|string|是|日志服务的Endpoint，请参考[服务入口](intl.zh-CN/API 参考/服务入口.md#)。|
|project|string|是|Project 名称。|

**请求Body** 

|参数名称|类型|是否必须|描述|
|----|--|----|--|
|\_\_topic\_\_|string|否|日志主题。|
|\_\_source\_\_|string|是|日志来源。|
|\_\_logs\_\_|list|是|日志内容列表。每个元素为一个 JSON 对象，表示一条日志。每条日志必须包含表示日志时间的 time 字段，格式为 UNIX 时间戳，如1558944742。|
|\_\_tags\_\_|object|否|日志标签。|

**请求头**

仅需要如下三个请求头：

-   x-log-apiversion: 0.6.0
-   x-log-bodyrawsize: 1234
-   x-log-compresstype: gzip

前两个为必选，第三个是可选，格式和含义请参考[公共请求头](intl.zh-CN/API 参考/公共请求头.md#)。

**响应头**

无特有响应头。关于 Log Service API 的公共响应头，请参考[公共响应头](intl.zh-CN/API 参考/公共响应头.md#)。

**响应元素**

成功后无任何响应元素。

## 示例 {#section_tca_lr7_oz9 .section}

**请求示例：** 

``` {#codeblock_3gc_uyp_q4h}
POST my-project.cn-hangzhou.log.aliyuncs.com/logstores/logstore-test/track HTTP/1.1

x-log-apiversion: 0.6.0
x-log-bodyrawsize: 1234
x-log-compresstype: gzip

{
  "__topic__": "topic",
  "__source__": "source",
  "__logs__": [
    {
      "__time__": 1558941776,
      "foo": "bar",
      "foo1": "bar1"
    },
    {
      "__time__": 1558941776,
      "bar": "foo",
      "bar1": "foo1"
    }
  ],
  "__tags__": {
    "tag1": "value1",
    "tag2": "value2"
  }
}
```

**响应示例：** 

``` {#codeblock_4f1_4mg_4ld}
HTTP/1.1 200 OK
Header:
{
Date=Wed, 20 May 2019 07:35:00 GMT, 
Content-Length=0, 
x-log-requestid=5642EFA499248C827B012B39, 
Connection=close, 
Server=nginx/1.6.1
}
```

