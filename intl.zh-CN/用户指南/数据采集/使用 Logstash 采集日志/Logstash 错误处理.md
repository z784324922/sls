# Logstash 错误处理 {#concept_qvr_lq5_vdb .concept}

配置Logstash采集日志数据后，如果在日志采集过程中遇到错误，可以按照报错类型选择对应处理方式。

通过Logstash收集日志数据时，如遇到以下收集错误，请按照对应建议进行处理。

-   日志服务查看到数据乱码

    Logstash 默认支持 UTF8 格式文件编码，请确认输入文件编码是否正确。

-   控制台提示错误

    控制台提示错误`io/console not supported; tty will not be manipulated`时，不影响产品功能，请忽略。

    其它错误类型建议建议参考Google或Logstash论坛，查询帮助信息。


