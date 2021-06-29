---
title: LeetCode解题报告（216）-- 458. Poor Pigs
tags:
  - LeetCode
mathjax: true
abbrlink: 25020
date: 2020-11-14 20:56:10
---

## Problem

There are 1000 buckets, one and only one of them is poisonous, while the rest are filled with water. They all look identical. If a pig drinks the poison it will die within 15 minutes. What is the minimum amount of pigs you need to figure out which bucket is poisonous within one hour?

Answer this question, and write an algorithm for the general case.

<!-- more -->

**General case:**

If there are `n` buckets and a pig drinking poison will die within `m` minutes, how many pigs (`x`) you need to figure out the **poisonous** bucket within `p` minutes? There is exactly one bucket with poison.

**Note:**

1. A pig can be allowed to drink simultaneously on as many buckets as one would like, and the feeding takes no time.
2. After a pig has instantly finished drinking buckets, there has to be a **cool down time** of *m* minutes. During this time, only observation is allowed and no feedings at all.
3. Any given bucket can be sampled an infinite number of times (by an unlimited number of pigs).

------

## Analysis

&emsp;&emsp;这是一道智力题，之前也说过，解决这类题目重在思考和分析，coding往往会很简单。先来看这道题目研究的是什么问题：有若干个**bucket **装着水，只有其中一个是有毒的。同时我们有若干只猪去试毒，毒性发作有一定的时间，也就是说经过一定的时间我们才知道某只猪是否中毒了。所以从直观上来看，其实有一个猪和测试次数的trade off，对于数量确定的bucket，如果猪的数量多的话，测试次数就可以少；如果猪的数量少的话，测试次数就会多。题目给定了bucket的数量和测试总时间以及中毒的时间（后两个参数可以推出测试的次数，下文解释），要求计算猪的数量。

&emsp;&emsp;我们先抛开题目的问法，来看另一种问法：**猪的数量是如何影响检测能力的。**假设中毒时间为15mins，总的测试时间为1h，所以测试的次数就是5次。这里解释一下为什么是5次，1个小时内，一只猪每15分钟喝一桶水，所以1h能喝4桶并且知道结果。如果四桶喝完之后还没有中毒，在总数为5桶的时候，就能够确定没有喝的那桶是有毒的。所以1只猪能检测5桶水。

&emsp;&emsp;然后我们来看看两只猪的情况。这里就要看一下要怎么要提高效率了，如果还是一桶一桶喝的话，这个效率就非常低，高效的方法应该是一个批量的思路（有点像水平扩容的思路）。上面提到一只猪的检测能力是5桶水，但实际上他的检测能力是5次饮水（一次饮水可以包含多桶水），所以一只猪一次可以喝5桶水，五次之后就能判断出有毒的水是出现在哪一批次。然后另一猪就是用来在这个批次中确定有毒的那桶水。实际上如果我们把水桶排列成一个5 * 5的矩阵，那么分别从横向喝纵向进行，就能够唯一地定位到有毒的水。所以这里看到检测能力的扩展是指数级别的。

&emsp;&emsp;所以单次检测能力决定了这个矩阵的边长，而猪的数量决定了维度。整理为数学的表达，假设检测次数为$m$，猪的数量为$x$，则检测的桶数为$m^x$。回到这道题目就非常简单了，题目要求的就是$x$，所以
$$
(\frac{minutesToTest}{minutesToDie} + 1)^x = buckets \\\log{(\frac{minutesToTest}{minutesToDie} + 1)^x} = \log{buckets} \\x \times \log{(\frac{minutesToTest}{minutesToDie} + 1)} = \log{buckets} \\x = \frac{\log{buckets}}{\log{(\frac{minutesToTest}{minutesToDie} + 1)}}
$$

------

## Solution

&emsp;&emsp;根据上面的数学推导编程即可，注意取整问题。

------

## Code

```c++
class Solution {
public:
    int poorPigs(int buckets, int minutesToDie, int minutesToTest) {
        return ceil(log(buckets) / log(minutesToTest / minutesToDie + 1));
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是一道非常考验数学分析能力的题目，思考分析为主，编程为辅。想下来还是花费了不少的时间，但是觉得挺有意思的。这道题目的分享到这里，谢谢！
