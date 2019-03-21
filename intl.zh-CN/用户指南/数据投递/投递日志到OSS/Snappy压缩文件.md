# Snappy压缩文件 {#concept_kpb_v4z_xgb .concept}

将日志服务中的日志数据投递到OSS中时，可以设置压缩方式为[snappy](https://google.github.io/snappy/)压缩，解压时可以通过C++ Lib、Java Lib、Python Lib和Linux 环境解压工具进行解压缩。

投递日志到OSS时选择[snappy](https://google.github.io/snappy/)算法对数据做压缩，可以减少 OSS Bucket 存储空间使用量。本文档提供以下解压方案。

-   **[使用 C++ Lib 解压缩](#)**
-   **[使用 Java Lib解压缩](#)**
-   **[使用 Python Lib解压缩](#)**
-   **[使用Linux 环境解压工具](#)**

## 使用 C++ Lib 解压缩 {#section_jp4_qpz_xgb .section}

[snappy 官网](http://google.github.io/snappy/) 右侧下载 Lib，执行 Snappy.Uncompress 方法解压。

## 使用 Java Lib解压缩 {#section_ctc_rpz_xgb .section}

下载[xerial snappy-java](https://github.com/xerial/snappy-java)，使用 Snappy.Uncompress 或 Snappy.SnappyInputStream解压缩，不支持 SnappyFramedInputStream。

**说明：** 1.1.2.1 版本存在 [bug](https://github.com/xerial/snappy-java/issues/142) ，可能无法解压部分压缩文件，1.1.2.6及以后版本已修复该问题，建议使用最新的Java Lib解压缩。

```
<dependency>
<groupId>org.xerial.snappy</groupId>
<artifactId>snappy-java</artifactId>
<version>1.0.4.1</version>
<type>jar</type>
<scope>compile</scope>
</dependency>
```

-   Snappy.Uncompress

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

-   Snappy.SnappyInputStream

    ```
    String fileName = "C:\\我的下载\\36_1474212963188600684_4451886.snappy";
    SnappyInputStream sis = new SnappyInputStream(new FileInputStream(fileName));
    byte[] buffer = new byte[4096];
    int len = 0;
    while ((len = sis.read(buffer)) != -1) {
        System.out.println(new String(buffer, 0, len));
    }
    ```


## 使用 Python Lib解压缩 {#section_akh_tpz_xgb .section}

1.  下载并安装[Python压缩库](https://pypi.org/project/python-snappy/)。
2.  执行解压代码。

    解压缩代码示例：

    ```
    import snappy
    compressed = open('/tmp/temp.snappy').read()
    snappy.uncompress(compressed)
    
    ```


**说明：** 不支持通过以下命令行工具解压OSS投递的snappy压缩文件，命令行模式下仅支持hadoop模式（hadoop\_stream\_decompress）与流模式（stream\_decompress）。

```
$ python -m snappy -c uncompressed_file compressed_file.snappy
$ python -m snappy -d compressed_file.snappy uncompressed_file

```

## Linux 环境解压工具 {#section_zbv_rpz_xgb .section}

针对 Linux 环境，日志服务提供可以解压 snappy文件的工具，单击下载 [snappy\_tool](http://logservice-resource.oss-cn-shanghai.aliyuncs.com/tools/snappy_tool)。

```
./snappy_tool 03_1453457006548078722_44148.snappy 03_1453457006548078722_44148
compressed.size: 2217186
snappy::Uncompress return: 1
uncompressed.size: 25223660
```

