# Tree map {#concept_ztv_sz1_v2b .concept}

A tree map is a rectangle chart that contains rectangle blocks in a tree structure. The area of each rectangle block in a tree map is proportional to the amount of data it represents. The larger the area is, the greater the proportion of data it represents.

## Components {#section_sym_11b_v2b .section}

Rectangle blocks are generated from data calculations and distributed in the chart.

## Configuration {#section_itf_c1b_v2b .section}

|Configuration|Description|
|:------------|:----------|
|**Legend filter**|Field that indicates a data type.|
|**Value column**|Value field. The greater the value of a data type, the larger the corresponding rectangle block will be.|
|**Padding**|The spacing between any two adjacent sides of different rectangle blocks. The value range of this field is 0–100 px.|

## Procedure {#section_t24_d1b_v2b .section}

1.  Enter a query statement, select a time interval, and then click **Search & Analysis**.
2.  Select the tree map ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17720/15416622979593_en-US.png).
3.  Configure the chart properties.

## Example {#section_nks_j1b_v2b .section}

Analyze the distribution of the hostnames in the Nginx logs.

```
* | select hostname, count(1) as count group by hostname order by count desc limit 1000 
```

Select `hostname` from the **Legend Filter** drop-down list and select `count` from the **Value Column** drop-down list.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17720/15416622979594_en-US.png)

