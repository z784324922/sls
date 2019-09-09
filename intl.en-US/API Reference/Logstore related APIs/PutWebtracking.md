# PutWebtracking {#reference_354467 .reference}

Combines multiple logs into one request.

If you want to collect logs by calling PutWebTracking, you must first enable Web Tracking for the destination Logstore where you want to store logs. This API action is suitable to the scenarios where larger numbers of logs needto be collected. For example, it would be suitable to call this API if you wanted to collect logs through a web page or a client.

You can also use the Web Tracking feature to collect logs. However, with Web Tracking, you can only write one log into a single request. For more information, see [Web Tracking ](../../../../reseller.en-US/Data Collection/Other collection methods/Web Tracking.md#).

## Request syntax {#section_h2h_pup_sjy .section}

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

## Request parameters {#section_6yr_vka_odh .section}

|Parameter|Type|Required?|Description|
|---------|----|---------|-----------|
|logstoreName|String|Yes|The Logstore name.|
|endpoint|String|Yes|The endpoint of Log Service. For more information, see [Service endpoint](reseller.en-US/API Reference/Service endpoint.md#).|
|project|String|Yes|The Project name.|

|Parameter|Type|Required?|Description|
|---------|----|---------|-----------|
|\_\_topic\_\_|String|No|The log topic.|
|\_\_source\_\_|String|Yes|The log source.|
|\_\_logs\_\_|List|Yes|The log list. Each element in the list is a JSON object, which represents a log. Each log must contain the time field. This field indicates the time the log is generated. The value of the time field is in the format of a UNIX timestamp. For example, 1558944742.|
|\_\_tags\_\_|Object|No|The log tage.|

## Request header {#section_047_uw9_75y .section}

A request can contain the following three request headers:

-   x-log-apiversion: 0.6.0
-   x-log-bodyrawsize: 1234
-   x-log-compresstype: gzip

The first two request headers are required, and the third one is optional. For information about the format and description of the request headers, see [Public request header](reseller.en-US/API Reference/Public request header.md#).

## Response header {#section_8ld_gvz_g8b .section}

The PutWebtracking interface does not have a special response header. For more information, see [Public response header](reseller.en-US/API Reference/Public response header.md#).

## Response elements {#section_bbd_kbr_ose .section}

No response element is returned when you call this action.

## Examples {#section_tca_lr7_oz9 .section}

**Request example** 

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

**Response example** 

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

