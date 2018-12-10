# 消费-通过ETL清洗数据 {#concept_43805_zh .concept}

日志处理过程中的一个假设是：数据并不是完美的。在原始数据与最终结果之间有 Gap，需要通过 ETL（Extract Transformation Load）等手段进行清洗、转换与整理。

## 案例 {#section_xw2_3yk_5fb .section}

“我要点外卖”是一个平台型电商网站，涉及用户、餐厅、配送员等。用户可以在网页、App、微信、支付宝等进行下单点菜；商家拿到订单后开始加工，并自动通知周围的外卖送货员；快递员将外卖送到用户手中。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13201/154443486032397_zh-CN.png)

运营小组有两个任务：

-   掌握外卖送货员的位置，定点调度。
-   掌握优惠券、现金的使用情况，定点投放优惠券进行互动运营。

## 送货员位置信息（GPS）数据加工 { .section}

GPS 数据（X，Y）通过送货员 App 每分钟汇报一次，格式如下：

```
2016-06-26 19:00:15 ID:10015 DeviceID:EXX12345678 Network:4G GPS-X:10.30.339 GPS-Y:17.38.224.5 Status:Delivering

```

其中记录了上报时间，送货员 ID，使用网络，设备号，坐标位置 GPS-X，GPS-Y。GPS 给出的经纬度非常准确，但运营小组需要统计每个区域当前的状态，并不需要这么细的数据。因此，需要对原始数据做一次转换（Transformation），将坐标转换为可读的城市、区域、邮政编码等字段。

```
(GPS-X,GPS-Y) --> (GPS-X, GPS-Y, City, District, ZipCode)

```

这就是一个典型的 ETL 需求。使用 loghub 功能创建两个 logstore（PositionLog），以及转换后 logstore（EnhancedLog）。通过运行 ETL 程序（例如 Spark Streaming、Storm、或容器中启动 Consumer Library），实时订阅 PositionLog，对坐标进行转化，写入 EnhancedLog。可以对 EnhancedLog 数据机型仓库，连接实时计算进行可视化，或建立索引进行查询。

整个过程推荐架构如下：

1.  快递员 App 上埋点，每分钟汇报当前 GPS 位置一次，写入第一个 logstore（PositionLog）
    -   推荐使用 LogHub Android/IOS SDK、MAN（移动数据分析）将移动设备日志接入。
2.  通过实时程序实时订阅 PositionLog 数据，处理后实时写入 EnhancedLog Logstore。
    -   推荐使用 Spark Streaming、Storm 或 Consumer Lib（一种自动负载均衡的编程模式） 或 SDK 订阅。
3.  对 Enahanced Log 进行处理，例如计算后进行数据可视化。推荐：
    -   LogHub 对接流计算
    -   LogShipper 投递（OSS，E-MapReduce，Table Store、MaxCompute）
    -   LogSearch：订单查询等

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13201/154443486032400_zh-CN.png)

## 支付订单脱敏与分析 { .section}

支付服务接受支付请求，包括支付的账号、方式、金额、优惠券等。

-   其中包含部分敏感信息，需要进行脱敏。
-   需要对支付信息中优惠券、现金两个部分进行剥离。

整个过程如下：

1.  创建 4 个 logstore，原始数据（PayRecords），脱敏后数据（Cleaned），现金订单（Cash），代金券（Coupon）。应用程序通过 Log4J Appender 向原数据 logstore（PayRecords）写入订单数据。

    推荐使用 Log4J Appender，敏感数据不落盘。

2.  脱敏程序实时消费 PayRecords，将账号相关信息剥离后，写入 Cleaned Logstore。

分流程序实时消费 Cleaned Logstore，通过业务逻辑把优惠券、现金两个部分分别存入对账相关的 logstore（Cash、Coupon）进行后续处理。

实时脱敏、分流程序推荐使用 Spark Streaming、Storm 或 Consumer Lib（一种自动负载均衡的编程模式） 或 SDK 订阅。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13201/154443486032401_zh-CN.png)

## 其他 { .section}

-   loghub 功能下每个 logstore 可以通过 RAM 进行账号级权限控制。
-   loghub 当前读写可以通过监控获得，消费进度可以通过控制台查看。

