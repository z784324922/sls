# Kubernetes事件采集 {#concept_g4y_nky_ngb .concept}

本文档主要介绍如何使用eventer将Kubernetes中的事件采集到日志服务。Kubernetes事件采集相关源码如下：[GitHub](https://github.com/AliyunContainerService/heapster/tree/master/events) 。

## 采集配置方式 {#section_c1v_tky_ngb .section}

**说明：** 

-   如果当前使用的是阿里云Kubernetes，只需配置endpoint、project和logStore即可。
-   如果当前使用的是自建Kubernetes，除参数endpoint、project、logStore之外，还需要额外配置参数regionId、internal、accessKeyId和accessKeySecret。

部署yaml模板如下：

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
        - --sink=sls:https://${endpoint}?project=${project}&logStore=${logstore}
```

## 参数配置 {#section_off_fly_ngb .section}

|配置项|类型|是否必选|说明|
|:--|:-|:---|:-|
|endpoint|string|必选|日志服务的endpoint，请参考[服务入口](../../../../../cn.zh-CN/API 参考/服务入口.md)。|
|project|string|必选|日志服务Project。|
|logStore|string|必选|日志服务Logstore。|
|internel|string|自建Kubernetes用户必选。阿里云Kubernetes用户无需填写。|请设置为false。|
|regionId|string|自建Kubernetes用户必选。阿里云Kubernetes用户无需填写。|日志服务所在Region的ID，请参考[服务入口](../../../../../cn.zh-CN/API 参考/服务入口.md)。|
|accessKeyId|string|自建Kubernetes用户必选。阿里云Kubernetes用户无需填写。|AccessKey ID，建议使用子账号AK信息。|
|accessKeySecret|string|自建Kubernetes用户必选。阿里云Kubernetes用户无需填写。|AccessKey Secret，建议使用子账号AK信息。|

## 日志样例 {#section_czl_kpy_ngb .section}

下述为采集到日志服务的日志样例，详细信息请参考[Kubernetes](https://kubernetes.io/docs/reference/federation/v1/definitions/#_v1_event)。

日志中的内容包括：

|日志字段|类型|说明|
|:---|:-|:-|
|hostname|string|事件发生所在的主机名。|
|level|string|日志等级，包括Normal、Warning。|
|pod\_id|string|Pod的唯一标识，仅在该事件类型和Pod相关时才具有此字段。|
|pod\_name|string|Pod名，仅在该事件类型和Pod相关时才具有此字段。|
|eventId|json|该字段为json类型的string，为事件的详细内容。|

```
hostname:  cn-hangzhou.i-***********"
level:  Normal
pod_id:  2a360760-1c82-11e9-9ddf-00163e0c7cbe
pod_name:  logtail-ds-blkkr
event_id:  {  
   "metadata":{  
      "name":"logtail-ds-blkkr.157b7cc90de7e192",
      "namespace":"kube-system",
      "selfLink":"/api/v1/namespaces/kube-system/events/logtail-ds-blkkr.157b7cc90de7e192",
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

