# 获取Logstore数据（维表）错误 {#concept_2070600 .concept}

如果加工规则中涉及其他Logstore资源的加载，则有可能会产生资源的加载或刷新错误。本文档主要介绍从其他Logstore获取数据的常见错误以及排查处理方法。

在成功读取源Logstore数据后，加工引擎开始对源Logstore的日志事件进行加工。如果加工规则中涉及OSS、RDS、Logstore等外联资源的加载，则也有可能会产生资源的加载或刷新错误。

错误影响：

-   在日志事件加工阶段，与加工规则冲突的日志事件会引发报错，并被丢弃，加工后的输出结果中将不包含这些日志事件。
-   加工任务会丢弃与加工规则冲突的日志事件，并继续加工其他的日志事件，不会重试。
-   如果多条事件分裂自同一条事件，只要其中有一条事件出错被丢弃，所有和该事件分裂自同一条源事件的其他事件也都会被丢弃。

    **说明：** 在输出之前，这些事件都是以树结构互相关联，并不是互相独立。


## 缺少必填的参数 {#section_uc4_o3t_1kl .section}

-   加工规则

    ``` {#codeblock_hki_ssi_55r}
    e_table_map(res_log_logstore_pull(endpoint="cn-shenzhen.log.aliyuncs.com",ak_id="xxx",
            ak_secret="xxx",project="etl-test-shenzhen",
            fields=["__source__"]),field="processid",output_fields=["__source__"])
    ```

-   错误日志

    ``` {#codeblock_0oa_7xe_och}
    error when calling : res_log_logstore_pull\nDetail: res_log_logstore_pull() missing 1 required positional argument: 'logstore'", "requestId": "
    ```

-   排查方法

    缺少了必要的参数`logstore`，出现`missing 1 required positional argument`错误信息的时候，表示缺少了必要的参数，请排查是否出现参数未填写的报错。

-   解决方案

    重新配置编排语法，配置缺少的参数。


## 设置主键维护但未设置delete\_data参数 {#section_g1f_i81_7yr .section}

-   加工规则

    ``` {#codeblock_rf7_nq2_8s2}
    e_table_map(res_log_logstore_pull(endpoint="xx",ak_id="xxx",
            ak_secret="xxx",project="etl-test-shenzhen",logstore="rds-mysql-test",
            fields=["__source__"],primary_keys="cid"),field="processid",output_fields=["__source__"])
    ```

-   错误日志

    ``` {#codeblock_roq_25y_4hi}
    errorMessage\": \"when setting parameter primart_keys,need set delete_data\\nDetail: None\
    ```

-   排查方法

    出现这个错误是因为当设置主键维护参数`primary_keys`的时候，`delete_data`参数没有设置。`primary_keys`参数和`delete_data`参数必须同时进行配置。

-   解决方案

    primary\_keys和delete\_data参数都要进行配置，配置后重新启动数据加工服务。


## 目标Logstore不存在 {#section_oj7_s39_5in .section}

-   加工规则

    ``` {#codeblock_saa_363_0uo}
    e_table_map(res_log_logstore_pull(endpoint="xx",ak_id="xxx",
            ak_secret="xxx",project="etl-test-shenzhen",logstore="pull_logstore_test9900881",
            fields=["__source__"],primary_keys="cid"),field="processid",output_fields=["__source__"])
    ```

-   错误日志

    ``` {#codeblock_20o_k5z_407}
    message:  fetch data get errors:{"errorCode": "LogStoreNotExist", "errorMessage": "logstore pull_logstore_test9900881 does not exist", "requestId": "5D7227AA269948500404B777"},retrytimes=2
    ```

-   排查方法

    当配置的目标Logstore不存在的时候就会出现上述错误日志。

-   解决方案

    检查目标Logstore 是否存在，重新配置编排语法，填入正确的Logstore的名称。


## Ak配置错误 {#section_tc2_npt_t7b .section}

