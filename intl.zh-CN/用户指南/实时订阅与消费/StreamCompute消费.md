# StreamCompute消费 {#concept_g4n_24q_zdb .concept}

StreamCompute 在创建Loghub类型数据源后，可以直接消费Loghub中数据，配置如下：

```
CREATE STREAM TABLE source_test_galaxy ( $schema ) WITH ( type='loghub', 
endpoint=$endpoint, accessId=$loghub_access_id, accessKey=$loghub_access_key, projectName=$project, logstore=$logstore );
```

|参数|参数说明|
|:-|:---|
| `$schema` |将日志中哪些Key映射成StreamCompute表中的Column，如：`name STRING, age STRING, id STRING`。|
| `$endpoint` |数据访问接入点，各Region接入点请查看[服务入口](../../../../intl.zh-CN/API 参考/服务入口.md)。|
| `$loghub_access_id` |有读权限账号（或子账号）对应AccessId。|
| `$loghub_access_key` |有读权限账号（或子账号）对应AccessKey。|
| `$project` |数据所在Project。|
| `$logstore` |数据所在Logstore。|

**示例：**

```
CREATE STREAM TABLE source_test_galaxy ( name STRING, age STRING, id STRING ) WITH ( type='loghub', endpoint='http://cn-hangzhou-intranet.log.aliyuncs.com', accessId='mock_access_id', accessKey='mock_access_key', projectName='ali-cloud-streamtest', logstore='stream-test' );
```

