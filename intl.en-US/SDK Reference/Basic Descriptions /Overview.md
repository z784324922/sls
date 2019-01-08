# Overview  {#reference_n3h_2sq_zdb .reference}

To allow developers to use Log Service more efficiently, Log Service provides software development kits \(SDKs\) in multiple languages \(Java,  .NET, Python, PHP, and C\).  Select to use an appropriate version as per your needs.

Log Service SDKs are implemented based on Log Service APIs and provide the same capabilities as Log Service APIs. For more information about the Log Service APIs, see [Overview](../../../../../reseller.en-US/API Reference/Overview.md).

Similar to Log Service APIs, you must have an enabled Alibaba Cloud AccessKey \(consisting of AccessKey ID and AccessKey Secret\) to use Log Service SDKs. For more information, see  [AccessKey](../../../../../reseller.en-US/API Reference/AccessKey.md).

To use Log Service SDKs, you must know the service endpoint of Log Service in each Alibaba Cloud region.  For how to specify the root endpoint in an SDK, see [SDK configurations](reseller.en-US/SDK Reference/Basic Descriptions /Configurations.md).

Though the implementation details of Log Service SDKs vary with different languages, the SDKs can be considered as Log Service APIs encapsulated in different languages and basically implement the same functions as follows.

-    [Unified encapsulation](reseller.en-US/SDK Reference/Basic Descriptions /Interface regulations.md)of the Log Service APIs, removing your need to  build specific API requests and parse responses.  The interfaces in various languages are similar, facilitating your switch between different languages.
-   [Digital signature](../../../../../reseller.en-US/API Reference/Request signature.md) logic for the Log Service APIs , greatly reducing the complexity of using APIs as you can ignore details of the API signature logic.
-   Encapsulation of logs collected to Log Service in the  [ProtoBuffer  format](../../../../../reseller.en-US/API Reference/Common resources/Data encoding method.md),  allowing you to write logs without caring about the details of Protocol Buffer format.
-   Implementation of the compression method defined in the Log Service APIs, removing the need to focus on the compression details. SDKs in some languages allow you to specify whether or not to write logs in the compression mode. \(By default, the compression mode is used.\)
-   Unified[error handling method](reseller.en-US/SDK Reference/Basic Descriptions /Handle errors.md) , allowing you to handle request exceptions in the method that languages are familiar with.
-   Currently, SDKs in all languages only support synchronous requests.

The download addresses, usage instructions, and complete programming references of SDKs in different languages are as follows.

|SDK language|Relevant document|Source code|
|:-----------|:----------------|:----------|
|Java |[Java SDK](reseller.en-US/SDK Reference/Java SDK.md), [UserGuide](http://log-java-docs.oss-cn-hangzhou.aliyuncs.com/)|[GitHub ](https://github.com/aliyun/aliyun-log-java-sdk)|
|.NET|[.NET SDK](reseller.en-US/SDK Reference/.NET SDK.md), [UserGuide](http://sls-dotnet-docs.oss-cn-hangzhou.aliyuncs.com/)|[GitHub](https://github.com/aliyun/aliyun-log-chsarp-sdk)|
|PHP|[PHP SDK](reseller.en-US/SDK Reference/PHP SDK.md)|[GitHub](https://github.com/aliyun/aliyun-log-php-sdk)|
|Node. js| |[GitHub](https://github.com/aliyun-UED/aliyun-sdk-js)|
|Python |[Python SDK](reseller.en-US/SDK Reference/Python SDK.md), [UserGuide](http://aliyun-log-python-sdk.readthedocs.io/README_CN.html)|[GitHub](https://github.com/aliyun/aliyun-log-python-sdk)|
|C|Usage instructions|[GitHub](https://github.com/aliyun/aliyun-log-c-sdk)|
|GO|Usage instructions|[GitHub](https://github.com/aliyun/aliyun-log-go-sdk)|
|iOS|[iOS SDK](reseller.en-US/SDK Reference/iOS SDK.md)|[GitHub](https://github.com/aliyun/aliyun-log-ios-sdk)|
|Android| |[GitHub](https://github.com/aliyun/aliyun-log-android-sdk)|

