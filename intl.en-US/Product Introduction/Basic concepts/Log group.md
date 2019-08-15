# Log group {#concept_y1s_gg3_wgb .concept}

A log group is a collection of logs. It is the basic unit for writing and reading data through the API or SDK. Logs in a log group have the same metadata such as the IP address and source.

When data is written through the Log Service API or SDK, multiple logs are packaged into a log group and sent to Log Service. Compared with reading and writing data by log, reading and writing data by log group can minimize the number of read and write operations and improve business efficiency.

A log group has a maximum size of 5 MB.

 ![Log group](images/2377_en-US.png "Log group")

