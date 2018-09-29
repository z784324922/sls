# Nginx logs  {#reference_kv2_sr5_vdb .reference}

The Nginx log format and directory are generally in the configuration file /etc/nginx/nginx.conf.

## Nginx log format {#section_rbt_stb_ry .section}

The log configuration file defines the print format of Nginx logs, that is, the main format:

```
log_format main '$remote_addr - $remote_user [$time_local] "$request" '
                 '$request_time $request_length '
                 '$status $body_bytes_sent "$http_referer" '
                 '"$http_user_agent"';
```

The declaration uses the main log format and the written file name.

```
access_log /var/logs/nginx/access.log main
```

## Field Description {#section_dhb_1c3_dbb .section}

|Field name |Definition|
|:----------|:---------|
|remoteaddr|The IP address of the client.|
|remote\_user |The username of the client.|
|request |The requested URL and HTTP protocol.|
|status|The request status.|
|bodybytessent|The number of bytes \(not including the size of the response header\) sent to the client. The total number of bytes for this variable is the same as that sent to the client by bytes\_sent in modlogconfig of the Apache module.|
|connection|The connection serial number.|
|connection\_requests |The number of requests received by using a connection.|
|msec|The log write time, which is  which is measured in seconds and precise to milliseconds.|
|pipe|Whether or not requests are sent by using the HTTP pipeline.  p indicates requests are sent by using the HTTP pipeline. Otherwise, the value is . . |
|httpreferer| Web page link from which the access is directed.|
|“http\_user\_agent”|Information about the browser on the client. http\_user\_agent must be enclosed in double quotation marks.|
|requestlength|The length of a request, including the request line,  request header, and request body. |
|Request\_time|The request processing time, which is measured in seconds and precise to milliseconds.  The time starts when the first byte is sent to the client and ends when the logs are written after the last character is sent to the client. |
|\[$time\_local\]|he local time in the general log format. This variable must be enclosed in brackets.|

## Log sample {#section_g2b_1c3_dbb .section}

```
192.168.1.2 - - [10/Jul/2015:15:51:09 +0800] "GET /ubuntu.iso HTTP/1.0" 0.000 129 404 168 "-" "Wget/1.11.4 Red Hat modified" 
```

## Configure Logtail to collect Nginx logs {#section_vkv_15b_ry .section}

1.  Click the **Data Import Wizard** chart in the Logstore list page to enter the data import wizard.
2.  Select a data source.

    Select the **text file** and click **Next**. 

3.  Select the data source.

    1.  Enter the **Configuration Name**, and **Log Path**.
    2.  Enter the nNginx log format.

        Complete the standard Nginx profile log configuration section, typically beginning with the `log_format`. Log Service automatically reads your Nginx key.

    3.  Set **Advanced Options** according to your requirements. Click **Next** after completing the configurations.

        For more information about advanced options, see Advanced options.

    After configuring Logtail, apply the configuration to the machine group to start collecting Nginx logs standardly.


