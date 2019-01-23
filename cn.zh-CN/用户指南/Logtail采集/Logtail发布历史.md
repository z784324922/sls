# Logtail发布历史 {#concept_w5w_q3q_zdb .concept}

## 0.16.18 {#section_ipk_tcz_ngb .section}

-   **新功能**
    -   支持Docker Event采集
    -   支持Journal日志采集
    -   插件处理新增 `processor_pick_key` 、 `processor_drop_last_key`
-   **优化**
    -   优化容器日志以及插件采集内存占用
    -   优化容器stdout多行日志采集性能

## 0.16.16 {#section_rvl_rkg_2gb .section}

-   **新功能**
    -   支持自动创建K8S audit（审计）日志各类资源
    -   支持通过环境变量配置启动参数，例如CPU、内存、发送并发等
    -   支持通过环境变量配置自定义tag上传
    -   sidecar模式支持自动创建配置
-   **优化**

    自动保存aliuid文件到本地文件

-   **问题修复**
    -   修复docker file采集有极低概率crash的问题
    -   修复通过环境变量创建出的配置在K8S中存在的 `IncludeLabel` 不生效问题

## 0.16.15 {#section_l3d_wkg_2gb .section}

-   **新功能**
    -   binlog支持gtid模式，该模式在MySQL支持时会自动开启
    -   历史数据导入支持配置检测外的文件夹
    -   K8S支持自动创建索引配置
-   **优化**
    -   当分行失败时，支持检查 `discardUnMatch` 并上报分行失败的日志
    -   遇到 unknown send error时自动重试，防止极低情况下数据丢失（例如发送的数据包中途被篡改）

## 0.16.14 {#section_gxx_xkg_2gb .section}

-   **优化**
    -   docker stdout 支持自动merge被docker engine拆分的日志
-   **新功能**
    -   导入历史数据支持通配符模式
    -   增加启动配置项 `default_tail_limit_kb` 用于配置首次采集文件跳转大小（默认1024）
    -   增加采集配置项 `batch_send_seconds` 用于配置数据发送merge的时间
    -   增加采集配置项 `batch_send_bytes` 用于配置数据发送merge的大小

## 0.16.13 {#section_w4h_1lg_2gb .section}

**新功能**

-   支持通过环境变量配置日志采集
-   binlog采集新增meta数据 `_filename_``_offset_`
-   安装脚本支持VPC下自动选择参数
-   支持全球加速安装模式

