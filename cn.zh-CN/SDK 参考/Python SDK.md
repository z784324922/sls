# Python SDK {#reference_l5b_225_xdb .reference}

## 下载地址 {#section_gx3_35f_jdb .section}

该 SDK GitHub 地址如下：

[https://github.com/aliyun/aliyun-log-python-sdk](https://github.com/aliyun/aliyun-log-python-sdk)

## 操作步骤 {#section_ylg_r1r_12b .section}

为快速开始使用 Log Service Python SDK，请按照如下步骤进行。

## 步骤 1 创建阿里云账号 {#section_zlg_r1r_12b .section}

具体方法请参考 [阿里云账号注册流程](https://help.aliyun.com/document_detail/37195.html)。

为了更好地使用阿里云服务，建议尽快完成实名认证，否则部分阿里云服务将无法使用。

## 步骤 2 获取阿里云访问密钥 {#section_amg_r1r_12b .section}

为了使用 Log Service Python SDK，您必须申请阿里云的 [访问秘钥](../../../../../cn.zh-CN/API 参考/访问秘钥.md)。

登录阿里云[秘钥管理页面](https://ak-console.aliyun.com/#/accesskey) 。选择一对用于 SDK 的访问密钥对。如果没有，请创建一对新访问密钥，且保证它处于**启用**状态。有关如何创建访问密钥，参见 [准备流程](../../../../../cn.zh-CN/用户指南/准备工作/准备流程.md)。

该密钥对会在下面的步骤使用，且需要保管好，不能对外泄露。另外，您可以参考 [配置](cn.zh-CN/SDK 参考/基本介绍/配置.md) 了解更多 SDK 如何使用访问密钥的信息。

## 步骤 3 创建日志服务项目和日志库 {#section_zrq_dbr_12b .section}

在使用日志服务Python SDK之前，请先在控制台上创建好项目（Project）和日志库（Logstore）。

在进行SDK操作之前，请先在控制台上创建Project和Logstore。

1.  登录日志服务管理控制台。
2.  单击右上角的**创建Project**。
3.  填写**Project名称**和**所属地域**，单击**确认**。
4.  在Project列表页面，单击项目的名称，然后单击**创建** [创建日志库](https://help.aliyun.com/document_detail/48990.html)。

    或者在创建完项目后，根据系统提示创建日志库，单击**创建**。

5.  填写日志库的配置信息并单击**确认**。

    您需要在此处填写Logstore名称、数据保存时间、Shard数目等。请按照您的需求合理设置[Shard](https://help.aliyun.com/document_detail/28976.html)数目，例如本文实例中，您需要设置4个Shard。


**说明：** 

-   请确保使用同一阿里云账号获取阿里云访问密钥和创建日志项目及日志库。
-   关于日志的项目、日志库等概念请参考 Log [基本概念](../../../../../cn.zh-CN/产品简介/基本概念.md)。
-   Log 的 Project 名称为日志服务全局唯一，而 Logstore 名称在一个 Project 下面唯一。
-   Log 的 Project 一旦创建则无法更改它的所属区域。目前也不支持在不同阿里云 Region 间迁移 Log Project。

## 步骤 4 安装 Python 环境 {#section_a1t_j2j_sy .section}

Python SDK是一个纯Python的库，支持在所有运行Python的操作系统。目前主要为Linux、Mac OS X和Windows。请如下安装Python：

1.  下载并安装最新的Python [安装包](https://www.python.org/downloads/)。

    **说明：** 

    -   目前，Python SDK支持Python 2.6/2.7和Python 3.3/3.4/3.5/3.6环境。您可以运行`python -V`查询当前的Python运行版本。
    -   Python官方不再支持Python 2.6和Python 3.3，推荐您使用Python 2.7、Python 3.4及以后版本。
2.  下载并安装Python的包管理工具[Pip](https://pip.pypa.io/en/latest/installing/)。

    完成pip安装后，你可以运行 `pip -V` 确认pip是否安装成功，并查看当前pip版本。


## 步骤 5 安装Python SDK {#section_bqt_42j_sy .section}

Shell下以管理员权限执以下命令，安装Python SDK。

```
pip install -U aliyun-log-python-sdk
```

## 步骤 6 开始一个 Python 程序 {#section_kbx_x2j_sy .section}

现在，你可以开始使用 SDK Python SDK。使用任何文本编辑器或者 Python IDE，运行如下示例代码即可与服务端交互并得到相应输出。

更多信息请参考[Github 项目地址以及更多详细说明](http://aliyun-log-python-sdk.readthedocs.io/)。

```
# encoding: utf-8
import time
from aliyun.log import *


def main():
    endpoint = ''  # 选择与上面步骤创建Project所属区域匹配的Endpoint
    access_key_id = ''  # 使用您的阿里云访问密钥AccessKeyId
    access_key = ''  # 使用您的阿里云访问密钥AccessKeySecret
    project = ''  # 上面步骤创建的项目名称
    logstore = ''  # 上面步骤创建的日志库名称

    # 重要提示：创建的logstore请配置为4个shard以便于后面测试通过
    # 构建一个client
    client = LogClient(endpoint, access_key_id, access_key)
    # list 所有的logstore
    req1 = ListLogstoresRequest(project)
    res1 = client.list_logstores(req1)
    res1.log_print()
    topic = ""
    source = ""

    # 发送10个数据包，每个数据包有10条log
    for i in range(10):
        logitemList = []  # LogItem list
        for j in range(10):
            contents = [('index', str(i * 10 + j))]
            logItem = LogItem()
            logItem.set_time(int(time.time()))
            logItem.set_contents(contents)
            logitemList.append(logItem)
        req2 = PutLogsRequest(project, logstore, topic, source, logitemList)
        res2 = client.put_logs(req2)
        res2.log_print()

    # list所有的shard，读取上1分钟写入的数据全部读取出来
    listShardRes = client.list_shards(project, logstore)
    for shard in listShardRes.get_shards_info():
        shard_id = shard["shardID"]
        start_time = int(time.time() - 60)
        end_time = start_time + 60
        res = client.get_cursor(project, logstore, shard_id, start_time)
        res.log_print()
        start_cursor = res.get_cursor()
        res = client.get_cursor(project, logstore, shard_id, end_time)
        end_cursor = res.get_cursor()
        while True:
            loggroup_count = 100  # 每次读取100个包
            res = client.pull_logs(project, logstore, shard_id, start_cursor, loggroup_count, end_cursor)
            res.log_print()
            next_cursor = res.get_next_cursor()
            if next_cursor == start_cursor:
                break
            start_cursor = next_cursor

    # 重要提示： 只有打开索引功能，才可以使用以下接口来查询数据
    time.sleep(60)
    topic = ""
    query = "index"
    From = int(time.time()) - 600
    To = int(time.time())
    res3 = None

    # 查询最近10分钟内，满足query条件的日志条数，如果执行结果不是完全正确，则进行重试
    while (res3 is None) or (not res3.is_completed()):
        req3 = GetHistogramsRequest(project, logstore, From, To, topic, query)
        res3 = client.get_histograms(req3)
    res3.log_print()

    # 获取满足query的日志条数
    total_log_count = res3.get_total_count()
    log_line = 10

    # 每次读取10条日志，将日志数据查询完，对于每一次查询，如果查询结果不是完全准确，则重试3次
    for offset in range(0, total_log_count, log_line):
        res = None
        for retry_time in range(0, 3):
            req4 = GetLogsRequest(project, logstore, From, To, topic, query, log_line, offset, False)
            res = client.get_logs(req4)
            if res is not None and res.is_completed():
                break
            time.sleep(1)
        if res is not None:
            res.log_print()
    listShardRes = client.list_shards(project, logstore)
    shard = listShardRes.get_shards_info()[0]

    # 分裂shard
    if shard["status"] == "readwrite":
        shard_id = shard["shardID"]
        inclusiveBeginKey = shard["inclusiveBeginKey"]
        midKey = inclusiveBeginKey[:-1] + str((int(inclusiveBeginKey[-1:])) + 1)
        client.split_shard(project, logstore, shard_id, midKey)

    # 合并shard
    shard = listShardRes.get_shards_info()[1]
    if shard["status"] == "readwrite":
        shard_id = shard["shardID"]
        client.merge_shard(project, logstore, shard_id)

    # 删除shard
    shard = listShardRes.get_shards_info()[-1]
    if shard["status"] == "readonly":
        shard_id = shard["shardID"]
        client.delete_shard(project, logstore, shard_id)

    # 创建外部数据源
    res = client.create_external_store(project,
                                       ExternalStoreConfig("rds_store", "cn-qingdao", "rds-vpc", "vpc-************",
                                                           "i***********", "*.*.*.*", "3306", "root",
                                                           "sfdsfldsfksflsdfs", "meta", "join_meta"))
    res.log_print()
    res = client.update_external_store(project, ExternalStoreConfig("rds_store", "cn-qingdao", "rds-vpc",
                                                                    "vpc-************", "i************", "*.*.*.*",
                                                                    "3306", "root", "sfdsfldsfksflsdfs", "meta",
                                                                    "join_meta"))
    res.log_print()
    res = client.get_external_store(project, "rds_store")
    res.log_print()
    res = client.list_external_store(project, "")
    res.log_print()
    res = client.delete_external_store(project, "rds_store")
    res.log_print()

    # 使用python sdk进行查询分析
    req4 = GetLogsRequest(project, logstore, From, To, topic, "* | select count(1)", 10, 0, False)
    res = client.get_logs(req4)
    res.log_print()

    # 使用python sdk进行join rds查询
    req4 = GetLogsRequest(project, logstore, From, To, topic,
                          "* | select count(1) from " + logstore + "  l  join  rds_store  r on  l.ikey =r.ekey", 10, 0,
                          False)
    res = client.get_logs(req4)
    res.log_print()

    # 使用python sdk把查询结果写入rds
    req4 = GetLogsRequest(project, logstore, From, To, topic, "* | insert into rds_store select count(1) ", 10, 0,
                          False)
    res = client.get_logs(req4)
    res.log_print()


if __name__ == '__main__':
    main()
```

