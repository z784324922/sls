# Docker事件输入源 {#concept_lck_225_ngb .concept}

Logtail除支持采集容器标准输出、容器文件外，还支持采集Docker Engine的事件信息。Docker Engine的事件信息记录了容器、镜像、插件、网络、存储等的所有交互事件，一般用于系统监控、问题排查、审计、安全防护等场景。

## 限制说明 {#section_alf_g25_ngb .section}

-   Logtail支持版本为0.16.18及以上
-   Logtail可运行在容器模式或在宿主机上运行，需具备访问DockerEngine（可以访问到`/var/run/docker.sock`）。

    Logtail采集Kubernetes日志请参考[Kubernetes日志采集流程](cn.zh-CN/用户指南/Logtail采集/容器日志采集/Kubernetes日志采集流程.md)，采集标准容器日志参考[标准Docker日志采集流程](cn.zh-CN/用户指南/Logtail采集/容器日志采集/标准Docker日志采集流程.md)。

-   Logtail在重启或停止期间，无法采集容器事件。

## 应用场景 {#section_yyk_h25_ngb .section}

-   监控所有容器的启停事件，当核心容器停止后立即告警。
-   采集所有容器的事件，用于审计以及安全分析。
-   监控所有镜像的拉取事件，若拉取非合法路径的镜像时立即告警。
-   采集所有容器的事件，用于Kubernetes、Docker Engine的问题排查。

## 配置项 {#section_mzf_325_ngb .section}

该输入源类型为: `service_docker_event`。

**说明：** Logtail支持对采集的数据进行处理加工后上传，处理方式参见[处理采集数据](cn.zh-CN/用户指南/Logtail采集/自定义插件/处理采集数据.md)。

|配置项|类型|是否必须|说明|
|:--|:-|:---|:-|
|EventQueueSize|int|否|事件缓冲队列大小，默认为10，无特殊需求请保持默认设置。|

## 元数据 {#section_clx_m25_ngb .section}

Docker事件的日志字段如下，详细信息请参考[Docker官方文档](https://docs.docker.com/engine/reference/commandline/events/)。

|字段|说明|
|:-|:-|
|\_type\_|资源类型，例如 container、image等。|
|\_action\_|操作类型，例如destroy、status等。|
|\_id\_|事件唯一标识。|
|\_time\_nano|Nano类型的事件时间戳。|

## 示例 {#section_esg_mjy_ngb .section}

采集配置：

```
{
  "inputs": [
    {
      "detail": {},
      "type": "service_docker_event"
    }
  ]
}
```

日志样例：

-   **样例1**：Image拉取事件

    ```
    __source__:  172.16.2.24
    __tag__:__hostname__:  logtail-ds-77brr
    __topic__:  
    _action_:  pull
    _id_:  registry.cn-hangzhou.aliyuncs.com/ringtail/eventer:v1.6.1.3
    _time_nano_:  1547910184047414271
    _type_:  image
    name:  registry.cn-hangzhou.aliyuncs.com/ringtail/eventer
    ```

-   **样例2**：Kubernetes中容器的销毁事件

    ```
    __source__:  172.16.2.30
    __tag__:__hostname__:  logtail-ds-xnvz2
    __topic__:  
    _action_:  destroy
    _id_:  af61340b0ac19e6f5f32be672d81a33fc4d3d247bf7dbd4d3b2c030b8bec4a03
    _time_nano_:  1547968139380572119
    _type_:  container
    annotation.kubernetes.io/config.seen:  2019-01-20T15:03:03.114145184+08:00
    annotation.kubernetes.io/config.source:  api
    annotation.scheduler.alpha.kubernetes.io/critical-pod:  
    controller-revision-hash:  2630731929
    image:  registry-vpc.cn-hangzhou.aliyuncs.com/acs/pause-amd64:3.0
    io.kubernetes.container.name:  POD
    io.kubernetes.docker.type:  podsandbox
    io.kubernetes.pod.name:  logtail-ds-44jbg
    io.kubernetes.pod.namespace:  kube-system
    io.kubernetes.pod.uid:  6ddcf598-1c81-11e9-9ddf-00163e0c7cbe
    k8s-app:  logtail-ds
    kubernetes.io/cluster-service:  true
    name:  k8s_POD_logtail-ds-44jbg_kube-system_6ddcf598-1c81-11e9-9ddf-00163e0c7cbe_0
    pod-template-generation:  9
    version:  v1.0
    ```


