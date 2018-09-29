# OpenTracing implementation of Jaeger {#concept_ek5_rnq_zdb .concept}

The advent of containers and serverless programming methods greatly increased the efficiency of software delivery and deployment. In the evolution of the architecture, there have been two changes:

-   The application architecture is changing from a single system to microservices. Then, the business logic changes to the call and request between microservices.
-   In terms of resources, traditional physical servers are fading out and changing to the invisible virtual resources.

![](images/5875_en-US.png "Architectural Evolution")

The preceding two changes show that behind the elastic and standardized architecture, the Operation & Maintenance \(O&M\) and diagnosis requirements are becoming more and more complex. To respond to these changes, a series of DevOps-oriented diagnosis and analysis systems have emerged, including the centralized log system \(logging\), the centralized measurement system \(metrics\), and the distributed tracing system \(tracing\).

## Logging, metrics, and tracing {#section_z1s_p3q_12b .section}

See the following characteristics of logging, metrics, and tracing:

-     **Logging is used to record discrete events**.

    For example, the debugging or error information of an application, which is the basis of problem diagnosis.

-   Metrics is used to record data that can be aggregated.

    For example, the current depth of a queue can be defined as a metric and updated when an element is added to or removed from the queue. The number of HTTP requests can be defined as a counter that accumulates the number when new requests are received.

-     **Tracing is used to record information within the request scope**.

    For example, the process and consumed time for a remote method call,  This is the tool we use to investigate system performance issues. Logging, metrics, and tracing have overlapping parts as follows.


![](images/5876_en-US.png "Logging, metrics and tracing")

We can use this information to classify existing systems. For example, Zipkin focuses on tracing. Prometheus begins to focus on metrics and may integrate with more tracing functions over time, but has little interest in logging. Systems such as ELK and Alibaba Cloud Log Service begin to focus on logging,  continuously integrate with features of other fields, and are moving to the center in the preceding figure.

For more information about the relationship among [Metrics, tracing, and   logging](http://peter.bourgon.org/blog/2017/02/21/metrics-tracing-and-logging.html?spm=a2c4e.11153959.blogcont514488.18.463f30c2m1sv0g). In what follows, we will introduce tracing systems.

## Technical background of tracing {#section_ewb_s3q_12b .section}

Tracing technology has been existing since the 1990s. However, the article “Dapper, a Large-Scale Distributed  Systems Tracing Infrastructure” of Google brings this field into the mainstream. The more detailed analysis of sampling is in the article “Uncertainty in Aggregate Estimates from Sampled Distributed Traces”.  After these articles were published, a group of excellent tracing software programs were developed.

Among them, the popular ones are as follows:

-   Dapper \(Google\): Foundation for all tracers
-   StackDriver Trace \(Google\)
-   Zipkin（Twitter）
-   Appdash（golang）
-   EagleEye \(Taobao\)
-   Ditecting \(Pangu, the tracing system used by Alibaba Cloud cloud products\)
-   Cloud Map \(Ant tracing system\)
-   sTrace \(Shenma\)
-   X-ray \(AWS\)

Distributed tracing systems have developed rapidly, with many variants. However, they generally have three steps: code tracking, data storage, and query display.

An example of a distributed call is given in the following figure. When the client initiates a request, it is first sent to the load balancer and then passes through the authentication service, billing service, and finally to the requested resources. Finally, the system returns a result.

![](images/5877_en-US.png "Example of distributed calls")

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13157/15381935105883_en-US.png)

After the data is collected and stored, the distributed tracing system generally presents the traces as a timing diagram containing a timeline. However, in the data collection process, generally you have to make great  changes to switch the tracing system because the system has to intrude on user codes and APIs of different systems are not compatible.

## OpenTracing {#section_pxr_53q_12b .section}

