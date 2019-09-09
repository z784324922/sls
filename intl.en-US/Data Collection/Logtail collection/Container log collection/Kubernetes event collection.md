# Kubernetes event collection {#concept_g4y_nky_ngb .concept}

This topic describes how to use the eventer component to collect events from Kubernetes to Log Service. For more information about the source code for Kubernetes event collection, visit [GitHub](https://github.com/AliyunContainerService/heapster/tree/master/events).

## Collection configuration method {#section_c1v_tky_ngb .section}

**Note:** 

-   If you use Alibaba Cloud Kubernetes, you only need to configure the endpoint, project, and logStore parameters.
-   If you use self-built Kubernetes, you need to configure the endpoint, project, logStore, regionId, internal, accessKeyId, and accessKeySecret parameters.

Deploy the YAML template as follows:

```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: eventer-sls
  namespace: kube-system
spec:
  replicas: 1
  template:
    metadata:
      labels:
        task: monitoring
        k8s-app: eventer-sls
      annotations:
        scheduler.alpha.kubernetes.io/critical-pod: ''
    spec:
      serviceAccount: admin
      containers:
      - name: eventer-sls
        image: registry.cn-hangzhou.aliyuncs.com/ringtail/eventer:v1.6.1.3
        imagePullPolicy: IfNotPresent
        command:
        - /eventer
        - --source=kubernetes:https://kubernetes.default
        - --sink=sls:https://${endpoint}? project=${project}&logStore=${logstore}
```

## Parameter settings {#section_off_fly_ngb .section}

|Parameter|Type|Required|Description|
|:--------|:---|:-------|:----------|
|endpoint|String|Yes|The endpoint of Log Service. For more information, see [Service endpoint](../../../../reseller.en-US/API Reference/Service endpoint.md).|
|project|String|Yes|The project in Log Service.|
|logStore|String|Yes|The Logstore in Log Service.|
|internal|String|Yes if you use self-built Kubernetes, and no if you use Alibaba Cloud Kubernetes|Specifies whether Alibaba Cloud Kubernetes is used. Set this parameter to false.|
|regionId|String|Yes if you use self-built Kubernetes, and no if you use Alibaba Cloud Kubernetes|The region ID of Log Service. For more information, see [Service endpoint](../../../../reseller.en-US/API Reference/Service endpoint.md).|
|accessKeyId|String|Yes if you use self-built Kubernetes, and no if you use Alibaba Cloud Kubernetes|The AccessKey ID of your account. We recommend that you use the AccessKey ID of a RAM user.|
|accessKeySecret|String|Yes if you use self-built Kubernetes, and no if you use Alibaba Cloud Kubernetes|The AccessKey Secret of your account. We recommend that you use the AccessKey Secret of a RAM user.|

## Log sample {#section_czl_kpy_ngb .section}

The following log sample is collected by Log Service. For more information, see [Kubernetes](https://kubernetes.io/docs/reference/federation/v1/definitions/#_v1_event).

The following table describes the content of the log.

|Log field|Type|Description|
|:--------|:---|:----------|
|hostname|String|The hostname of the event.|
|level|String|The level of the log. Valid values: Normal and Warning.|
|pod\_id|String|The unique identifier of the pod. This field is available only when the event type is related to the pod.|
|pod\_name|String|The name of the pod. This field is available only when the event type is related to the pod.|
|eventId|JSON|The details of the event, which is a JSON string.|

```
hostname:  cn-hangzhou.i-***********"
level:  Normal
pod_id:  2a360760-1c82-11e9-9ddf-00163e0c7cbe
pod_name:  logtail-ds-blkkr
event_id:  {  
   "metadata":{  
      "name":"logtail-ds-blkkr. 157b7cc90de7e192",
      "namespace":"kube-system",
      "selfLink":"/api/v1/namespaces/kube-system/events/logtail-ds-blkkr. 157b7cc90de7e192",
      "uid":"2aaf75ab-1c82-11e9-9ddf-00163e0c7cbe",
      "resourceVersion":"6129169",
      "creationTimestamp":"2019-01-20T07:08:19Z"
   },
   "involvedObject":{  
      "kind":"Pod",
      "namespace":"kube-system",
      "name":"logtail-ds-blkkr",
      "uid":"2a360760-1c82-11e9-9ddf-00163e0c7cbe",
      "apiVersion":"v1",
      "resourceVersion":"6129161",
      "fieldPath":"spec.containers{logtail}"
   },
   "reason":"Started",
   "message":"Started container",
   "source":{  
      "component":"kubelet",
      "host":"cn-hangzhou.i-***********"
   },
   "firstTimestamp":"2019-01-20T07:08:19Z",
   "lastTimestamp":"2019-01-20T07:08:19Z",
   "count":1,
   "type":"Normal",
   "eventTime":null,
   "reportingComponent":"",
   "reportingInstance":""
}
```

