# Configurations {#reference_byf_bwq_12b .reference}

Like using APIs to interact with Log Service, you must also specify basic configurations when using SDKs.  Currently, SDKs in all languages define a client class as the endpoint class.  Basic configurations are specified when the endpoint class is built and 

include the following items:

-   Service endpoint: Specify the service endpoint that the client must access.
-   •Alibaba Cloud AccessKey \(consisting of AccessKey ID and AccessKey Secret\): Specify the AccessKey used by the client to access Log Service.

For how to use the two configuration items, see the following sections.

## Service endpoint {#section_vbc_lpf_sy .section}

When using SDKs, you must identify the region where the Log Service project to be accessed resides \(such as China East 1 \(Hangzhou\) or China North 1 \(Qingdao\)\) and then select the Log Service endpoint that matches with the region to initialize the client. the client. The service endpoint is defined in the same way as the service endpoint of APIs [Service endpoint](../../../../../reseller.en-US/API Reference/Service endpoint.md) .

-   When selecting an endpoint for the client, make sure that the region where the project to be accessed resides is the same as the region that corresponds to the endpoint. Otherwise, you  cannot use SDK to access your specified project.
-   The client can only specify the service endpoint when being built, you must use different endpoints to build different clients if you want to access projects in different regions.
-   Currently, all the API service endpoints only support HTTP.
-   You can also use an intranet endpoint to avoid Internet bandwidth overhead if you are using SDKs in an Alibaba Cloud Elastic Compute Service \(ECS\) instance. For more information, see  [Service endpoint](../../../../../reseller.en-US/API Reference/Service endpoint.md).

## AccessKey {#section_dtk_npf_sy .section}

As [AccessKey](../../../../../reseller.en-US/API Reference/AccessKey.md)  described in AccessKey, all requests that interact with Log Service must undergo security verification. An AccessKey is a critical factor in request security verification and is composed of an AccessKey ID  and an AccessKey Secret. You must specify two parameters \(AccessKey ID and AccessKey Secret\), that is, the AccessKey, when building the client. Therefore, log on to the Alibaba  Cloud Access Key Management pageto obtain or create an AccessKey before using SDKs.

**Note:** 

-   If you have multiple AccessKeys under your Alibaba Cloud account, make sure that the AccessKey ID and AccessKey Secret specified when building the client are in pair. Otherwise, the AccessKey cannot  pass the security verification required by Log Service.
-   The specified AccessKey must be enabled. Otherwise, the request is denied by Log Service.  You can also log on to the Alibaba Cloud Access Key Management page to view the AccessKey status.

## Example  {#section_bj4_4pf_sy .section}

To access a project in region China East 1 \(Hangzhou\) and you have an enabled AccessKey  as follows:

```
AccessKeyId = "bq2sjzesjmo**************"
AccessKeySecret = "4fdO2fTDDnZPU/*************"
```

The corresponding client instance can be instanced as follows:

Java:

```
String endpoint = "cn-hangzhou.log.aliyuncs.com"; //The Log Service endpoint of region China East 1 (Hangzhou).
String accessKeyId = "bq2sjzesjmo86kq35behupbq"; //Your AccessKey ID.
String accessKeySecret = "4fdO2fTDDnZPU/L7CHNdemB2Nsk=";//Your AccessKey Secret.
Client client = new Client(endpoint, accessKeyId, acccessKeySecret);
//Use client to operate Log Service project...
```

NET\(C\#\):

```
String endpoint = "cn-hangzhou.log.aliyuncs.com"; //The Log Service endpoint of region China East 1 (Hangzhou).
String accessKeyId = "bq2sjzesjmo86kq35behupbq"; //Your AccessKey ID.
String accessKeySecret = "4fdO2fTDDnZPU/L7CHNdemB2Nsk=";//Your AccessKey Secret.
SLSClient client = new SLSClient(endpoint, accessKeyId, accessKeySecret);
//use client to operate sls project......
```

PHP:

```
$endpoint = 'cn-hangzhou.log.aliyuncs.com'; //The Log Service endpoint of region China East 1 (Hangzhou).
$accessKeyId = 'bq2sjzesjmo86kq35behupbq'; //Your AccessKey ID.
$accessKey = '4fdO2fTDDnZPU/L7CHNdemB2Nsk=';//Your AccessKey Secret.
$client = new Aliyun_Sls_Client($endpoint, $accessKeyId, $accessKey);
//use client to operate sls project......
```

Python:

```
//Use client to operate Log Service project...
endpoint = 'regionid.example.com'
# The Log Service endpoint of region China East 1 (Hangzhou).
accessKeyId = 'bq2sjzesjmo*************'
# Your AccessKey ID.
accessKey = '4fdO2fTDDnZPU/*************'  
client = LogClient(endpoint, accessKeyId, accessKey)
#use client to operate log project......
```

