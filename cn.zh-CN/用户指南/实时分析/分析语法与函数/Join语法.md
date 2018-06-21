# Join语法 {#reference_fz2_qmq_zdb .reference}

Join用于多表中字段之间的联系。日志服务除了支持单个Logstore的Join之外，还支持Logstore和RDS的Join，以及Logstore和Logstore的Join。本文档为您介绍如何使用跨Logstore的Join功能。

## 操作步骤 {#section_fbl_dmv_tdb .section}

1.  [下载](http://github.com/aliyun/aliyun-log-python-sdk)最新版本Python SDk。
2.  使用GetProjectLogs接口进行查询。

## SDK示例 {#section_ujs_2mv_tdb .section}

```
#!/usr/bin/env python
#encoding: utf-8
import time,sys,os
from aliyun.log.logexception import LogException
from aliyun.log.logitem import LogItem
from aliyun.log.logclient import LogClient
from aliyun.log.getlogsrequest import GetLogsRequest
from aliyun.log.getlogsrequest import GetProjectLogsRequest
from aliyun.log.putlogsrequest import PutLogsRequest
from aliyun.log.listtopicsrequest import ListTopicsRequest
from aliyun.log.listlogstoresrequest import ListLogstoresRequest
from aliyun.log.gethistogramsrequest import GetHistogramsRequest
from aliyun.log.index_config import *
from aliyun.log.logtail_config_detail import *
from aliyun.log.machine_group_detail import *
from aliyun.log.acl_config import *
if __name__=='__main__':
    token = None
    endpoint = "http://cn-hangzhou.log.aliyuncs.com"
    accessKeyId  = 'LTAIvKy7U'
    accessKey='6gXLNTLyCfdsfwrewrfhdskfdsfuiwu'
    client = LogClient(endpoint, accessKeyId, accessKey,token)
    logstore = "meta"
    # 在查询语句中，指定两个Logstore，每个Logstore要分别指定各自的时间范围，以及两个Logstore关联的key
    req = GetProjectLogsRequest(project,"select count(1) from sls_operation_log  s join  meta m on s.__date__ >'2018-04-10 00:00:00' and s.__date__ < '2018-04-11 00:00:00' and m.__date__ >'2018-04-23 00:00:00' and m.__date__ <'2018-04-24 00:00:00' and s.projectid = cast(m.ikey as varchar)");
    res = client.get_project_logs(req)
    res.log_print();
    exit(0)
```

