# JSON storage {#concept_y4l_34q_zdb .concept}

This document introduces the configurations about JSON storage for Log Service logs that are shipped to Object Storage Service \(OSS\). For more information about shipping logs to OSS, see [Ship logs to OSS](reseller.en-US/User Guide/Data shipping/Ship logs to OSS/Ship logs to OSS.md).

The compression types and file addresses of OSS files are as follows.

|Compression type|File suffix|Example of OSS file address|
|:---------------|:----------|:--------------------------|
|Do Not Compress|None|oss://oss-shipper-shenzhen/ecs\_test/2016/01/26/20/54\_1453812893059571256\_937|
|snappy|.snappy|oss://oss-shipper-shenzhen/ecs\_test/2016/01/26/20/54\_1453812893059571256\_937.snappy|

**Do Not Compress**

An object is combined by multiple logs. Each line of the file is a log in the JSON format. See the following example:

```
{"__time__":1453809242,"__topic__":"","__source__":"10.170. ***.***","ip":"10.200. **.***","time":"26/Jan/2016:19:54:02 +0800","url":"POST
              /PutData? Category=YunOsAccountOpLog&AccessKeyId=<yourAccessKeyId>&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=<yourSignature>
              HTTP/1.1","status":"200","user-agent":"aliyun-sdk-java"}
```

**Compress \(snappy\)**

Use [Snappy](https://google.github.io/snappy/)  C++ \(Snappy.Compress method\) to compress the data in none format at the file level. You can obtain the file in none format after decompress the .snappy file. You can obtain the file in none format after decompress the `.snappy` file.

**Decompressing through C++ Lib**

Download Lib from [Snappy official website](http://google.github.io/snappy/) and use the Snappy.Uncompress method to decompress the .snappy file.

**Java Lib**

\[xerial snappy-java\], use Snappy.Uncompress or Snappy.SnappyInputStream \(SnappyFramedInputStream not supported\).

```
<dependency>
<groupId>org.xerial.snappy</groupId>
<artifactId>snappy-java</artifactId>
<version>1.0.4.1</version>
<type>jar</type>
<scope>compile</scope>
</dependency>
```

**Note:** Version 1.1.2.1 may not decompress parts of the compressed file because of a [bug](https://github.com/xerial/snappy-java/issues/142) , which is

Snappy.Uncompress

```
String fileName = "C:\\My download\\36_1474212963188600684_4451886.snappy";
RandomAccessFile randomFile = new RandomAccessFile(fileName, "r");
int fileLength = (int) randomFile.length();
randomFile.seek(0);
byte[] bytes = new byte[fileLength];
int byteread = randomFile.read(bytes);
System.out.println(fileLength);
System.out.println(byteread);
byte[] uncompressed = Snappy.uncompress(bytes);
String result = new String(uncompressed, "UTF-8");
System.out.println(result);
```

Snappy.SnappyInputStream

```
String fileName = "C:\\My download\\36_1474212963188600684_4451886.snappy";
SnappyInputStream sis = new SnappyInputStream(new FileInputStream(fileName));
byte[] buffer = new byte[4096];
int len = 0;
while ((len = sis.read(buffer)) ! = -1) {
    System.out.println(new String(buffer, 0, len));
}
```

**Unzipping tool under Linux environment**

For Linux environment, a tool used to decompress .snappy file is provided. Click to download the [snappy\_tool](http://logservice-resource.oss-cn-shanghai.aliyuncs.com/tools/snappy_tool).

```
./snappy_tool 03_1453457006548078722_44148.snappy 03_1453457006548078722_44148
compressed.size: 2217186
snappy::Uncompress return: 1
uncompressed.size: 25223660
```