The   [OpenTracing](http://opentracing.io/?spm=a2c4e.11153959.blogcont514488.21.463f30c2m1sv0g) specification is created to address the problem of API incompatibility between different distributed tracing systems. OpenTracing is a lightweight standardization layer and is located between applications/class libraries and tracing or log analysis programs.

![](images/5878_en-US.png "OpenTracing")

**Advantages**

-   OpenTracing already enters CNCF and provides unified concept and data standards for global distributed tracing systems.
-   OpenTracing provides APIs with no relation to platforms or vendors, which allows developers to conveniently add \(or change\) the implementation of tracing system.

**data model**

In OpenTracing, a trace \(call chain\) is implicitly defined by the span in this call chain.  Specifically, a trace \(call chain\) can be considered as a directed acyclic graph \(DAG\) composed of multiple spans. The relationship between spans is called References.

For example, the following trace is composed of eight spans:

```
In a single trace, there are causal relationships between spans
        [Span A] ←←←(the root span)
            |
     +------+------+
     |             |
  [Span B] [Span C] ←←←(Span C is a child node of Span A, ChildOf)
     |             |
 [Span D] +---+-------+
               |           |
           [Span E] [Span F] >>> [Span G] >>> [Span H]
                                       ↑
                                       ↑
                                       ↑
                         (Span G is called after Span F, FollowsFrom)
```

Sometimes, as shown in the following example, a timing diagram based on a timeline can better display the trace \(call chain\):

```
The time relationship between spans in a single trace.
––|–––––––|–––––––|–––––––|–––––––|–––––––|–––––––|–––––––|–> time
 [Span A···················································]
   [Span B··············································]
      [Span D··········································]
    [Span C········································]
         [Span E·······] [Span F··] [Span G··] [Span H··]
```

each SPAN contains the following states:

-   An operation name.
-   A start timestamp.
-   A finish timestamp.
-   Span tag. A collection of span tags composed of key-value pairs. In a key-value pair, the key must be a string and the value type can be string, boolean, or numeric value.
-   Span log. A collection of span logs. Each log operation contains one key-value pair and one timestamp. In a key-value pair, the key 

must be a string and the value can be of any type.  However, note that not all tracers that support  OpenTracing must support all value types.

-   SpanContext. The context of the span.
-   References \(relationship between spans\). Zero or multiple related spans \(establish the relationship between spans based on the SpanContext\).

Each SpanContext contains the following statuses:

-   Any OpenTracing implementation must transmit the current call chain status \(for example,  trace and span IDs\) across process boundaries based on a unique span.
-   Baggage items, which are the data that accompanies a trace. This is a collection of key-value pairs stored in a trace and also must be transmitted across process boundaries.

For more information about OpenTracing data models, see the OpenTracing semantic standards.

## Function implementation {#section_fyr_53q_12b .section}

[Supported tracer implementations](http://opentracing.io/documentation/pages/supported-tracers.html?spm=a2c4e.11153959.blogcont514488.23.463f30c2m1sv0g) lists all OpenTracing implementations. Of these implementations, [Jaeger](http://jaeger.readthedocs.io/en/latest/?spm=a2c4e.11153959.blogcont514488.24.463f30c2m1sv0g) and [Zipkin](https://zipkin.io/?spm=a2c4e.11153959.blogcont514488.25.463f30c2m1sv0g) are the most popular ones.

## Jaeger {#section_gyr_53q_12b .section}

Jaeger is an open-source distributed tracing system released by Uber. It is compatible with OpenTracing APIs.

## Architecture {#section_hyr_53q_12b .section}

![](images/5879_en-US.png "Architecture")

As shown in the preceding figure, Jaeger is composed of the following components.

-   Jaeger client: Implements SDKs that conform to OpenTracing standards for different languages.  Applications use the API to write data. The client library  transmits trace information to the jaeger-agent according to the sampling policy specified by the application.
-   Agent: A network daemon that monitors span data received by the UDP port and then sends the data to the collector in batches.  It is designed as a basic component and deployed to all hosts. The agent decouples the client library and collector, shielding the client library from collector routing and discovery details.
-   Collector: Receives the data sent by the jaeger-agent and then writes the data to backend storage. The collector is designed as a stateless component. Therefore, you can simultaneously run an arbitrary number of jaeger-collectors.
-   Data store: The backend storage is designed as a pluggable component that supports writing data to Cassandra and Elasticsearch.
-   Query: Receives query requests, retrieves trace information from the backend storage system, and displays it in the UI. Query is stateless and you can start multiple instances. You can deploy the instances behind load balancers such as Nginx.

## Jaeger on Alibaba Cloud Log Service {#section_qqh_gjq_12b .section}

is based on Jaeger, which allows you to persistently store collected tracing data in Log Service, and query and display the data by using the native Jaeger interfaces.

![](images/5880_en-US.png "Jaeger")

## Advantages {#section_zpt_hjq_12b .section}

-   The native Jaeger only supports the persistent storage of data to Cassandra and Elasticsearch. You must maintain the stability of the backend storage system and adjust the storage capacity on your own. Jaeger on Alibaba Cloud Log Service  leverages the capability of Log Service to process massive volumes of data and allows you to enjoy the convenience in the distributed tracing field without worrying about backend storage system problems.
-   The Jaeger UI only provides query and trace display functions, without sufficient support for problem analysis and troubleshooting. By using Jaeger on Alibaba Cloud Log Service, you can take advantage of the powerful query and analysis capabilities of Log Service to quickly analyze the problems in the system.
-   Compared to Jaeger using Elasticsearch as the backend storage, Log Service supports the Pay-As-You-Go billing method and the cost is  only 13% of the Elasticsearch cost. For more information, see Compare LogSearch/Analytics with ELK in log query and analysis.

## serviceLatency The backend latency \(in milliseconds\). Procedure {#section_tqh_gjq_12b .section}

For more information, see [GitHub](https://github.com/aliyun/jaeger/blob/master/README_CN.md).

## Configuration example {#section_uqh_gjq_12b .section}

HotROD is an application composed of multiple microservices and uses the OpenTracing API to record trace information.

Follow these steps to use Jaeger on Alibaba Cloud Log Service to diagnose problems in HotROD. The video contains the following:

1.  Configure Log Service.
2.  Run Jaeger by running the docker-compose command.
3.  Run HotROD.
4.  Use the Jaeger UI to retrieve the specified trace information.
5.  Use the Jaeger UI to view detailed trace information.
6.  Use the Jaeger UI to locate application performance bottlenecks.
7.  Use the Log Service console to locate the performance bottleneck of the application.
8.  The application calls the OpenTracing API.

## Configuration tutorial {#section_vmq_p3q_12b .section}

[http://cloud.video.taobao.com//play/u/2143829456/p/1/e/6/t/1/50081772711.mp4](http://cloud.video.taobao.com//play/u/2143829456/p/1/e/6/t/1/50081772711.mp4)

You can use the following query statements in this example:

-   Count the average latency and request counts of frontend service HTTP  GET/dispatch operations every minute.

    ```
    process.serviceName: "frontend" and operationName: "HTTP GET /dispatch" |
    select from_unixtime( __time__ - __time__ % 60) as time,
    truncate(avg(duration)/1000/1000) as avg_duration_ms,
    count(1) as count
    group by __time__ - __time__ % 60 order by time desc limit 60
    ```

-   Compare the time consumed by the operations of two traces.

    ```
    traceID: "trace1" or traceID: "trace2" |
    select operationName,
    (max(duration)-min(duration))/1000/1000 as duration_diff_ms
    group by operationName
    order by duration_diff_ms desc
    ```

-   Count the IP addresses of the traces whose latencies are more than 1.5 seconds.

    ```
    process.serviceName: "frontend" and operationName: "HTTP GET /dispatch" and duration > 1500000000 |
    select "process.tags.ip" as IP,
    truncate(avg(duration)/1000/1000) as avg_duration_ms,
    count(1) as count
    group by "process.tags.ip"
    ```


