# 分析-Nginx访问日志 {#concept_64278_zh .concept}

目前，日志服务支持保存查询语句为**快速查询**，对查询设置触发周期（间隔），并对执行结果设定判断条件并且告警。您还可以设置告警动作，即定期运行的快速查询结果一旦触发告警条件，将以何种方式通知您。

目前支持通知方式有以下3种：

-   通知中心：在阿里云通知中心可以设置多个联系人，通知会通过邮件和短信方式发送。
-   WebHook：包括钉钉机器人，及自定义WebHook等。
-   （即将支持）写回日志服务（logstore\)：可以通过流计算，函数服务进行事件订阅；也可以对告警生成视图和报表。

告警功能配置与操作请参考[设置告警](../../../../intl.zh-CN/用户指南/告警/设置告警任务/设置告警.md#)。 除通过日志服务监控并告警外，您还可以通过云监控产品对日志服务各项指标进行监控，并在触发告警条件时为您发送提醒消息。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13215/155617705132470_zh-CN.png)

## 实践场景 {#section_izx_jnb_wfb .section}

本文以Nginx日志为例，为您示范如何使用日志服务对采集到的日志信息定时查询分析，并通过日志查询结果判断以下业务问题：

-   是否有错误。
-   是否有性能问题。
-   是否有流量急跌或暴涨。

## 准备工作（Nginx日志接入） {#section_dpv_mnb_wfb .section}

1.  采集日志数据。
    1.  在Logstore列表页面进入数据源接入向导，并选择**NGINX访问日志**。
    2.  填写配置名称，日志路径，Nginx日志格式，NGINX键名称，并酌情配置高级选项。
    3.  单击**下一步**，进入配置索引步骤。
2.  设置索引。

    详细内容请参考[开启并配置索引](../../../../intl.zh-CN/用户指南/查询与分析/开启并配置索引.md#)与[可视化](../../../../intl.zh-CN/用户指南/可视化分析/其他可视化方案/对接DataV.md#)或最佳实践[网站日志分析案例](../../../../intl.zh-CN/快速入门/采集并分析Nginx访问日志.md#)。

3.  对关键指标设置视图和告警。

Sample视图：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13215/155617705132472_zh-CN.png)

## 操作步骤 { .section}

## 1. 判断是否有错误 { .section}

错误一般有这样几类：404（请求无法找到地址）/502/500（服务端错误），我们一般只需关心500（服务端错误）。

判断是否有500错误，您可以使用以下query统计单位时间内错误数c，并将报警规则设置为c \> 0 则发送告警。

```
status:500 | select count(1) as c
```

这种方式比较简单，但往往过于敏感，对于一些业务压力较大的服务而言有零星几个500是正常的。为了应对这种情况，您可以在告警条件中设置触发次数为2次，即只有连续2次检查都符合条件后再发告警。

## 2. 判断是否有性能问题 { .section}

服务器运行过程中虽然没有错误，但有可能会出现延迟（Latency）增大情况，您可以针对延迟进行告警。

例如您可以通过以下方式计算某个接口（“/adduser"）所有写请求（”Post“）延时。告警规则设置为 l \> 300000 即当平均值超过300ms后告警。

```
Method:Post and URL:"/adduser" | select avg(Latency) as l
```

利用平均值来报警简单而直接，但这种方法往往会使得一些个体请求延时被平均掉，无法反馈问题。例如，对该时间段的Latency计算一个数学上的分布，即划分20个区间，计算每个区间内的数目，从分布图上可以看到大部分请求延时非常低（<20ms），但最高的延时有2.5s。

```
Method:Post and URL:"/adduser" | select numeric_histogram(20, Latency)
```

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13215/155617705132473_zh-CN.png)

为应对这种情况，您可以用数学统计中的百分数（99%最大延时）来作为报警条件，这样既可以排除偶发的延时高引起误报，也能对整体的演示更有代表性。以下的语句计算了99%分位的延时大小 approx\_percentile\(Latency, 0.99\) ，同样您也可以修改第二个参数进行其他分位的划分，例如中位数的请求延时 approx\_percentile\(Latency, 0.5\)。

```
Method:Post and URL:"/adduser" | select approx_percentile(Latency, 0.99) as p99
```