-   加工规则

    ``` {#codeblock_sbl_nju_vs8}
    e_table_map(res_log_logstore_pull(
            endpoint="cn-hangzhou.log.aliyuncs.com",
            ak_id="xx",
            ak_secret="xx",
            project="sls-test",
            logstore="pull_logstore_test",
            fields=[("id", "new_id"), "name", "status"],
            from_time="begin"), "name", "new_id")
    ```

-   错误日志

    ``` {#codeblock_kzw_n2l_jq5}
    message:  fetch data get errors:{"errorCode": "SignatureNotMatch", "errorMessage": "signature gdaL/nWSRtve5FOB+QqHO/sBdnA= not match", "requestId": "5D760261ED35D40AA4AB1953"},retrytimes=1
    ```

    ``` {#codeblock_d8o_ncy_plb}
    message:  fetch data get errors:{"errorCode": "Unauthorized", "errorMessage": "AccessKeyId not found: xx", "requestId": "5D7602A01808F9EAA6EB0E2B"},retrytimes=3
    ```

-   排查方法

    出现上述的错误时，请检查编排语法中ak参数是否正确，如果ak\_serect和ak\_id值不匹配，或者ak\_id不存在就会报上面的错误。

-   解决方案

    重新配置正确的ak参数并且重新启动数据加工任务。


## Ak权限不足 {#section_npb_djc_p0f .section}

-   加工规则

    ``` {#codeblock_spf_d8u_sou}
    e_table_map(res_log_logstore_pull(
            endpoint="cn-hangzhou.log.aliyuncs.com",
            ak_id="xx",
            ak_secret="xx",
            project="sls-test",
            logstore="pull_logstore_test",
            fields=[("id", "new_id"), "name", "status"],
            from_time="begin"), "name", "new_id")
    ```

-   错误日志

    ``` {#codeblock_php_vs1_ox0}
    message:  fetch data get errors:{"errorCode": "Unauthorized", "errorMessage": "denied by sts or ram, action: log:ListShards, resource: acs:log:cn-hangzhou:1654218965343050:...}
    ```

-   排查方法

    出现`Unauthorized`的错误信息一般情况是权限不足，可以检查一下配置的ak是否具有目标Logstore的读写权限。

-   解决方案

    检查ak权限，如果权限不足，请开通所需要的权限。


## Endpoint填写错误 {#section_89a_3jt_914 .section}

-   加工规则

    ``` {#codeblock_0wt_4fu_xm5}
    e_table_map(res_log_logstore_pull(
            endpoint="xxx",
            ak_id="xx",
            ak_secret="xx",
            project="sls-test",
            logstore="pull_logstore_test",
            fields=[("id", "new_id"), "name", "status"],
            from_time="begin"), "name", "new_id")
    ```

-   错误日志

    ``` {#codeblock_wan_ral_q13}
    message:  fetch data get errors:{"errorCode": "ProjectNotExist", "errorMessage": "The Project does not exist : ali-sls-etl-regression-test", "requestId": "5D760AB12ECD0722AA1DD681"}
    ```

    ``` {#codeblock_2ra_uh5_qzo}
    message:  fetch data get errors:{"errorCode": "LogRequestError", "errorMessage": "HTTPConnectionPool(host='ali-sls-etl-regression-test.xx', port=80): Max retries exceeded with url: /logstores/pull_logstore_test/shards (Caused by NewConnectionError('<urllib3.connection.HTTPConnection object at 0x7f968a298ef0>: Failed to establish a new connection: [Errno -2] Name or service not known
    ```

-   排查方法

    出现`ProjectNotExist`或者`LogRequestError`错误，一般情况下是Endpoint填写错误，导致链接错误，或者链接上了但是发现Endpoint下没有需要的Project 。

-   解决方案

    Endpoint配置正确的服务入口。请参见[服务入口](../../../../cn.zh-CN/API 参考/服务入口.md#)。


