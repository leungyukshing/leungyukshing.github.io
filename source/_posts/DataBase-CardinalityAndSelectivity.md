---
title: 数据库索引基数与选择度
mathjax: true
abbrlink: 9687
date: 2020-01-13 15:29:49
---

## 数据库索引基数与选择度

&emsp;&emsp;在这篇博客我们来讨论一下索引基数（cardinality）和索引选择度（selectivity）。他们大致上的意思是**表达了数据库中某个字段的取值的混乱程度。**
<!-- more -->

## 索引基数

&emsp;&emsp;这个概念代表了表中某个字段中不同数组的个数，可以理解为使用`DISTINCT`之后的结果。举个例子，如果这个字段是性别，而如果性别字段只有“男”或者“女”这两种可能的话，这个字段的cardinality就是2。

## 索引选择度

&emsp;&emsp;selectivity是根据cardinality计算出来的，公式为：
$$
selectivity = \frac{cardinality}{number\ of \ rows} \times100\%
$$
这个值可以代表这个属性的数据的区别程度，这个值越接近100%，索引的效率越高，MySQL的优化器就倾向于选择使用这个索引。用回上面性别的例子，如果一个表格用几万行的记录，但是这一列的值只有两种可能，那么这个值肯定是非常小的；相反对于主键列，每一条record都是不一样的，所以这个值就是100%。
