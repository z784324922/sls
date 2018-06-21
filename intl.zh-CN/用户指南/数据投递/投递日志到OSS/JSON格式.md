# JSON格式 {#concept_y4l_34q_zdb .concept}

本文档主要介绍日志服务投递OSS使用JSON存储的相关配置，关于投递日志到OSS的其它内容请参考[投递日志到OSS](intl.zh-CN/用户指南/数据投递/投递日志到OSS.md)。

OSS文件压缩类型及文件地址见下表。

|压缩类型|文件后缀|OSS文件地址举例|
|:---|:---|:--------|
|不压缩|无|oss://oss-shipper-shenzhen/ecs\_test/2016/01/26/20/54\_1453812893059571256\_937|
|snappy|.snappy|oss://oss-shipper-shenzhen/ecs\_test/2016/01/26/20/54\_1453812893059571256\_937.snappy|

**不压缩**

Object由多条日志拼接而成，文件的每一行是一条JSON格式的日志，样例如下：

```
{"__time__":1453809242,"__topic__":"","__source__":"10.170.148.237","ip":"10.200.98.220","time":"26/Jan/2016:19:54:02 +0800","url":"POST
              /PutData?Category=YunOsAccountOpLog&AccessKeyId=U0UjpekFQOVJW45A&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=pD12XYLmGxKQ%2Bmkd6x7hAgQ7b1c%3D
              HTTP/1.1","status":"200","user-agent":"aliyun-sdk-java"}
```

**snappy压缩**

采用 [snappy 官网](https://google.github.io/snappy/) 的 C++ 实现（Snappy.Compress 方法），对 none 格式数据做文件级别的压缩得到。对`.snappy`文件解压缩后即可得到对应的 none 格式文件。

**使用 C++ Lib 解压缩**

[snappy 官网](http://google.github.io/snappy/) 右侧下载 Lib，执行 Snappy.Uncompress 方法解压。

**使用 Java Lib解压缩**

[xerial snappy-java](https://github.com/xerial/snappy-java)，可以使用 Snappy.Uncompress 或 Snappy.SnappyInputStream（不支持 SnappyFramedInputStream）。

```
<dependency>
<groupId>org.xerial.snappy</groupId>
<artifactId>snappy-java</artifactId>
<version>1.0.4.1</version>
<type>jar</type>
<scope>compile</scope>
</dependency>
```

**说明：** 1.1.2.1 版本存在 [bug](https://github.com/xerial/snappy-java/issues/142) 可能无法解压部分压缩文件，1.1.2.6 版本修复。

Snappy.Uncompress

```
String fileName = "C:\\我的下载\\36_1474212963188600684_4451886.snappy";
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
String fileName = "C:\\我的下载\\36_1474212963188600684_4451886.snappy";
SnappyInputStream sis = new SnappyInputStream(new FileInputStream(fileName));
byte[] buffer = new byte[4096];
int len = 0;
while ((len = sis.read(buffer)) != -1) {
    System.out.println(new String(buffer, 0, len));
}
```

**Linux 环境解压工具**

针对 Linux 环境，我们提供了可以解压 snappy 文件的工具，点击下载 [snappy\_tool](http://logservice-resource.oss-cn-shanghai.aliyuncs.com/tools/snappy_tool)。

```
./snappy_tool 03_1453457006548078722_44148.snappy 03_1453457006548078722_44148
compressed.size: 2217186
snappy::Uncompress return: 1
uncompressed.size: 25223660
```

