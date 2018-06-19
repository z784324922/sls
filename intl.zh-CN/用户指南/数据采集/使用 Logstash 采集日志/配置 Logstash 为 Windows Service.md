# 配置 Logstash 为 Windows Service {#concept_nlw_4p5_vdb .concept}

在 PowerShell 下启动 logstash.bat，Logstash进程会在前台工作，一般用于配置测试和采集调试。建议调试通过后把Logstash设置为 Windows Service，可以保持后台运行以及开机自启动。

除了将Logstash设置为Windows Service之外，您还可以通过命令行启动、停止、修改和删除服务。更多 NSSM 使用方法请参考 [NSSM官方文档](https://nssm.cc/usage)。

## 添加服务 {#section_t3z_gbk_z1b .section}

一般用于首次部署时执行，如已添加过服务，请跳过该步骤。

您可以执行以下命令添加服务。

-   32 位系统

    ```
    C:\logstash-2.2.2-win\nssm-2.24\win32\nssm.exe install logstash "C:\logstash-2.2.2-win\bin\logstash.bat" "agent -f C:\logstash-2.2.2-win\conf"
    ```

-   64 位系统

    ```
    C:\logstash-2.2.2-win\nssm-2.24\win64\nssm.exe install logstash "C:\logstash-2.2.2-win\bin\logstash.bat" "agent -f C:\logstash-2.2.2-win\conf"
    ```


## 启动服务 {#section_rpj_5bk_z1b .section}

如Logstash conf 目录后有配置文件更新，请先停止服务，再启动服务。

您可以执行以下命令启动服务。

-   32 位系统

    ```
    C:\logstash-2.2.2-win\nssm-2.24\win32\nssm.exe start logstash
    ```

-   64 位系统

    ```
    C:\logstash-2.2.2-win\nssm-2.24\win64\nssm.exe start logstash
    ```


## 停止服务 {#section_h22_ybk_z1b .section}

您可以执行以下命令停止服务。

-   32 位系统

    ```
    C:\logstash-2.2.2-win\nssm-2.24\win32\nssm.exe stop logstash
    ```

-   64 位系统

    ```
    C:\logstash-2.2.2-win\nssm-2.24\win64\nssm.exe stop logstash
    ```


## 修改服务 {#section_xhs_zbk_z1b .section}

您可以执行以下命令修改服务。

-   32 位系统

    ```
    C:\logstash-2.2.2-win\nssm-2.24\win32\nssm.exe edit logstash
    ```

-   64 位系统

    ```
    C:\logstash-2.2.2-win\nssm-2.24\win64\nssm.exe edit logstash
    ```


## 删除服务 {#section_lw2_bck_z1b .section}

您可以执行以下命令删除服务。

-   32 位系统

    ```
    C:\logstash-2.2.2-win\nssm-2.24\win32\nssm.exe remove logstash
    ```

-   64 位系统

    ```
    C:\logstash-2.2.2-win\nssm-2.24\win64\nssm.exe remove logstash
    ```


