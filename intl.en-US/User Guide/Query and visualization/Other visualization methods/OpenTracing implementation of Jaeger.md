# OpenTracing implementation of Jaeger {#concept_ek5_rnq_zdb .concept}

The advent of containers and serverless programming methods greatly increased the efficiency of software delivery and deployment. The evolution of the architecture has shown these changes:

-   The application architecture is changing from a single system to microservices. Then, the business logic changes to the call and request between microservices.
-   In terms of resources, traditional physical servers are fading out and changing to the invisible virtual resources.

![](images/5875_en-US.png "Architectural evolution")

According to these changes, behind the elastic and standardized architecture, the operations and maintenance \(O&M\) and diagnostics requirements become more and more complex. To address this trend, a series of development and operations \(DevOps\)-oriented diagnostic and analysis systems have emerged, including centralized logging systems, centralized metrics systems, and distributed tracing systems.

In addition to Jaeger, Alibaba Cloud also provides the OpenTracing link tracing service [XTrace](https://www.aliyun.com/product/xtrace).

## Logging, metrics, and tracing systems {#section_z1s_p3q_12b .section}

The features of logging, metrics, and tracing systems are described as follows:

-   **A logging system is used to record discrete events**.

    The system records data such as the debugging or error information of an application. This data is the basis of diagnostics.

-   **A metrics system is used to record data that can be aggregated**.

    For example, the current depth of a queue can be defined as a metric and updated when an element is added to or removed from the queue. The number of HTTP requests can be defined as a counter that accumulates the number when new requests arrive.

-   **A tracing system is used to record information within the request scope**.

    The system records data such as the process and consumed time for a remote method call. This data is the tool we use to investigate system performance issues. The logging, metrics, and tracing systems provide overlapped features as follows.


![](images/5876_en-US.png "Logging, metrics, and tracing systems")

Based on these descriptions, we can classify existing systems. For example, Zipkin focuses on tracing. Prometheus begins to focus on metrics and may integrate with more tracing features in the future, but can hardly deal with logging. Systems such as ELK and Alibaba Cloud Log Service begin to focus on logging, continuously integrate with features of other fields, and are moving toward the intersection of all three systems.

For more information, see [Metrics, tracing, and logging](http://peter.bourgon.org/blog/2017/02/21/metrics-tracing-and-logging.html?spm=a2c4e.11153959.blogcont514488.18.463f30c2m1sv0g). Tracing systems are described in the following sections.

## Background {#section_ewb_s3q_12b .section}

The tracing technology has emerged since the 1990s. However, this technology moved into the mainstream with Google's article "Dapper, a Large-Scale Distributed Systems Tracing Infrastructure". Meanwhile, the article "Uncertainty in Aggregate Estimates from Sampled Distributed Traces" described the more detailed analysis of sampling. After these articles were published, a group of excellent tracing software programs were developed.

The following tracing software programs have been widely used:

-   Dapper \(Google\): the foundation for all tracers
-   Stackdriver Trace \(Google\)
-   Zipkin \(Twitter\)
-   Appdash \(Golang\)
-   EagleEye \(Taobao\)
-   Ditecting \(Pangu, the tracing system used by Alibaba Cloud cloud services\)
-   Cloud Map \(Ant tracing system\)
-   sTrace \(Shenma\)
-   X-Ray \(AWS\)

Distributed tracing systems have developed rapidly into many variants. However, they generally have three steps: code tracking, data storage, and query display.

The following example shows a distributed call. When a client initiates a request, the request first goes to the load balancer and then passes through the authentication service, billing service, and finally to the requested resources. Afterward, the system returns a result.

![](images/5877_en-US.png "Example of a distributed call")

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13157/15638735755883_en-US.png)

After collecting and storing the data, the distributed tracing system presents the traces in a timing diagram that contains a timeline. However, during data collection, the system has to intrude on user code and the API operations of different systems are not compatible. This causes great changes if you want to switch tracing systems.

## OpenTracing {#section_pxr_53q_12b .section}

The [OpenTracing](http://opentracing.io/?spm=a2c4e.11153959.blogcont514488.21.463f30c2m1sv0g) standard was introduced to prevent API compatibility issues among different distributed tracing systems. OpenTracing is a lightweight standardization layer. This layer is located between applications or class libraries and tracing or log analysis programs.

![](images/5878_en-US.png "OpenTracing")

 **Benefits** 

-   OpenTracing already enters Cloud Native Computing Foundation \(CNCF\) and provides unified concepts and data standards for global distributed tracing systems.
-   OpenTracing provides APIs with no relation to platforms or vendors. This allows developers to easily add or change the implementation of tracing systems.

 **Data model** 

In OpenTracing, a trace \(call chain\) is implicitly defined by the span in this call chain. A trace \(call trace\) can be regarded as a directed acyclic graph \(DAG\). The DAG consists of one or more spans. The relationships between spans are called references.

The following example shows a trace that consists of eight spans.

``` {#codeblock_2zi_v9w_9ay}
Causal relationships between spans in a single trace
        [Span A]  ←←←(the root span)
            |
     +------+------+
     |             |
 [Span B]      [Span C] ←←←(Span C is a child node of Span A, ChildOf)
     |             |
 [Span D]      +---+-------+
               |           |
           [Span E]    [Span F] >>> [Span G] >>> [Span H]
                                       ↑
                                       ↑
                                       ↑
                         (Span G is called after Span F, FollowsFrom)
```

In some cases, as shown in the following example, a sequence diagram based on a timeline better displays a trace \(call trace\).

``` {#codeblock_hd5_kky_cxm}
The time relationship between spans in a single trace
––|–––––––|–––––––|–––––––|–––––––|–––––––|–––––––|–––––––|–> time
 [Span A···················································]
   [Span B··············································]
      [Span D··········································]
    [Span C········································]
         [Span E·······]        [Span F··] [Span G··] [Span H··]
```

Each span contains the following statuses:

-   An operation name.
-   A start timestamp.
-   A finish timestamp.
-   Span tag. This is a collection of span tags that consist of key-value pairs. In a key-value pair, the key must be a string and the value can be a string, boolean, or numeric value.
-   Span log. This is a collection of span logs. Each log operation contains one key-value pair and one timestamp.

In a key-value pair, the key must be a string and the value can be of any type. However, be aware that tracers that support OpenTracing do not support all value types.

-   SpanContext. This is the context of the span.
-   References. They indicate the relationship between zero or multiple related spans. Spans establish the relationship based on the SpanContext.

Each SpanContext contains the following statuses:

-   Any OpenTracing implementation must transmit the current call chain status such as trace and span identifiers across process boundaries based on a unique span.
-   Baggage items that are the data accompanying a trace. They are a collection of key-value pairs stored in a trace and must be transmitted across process boundaries.

For more information about OpenTracing data models, see the OpenTracing semantic standards.

## Implementations {#section_fyr_53q_12b .section}

The document [Supported tracers](https://opentracing.io/docs/supported-tracers/) lists all OpenTracing implementations. In these implementations, [Jaeger](http://jaeger.readthedocs.io/en/latest/?spm=a2c4e.11153959.blogcont514488.24.463f30c2m1sv0g) and [Zipkin](https://zipkin.io/?spm=a2c4e.11153959.blogcont514488.25.463f30c2m1sv0g) are widely used.

## Jaeger {#section_gyr_53q_12b .section}

Jaeger is an open-source distributed tracing system released by Uber. This system is compatible with OpenTracing API operations.

## Architecture {#section_hyr_53q_12b .section}

![](images/5879_en-US.png "Architecture")

As shown in this figure, Jaeger consists of the following components:

-   Jaeger client: implements SDKs that are compatible with OpenTracing standards for different programming languages. An application uses an API operation to write data. The client library transmits trace information to the jaeger-agent according to the sampling policy specified in the application.
-   Agent: a network daemon that monitors span data received by the User Datagram Protocol \(UDP\) port and that sends multiple items of data to the collector at the same time. The agent is designed as a basic component and deployed on all hosts. The agent decouples the client library and the collector, and shields the client library from collector routing and discovery details.
-   Collector: receives data from the jaeger-agent and then writes the data to backend storage. The collector is designed as a stateless component. Therefore, you can simultaneously run an arbitrary number of jaeger-collectors.
-   Data store: the backend storage designed as a pluggable component that supports writing data to Cassandra and Elasticsearch.
-   Query: receives query requests, retrieves trace information from the backend storage system, and then displays the result in the user interface \(UI\). Query is stateless. You can start multiple instances, and deploy the instances behind NGINX load balancers.

## Jaeger on Alibaba Cloud Log Service {#section_qqh_gjq_12b .section}

[Jaeger on Alibaba Cloud Log Service](https://github.com/aliyun/aliyun-log-jaeger?spm=a2c4e.11153959.blogcont514488.27.463f30c2rnfGmn) is a Jaeger-based distributed tracing system. This system persists tracing data in Log Service, and queries and displays data by using the Jaeger native API operations.

![](images/5880_en-US.png "Jaeger")

## Benefits {#section_zpt_hjq_12b .section}

-   The native Jaeger only supports persistent storage in Cassandra and Elasticsearch. You must maintain the stability of the backend storage system and adjust the storage capacity. Jaeger on Alibaba Cloud Log Service uses Log Service of Alibaba Cloud to process large amounts of data. In this way, you can easily benefit from the Jaeger distributed tracing technology, and save more efforts on the backend storage system.
-   The Jaeger UI can query and display traces, but requires more support for analysis and troubleshooting. By using Jaeger on Alibaba Cloud Log Service, you can use the powerful query and analysis features of Log Service to efficiently analyze system issues.
-   Compared with Jaeger that uses Elasticsearch as the backend storage, Log Service costs only 13% of the price of Elasticsearch when you use the pay-as-you-go billing method. For more information, see Compare LogSearch/Analytics with ELK in log query and analysis.

## Procedure {#section_tqh_gjq_12b .section}

For more information, see [GitHub](https://github.com/aliyun/jaeger/blob/master/README_CN.md).

## Example {#section_uqh_gjq_12b .section}

HotROD is an application that consists of multiple microservices and uses the OpenTracing API operations to record trace information.

To use Jaeger on Alibaba Cloud Log Service to diagnose issues in HotROD, follow these steps as shown in the tutorial video:

1.  Configure Log Service.
2.  Run Jaeger by running the docker-compose command.
3.  Run HotROD.
4.  Use the Jaeger UI to retrieve the specified trace information.
5.  Use the Jaeger UI to view detailed trace information.
6.  Use the Jaeger UI to locate application performance bottlenecks.
7.  Use the Log Service console to locate application performance bottlenecks.
8.  The application calls the OpenTracing API operations.

## Tutorial {#section_vmq_p3q_12b .section}

[http://cloud.video.taobao.com//play/u/2143829456/p/1/e/6/t/1/50081772711.mp4](http://cloud.video.taobao.com//play/u/2143829456/p/1/e/6/t/1/50081772711.mp4)

You can use the following query statements in this example:

-   Collect statistics on the average latency and requests of frontend service HTTP GET/dispatch operations in minutes.

    ``` {#codeblock_v3x_c6f_hw0}
    process.serviceName: "frontend" and operationName: "HTTP GET /dispatch" |
    select from_unixtime( __time__ - __time__ % 60) as time,
    truncate(avg(duration)/1000/1000) as avg_duration_ms,
    count(1) as count
    group by __time__ - __time__ % 60 order by time desc limit 60
    ```

-   Compare the time consumed by two trace operations.

    ``` {#codeblock_56z_m5a_tdi}
    traceID: "trace1" or traceID: "trace2" |
    select operationName,
    (max(duration)-min(duration))/1000/1000 as duration_diff_ms
    group by operationName
    order by duration_diff_ms desc
    ```

-   Collect statistics on trace IP addresses with latencies of more than 1.5 seconds.

    ``` {#codeblock_qej_ng3_18e}
    process.serviceName: "frontend" and operationName: "HTTP GET /dispatch" and duration > 1500000000 |
    select "process.tags.ip" as IP,
    truncate(avg(duration)/1000/1000) as avg_duration_ms,
    count(1) as count
    group by "process.tags.ip"
    ```


