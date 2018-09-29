# Configure and parse text logs {#concept_c4f_b3c_wdb .concept}

## Specify log line separation method {#section_jbf_df1_ry .section}

A full access log is typically a row by line, such as the nginx's access log, each log is split with line breaks. For example, the following two access logs:

```
10.1.1.1 - - [13/Mar/2016:10:00:10 +0800] "GET / HTTP/1.1" 0.011 180 404 570 "-" "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; 360se)"
10.1.1.1 - - [13/Mar/2016:10:00:11 +0800] "GET / HTTP/1.1" 0.011 180 404 570 "-" "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; 360se)"
```

For Java applications, a program log usually spans several lines. The characteristic log header is used to separate two logs. For example, see the following Java program log:

```
[2016-03-18T14:16:16,000] [INFO] [SessionTracker] [SessionTrackerImpl.java:148] Expiring sessions
0x152436b9a12aecf, 50000
0x152436b9a12aed2, 50000
0x152436b9a12aed1, 50000
0x152436b9a12aed0, 50000
```

The preceding Java log has a starting field in the time format. The regular expression is \\\[\\d+-\\d+-\\w+:\\d+:\\d+,\\d+\]\\s.\* \*. You can complete the configurations in the console as follows.

 ![](images/2869_en-US.png "Full mode parsing Regular Expression") 

## Extract log fields {#section_olm_3f1_ry .section}

According to the [Log Service data models](../../../../intl.en-US/Product Introduction/Basic concepts/Log.md), a log contains one or more key-value pairs. To extract specified fields for analysis, you must set a regular expression. If log content does not need to be processed, the log can be considered as a key-value pair.

For the access log in the previous example, you can choose to extract a field or not.

-   When fields are extracted

    Regular expression: `(\S+)\s-\s-\s\[(\S+)\s[^]]+]\s"(\w+). *`, Extracted contents: `10.1.1.1`, `13/Mar/2016:10:00` and `GET`.

-   When fields are not extracted

    Regular expression: `(. *)`, Extracted contents: `10.1.1.1 - - [13/Mar/2016:10:00:10 +0800] "GET / HTTP/1.1" 0.011 180 404 570 "-" "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; 360se)"`"


## Specify log time  {#section_yl1_4f1_ry .section}

According to the Log Service data models, a log must have a time field in UNIX timestamp format. Currently, the log time can be set to the system time when Logtail collects the log or the time field in the log content. 

For the access log in the previous example:

-   Extract the time field in the log content`Time:  13/Mar/2016:10:00:10`Time expression: `%d/%b/%Y:%H:%M:%S`
-   The system time when the log is collected Time: Timestamp when the log is collected.

