# Data encoding method {#reference_uvb_r5q_12b .reference}

Protocol Buffer is a structured data interchange format developed by Google. It is widely used in many internal and external services of Google.  Currently, Log Service uses Protocol Buffer format as  the standard log writing format.  You must serialize the original log data into Protocol Buffer data streams before writing logs to Log Service by using APIs.

```
message Log
{
    required uint32 Time = 1;// UNIX Time Format
    message Content
    {
        required string Key = 1;
        required string Value = 2;
    }  
    repeated Content Contents = 2;
}

message LogTag
{
    required string Key = 1;
    required string Value = 2;
}

message LogGroup
{
    repeated Log Logs= 1;
    optional string Reserved = 2; // reserved fields
    optional string Topic = 3;
    optional string Source = 4;
    repeated LogTag LogTags = 6;
}

message LogGroupList
{
    repeated LogGroup logGroupList = 1;
}
```

**Note:** 

-   Protocol Buffer does not require the key-value pair to be unique. You must avoid such situation. Otherwise, the behavior is undefined.
-   For more information about Protocol Buffer format, see [Github](https://github.com/google/protobuf).
-   For more information about the API for writing logs to Log Service, see [PostLogStoreLogs](reseller.en-US/API Reference/Logstore related APIs/PostLogstoreLogs.md).

