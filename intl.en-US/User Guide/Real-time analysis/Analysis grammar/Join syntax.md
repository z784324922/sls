# Join syntax {#reference_fz2_qmq_zdb .reference}

Join is used for combining fields from multiple tables. Besides Join for a single Logstore, Log Service also supports Join for Logstore and RDS, and for several Logstores. This document describes how to use the Join function between Logstores.

## Procedure {#section_fbl_dmv_tdb .section}

1.  [Download](http://github.com/aliyun/aliyun-log-python-sdk) the latest version of Python SDK.
2.  Use the GetProjectLogs interface for query.

## SDK sample {#section_ujs_2mv_tdb .section}

```
/usr/bin/env python
#encoding: utf-8
import time,sys,os
From aliyun. log. logexception import logexception
from aliyun.log.logitem import LogItem
from aliyun.log.logclient import LogClient
From aliyun. log. getlogsrequest import getlogsrequest
from aliyun.log.getprojectlogsrequest import GetProjectLogsRequest
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
    accessKeyId = 'LTAIvKy7U'
    accessKey='6gXLNTLyCfdsfwrewrfhdskfdsfuiwu'
    client = LogClient(endpoint, accessKeyId, accessKey,token)
    logstore = "meta"
    # In the query statement, specify two Logstores. For each Logstore specify its time range and the key
    req = GetProjectLogsRequest(project,"select count(1) from sls_operation_log s join meta m on s.__date__ >'2018-04-10 00:00:00' and s.__date__ < '2018-04-11 00:00:00' and m.__date__ >'2018-04-23 00:00:00' and m.__date__ <'2018-04-24 00:00:00' and s.projectid = cast(m.ikey as varchar)");
    Res = client. Fig (req)
    res.log_print();
    exit(0)
```

