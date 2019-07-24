# OSS访问日志简介 {#concept_asf_lt1_ygb .concept}

用户在访问OSS的过程中，会产生大量的访问日志。OSS日志存储功能，可将OSS的访问日志，以小时为单位，按照固定的命名规则，生成一个Object写入您指定的Bucket（目标 Bucket，Target Bucket）。

## 开启OSS日志存储 {#section_lxp_1w1_ygb .section}

1.  登录[OSS 管理控制台](https://oss.console.aliyun.com/)。
2.  在左侧存储空间列表中，单击目标存储空间名称，打开该存储空间概览页面。
3.  单击基础设置页签，找到**日志管理**区域。
4.  单击**设置**，开启日志存储并设置**日志存储位置**及**日志前缀**。
    -   日志存储位置：下拉选择存储日志记录的存储空间名称，只能选择同一用户下同一数据中心的存储空间。
    -   日志前缀：请填写日志生成的目录和前缀，日志将被记录到指定的目录下。
5.  单击**保存**。

## 日志记录命名规则 {#section_j4c_s1b_ygb .section}

存储访问日志记录的对象命名规则示例如下:

<TargetPrefix\><SourceBucket\>YYYY-MM-DD-HH-MM-SS-<UniqueString\>

-   <TargetPrefix\>：用户指定的日志前缀。
-   <SourceBucket\>：源存储空间名称。
-   YYYY-MM-DD-HH-MM-SS：该日志创建时的北京时间，显示内容为“年-月-日-小时-分-秒”，字母位数与最终呈现数字位数一致。
-   <UniqueString\>：OSS 系统生成的字符串。

例如，一个实际用于存储 OSS 访问日志的对象名称如下：

MyLog-OSS-example2015-09-10-04-00-00-0000

-   MyLog 为用户指定的日志前缀。
-   oss-example 是源存储空间的名称。
-   2015-09-10-04-00-00 是该日志被创建时的北京时间。
-   0000 是 OSS 系统生成的字符串。