在监控的场景中，您也可以在一个图上绘出平均延时，50%分位延时，以及90%分位延时。以下是按一天的窗口（1440分钟）统计各分钟内延时的图：

```
* | select avg(Latency) as l, approx_percentile(Latency, 0.5) as p50, approx_percentile(Latency, 0.99) as p99, date_trunc('minute', time) as t group by t order by t desc limit 1440
```

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13215/155617705132474_zh-CN.png)

## 3. 判断是否有流量急跌或暴涨 { .section}

服务器端自然流量一般符合概率上的分布，会有一个缓慢上涨或下降过程。流量急跌或暴涨表示短时间内流量变化非常大，一般都是不正常的现象，需要留意。

如以下监控图所示，在2分钟时间内流量大小下跌30%以上，在2分钟内后又迅速恢复。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13215/155617705132475_zh-CN.png)

急跌和暴涨一般会有如下参考系：

-   上一个时间窗口：环比上一个时间段。
-   上一天该时间段的窗口：环比昨天。
-   上一周该时间段的窗口：环比上周。

本文以第一种情况为例，计算流量infow数据的变动率，也可以换成QPS等流量。

## 3.1 首先定义一个计算窗口 { .section}

定一个1分钟的窗口，统计该分钟内的流量大小，以下是一个5分钟区间统计：

```
* | select sum(inflow)/(max(__time__)-min(__time__)) as inflow , __time__-__time__%60  as window_time from log group by window_time order by window_time limit 15
```

从结果分布上看，每个窗口内的平均流量 `sum(inflow)/(max(__time__)-min(__time__))` 应该是均匀的。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13215/155617705332476_zh-CN.png)

## 3.2 计算窗口内的差异值（最大值变化率） { .section}

这里我们会用到子查询，我们写一个查询，从上述结果中计算最大值或最小值与平均值的变化率（这里的max\_ratio），例如如下计算结果max\_ratio为 1.02。我们可以定义一个告警规则，如果max\_ratio \> 1.5（变化率超过50%）就告警。

```
 * | select max(inflow)/avg(inflow) as max_ratio from (select sum(inflow)/(max(__time__)-min(__time__)) as inflow , __time__-__time__%60  as window_time from log group by window_time order by window_time limit 15) 
```

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13215/155617705332477_zh-CN.png)

## 3.3 计算窗口内的差异值（最近值变化率） { .section}

在一些场景中我们更关注最新的数值是否有波动（是否已经恢复），那可以通过max\_by方法获取最大windows\_time中的流量来进行判断，这里计算的最近值为lastest\_ratio=0.97。

```
 * | select max_by(inflow, window_time)/1.0/avg(inflow) as lastest_ratio from (select sum(inflow)/(max(__time__)-min(__time__)) as inflow , __time__-__time__%60  as window_time from log group by window_time order by window_time limit 15) 
```

**说明：** 这里的max\_by函数计算结果为字符类型，我们需要强转成数字类型。 如果要计算变化相对率，可以用（1.0-max\_by\(inflow, window\_time\)/1.0/avg\(inflow\)\) as lastest\_ratio代替。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13215/155617705332478_zh-CN.png)

## 3.4 计算窗口内的差异值（定义波动率，上一个值与下一个变化率） { .section}

波动率另外一种计算方法是数学上一阶导数，即当前窗口值与上个窗口值的变化值。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13215/155617705332479_zh-CN.png)

使用窗口函数\(lag\)进行计算，窗口函数中提取当前inflow与上一个周期inflow "lag\(inflow, 1, inflow\)over\(\) " 进行差值，并除以当前值作为一个变化比率：

```
 * | select (inflow- lag(inflow, 1, inflow)over() )*1.0/inflow as diff, from_unixtime(window_time) from (select sum(inflow)/(max(__time__)-min(__time__)) as inflow , __time__-__time__%60  as window_time from log group by window_time order by window_time limit 15) 
```

在示例中，11点39分流量有一个较大的降低（窗口之间变化率为40%以上）：

如果要定义一个绝对变化率，可以使用abs函数（绝对值）对计算结果进行统一。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13215/155617705332480_zh-CN.png)

## 总结 { .section}

日志服务查询分析能力是完整SQL92，支持各种数理统计与计算等，只要会用SQL都能进行快速分析，欢迎尝试！

