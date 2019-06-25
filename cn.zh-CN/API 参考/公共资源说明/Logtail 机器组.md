# Logtail 机器组 {#reference_oqd_t5q_12b .reference}

机器：当机器安装 Logtail 并正常启动后，会根据 Logtail 配置中的用户信息自动关联到当前用户。目前 machine 有三种标示的方式，分别为：

-   IP：hostname 对应 IP 地址。最容易理解，但在 VPC 等环境下可能会有重复。
-   UUID \(machine-uniqueid\)：DMI 设备中 UUID，参见 [RFC4122](http://www.ietf.org/rfc/rfc4122.txt)。
-   Userdefined-id：用户在 Logtail 目录下自定义该机器标识。

每台机器属性如下：

``` {#codeblock_14d_oto_ec2}
{
    "ip" : "testip1",
    "machine-uniqueid" : "testuuid1",
    "userdefined-id" : "testuserdefinedid1",
    "lastHeartbeatTime":1397781420
 }
```

机器属性参数如下：

|参数名称|类型|描述|
|:---|:-|:-|
|ip|string|机器 hostname 对应 IP 地址|
|uuid|string|机器标示的唯一主键，由 Logtail 上传|
|userdefined-id|string|用户自定义机器标示，由 Logtail 上传|
|lastHeartbeatTime\(output-only\)|integer|机器的最后心跳时间（从 epoch 时间开始的秒数）|

## machinegroup {#section_lyj_k4f_sy .section}

Project 下当前用户拥有的机器分组。机器分组可以通过两种方式来标示（IP 与 userdefined）。IP 较为容易辨识，userdefined 可以解决 VPC 下 IP 相同的问题，用户可以选任意一种方式进行机器标识。

machinegroup 命名规范：

-   只能包括小写字母、数字、连字符（-）和下划线（\_）
-   必须以小写字母或者数字开头和结尾
-   长度必须在 2~128 字节以内

## 完整资源示例 {#section_ktz_l4f_sy .section}

``` {#codeblock_po5_4hp_n3g}
{
    "groupName" : "testgroup",
    "groupType" : "",
    "groupAttribute" : {
        "externalName" : "testgroup",
        “groupTopic": "testgrouptopic"
    },
    "machineIdentifyType": "ip",
    "machineList" : [
        "ip1",
        "ip2"
        …
    ],
    "createTime": 1431705075,
    "lastModifyTime" : 1431705075
}
```

|属性名称|类型|必须|描述|
|:---|:-|:-|:-|
|groupName|string|是|机器分组名称， Project 下唯一|
|groupType|string|否|机器分组类型，默认为空|
|machineIdentifyType|string|是|机器标识类型，分为 IP 和 userdefined 两种|
|groupAttribute|object|是|机器分组的属性，默认为空|
|machineList|array|是|具体的机器标识，可以是 IP 或 userdefined-id|
|createTime\(output-only\)|int|否|该资源创建时间|
|lastModifyTime\(output-only\)|int|否|该资源服务端更新时间|

groupAttribute 说明如下：

|属性名称|类型|是否必须|描述|
|:---|:-|:---|:-|
|groupTopic|string|否|机器分组的 Topic，默认为空|
|externalName|string|否|机器分组所依赖的外部管理标识，默认为空|

