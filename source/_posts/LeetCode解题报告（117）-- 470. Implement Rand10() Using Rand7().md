---
title: LeetCode解题报告（117）-- 470. Implement Rand10() Using Rand7()
tags:
  - LeetCode
mathjax: true
abbrlink: 58530
date: 2020-08-29 14:49:25
---

## Problem

Given the **API** `rand7` which generates a uniform random integer in the range 1 to 7, write a function `rand10` which generates a uniform random integer in the range 1 to 10. You can only call the API `rand7` and you shouldn't call any other API. Please **don't** use the system's `Math.random()`.

**Notice that** Each test case has one argument `n`, the number of times that your implemented function `rand10` will be called while testing. 

**Follow up:**

1. What is the [expected value](https://en.wikipedia.org/wiki/Expected_value) for the number of calls to `rand7()` function?
2. Could you minimize the number of calls to `rand7()`?

<!-- more -->

**Example 1:**

```
Input: n = 1
Output: [2]
```

**Example 2:**

```
Input: n = 2
Output: [2,8]
```

**Example 3:**

```
Input: n = 3
Output: [3,8,10]
```

**Constraints:**

- `1 <= n <= 105`

------

## Analysis

&emsp;&emsp;题目要求我们用`rand7()`去实现一个`rand10()`，随机数一个很重要的要求就是产生每个数字的概率都是一样的。

&emsp;&emsp;首先解决数字范围问题，`rand7()`产生的数字范围比`rand10()`要小，比较直观的解决方法就是产生两次，然后想加起来，这样的范围就从`[0,1]`扩大为`[2, 14]`了，再减去1，那么就是`[1, 13]`，这个范围是包括了目标范围`[1, 10]`的。但是这里面有一个很严重的问题，就是这样产生的每个数字并不是等概率的。

&emsp;&emsp;仔细分析一下，造成上面不是等概率的原因是因为不同的数字组合可以产生一样的数字，如`1 + 2 = 3`和`2 + 1 = 3`，为了解决这种情况，可以给第一次随机产生的数字提高一个维度，乘上一个系数，这样第一次和第二次产生的数字就可以完全错开了。以`rand2() -> rand4()`为例，第一次随机生成的为`(rand2() - 1) * N`，第二次随机生成的为`rand2()`。所以可以推出从低到高的公式就是：
$$
rand(x)() -> rand(xN)() = (rand(x)() - 1) \times N + rand(x)()
$$
&emsp;&emsp;然后我们再来看一下如果从高到低推导，从高到低的推导其实更加容易，因为借助取余运算很容易就能把数值缩小到一个范围内，当然了，要成倍数才能保持等概率。这里直接给出公式了：
$$
rand(xN) -> rand(x)() = (rand(xN) \% N) + 1
$$
&emsp;&emsp;至此，无论是从低到高还是从高到低的转换，我们都有公式了，但是有个限制就是必须有倍数关系。回到这道题目，思路就可以转换为

1. `rand7() -> rand49()`
2. `rand49() -> rand40()`
3. `rand40() -> rand10()`

&emsp;&emsp;上面第1、3都好完成，根据公式做就好了，但是第二步就有点麻烦了，从`rand49()`到`rand40()`不是倍数关系，所以这里需要用到[Rejection Sampling](https://en.wikipedia.org/wiki/Rejection_sampling)。背后的数学证明比较复杂，这里我简单讲一下原理。在`rand49()`随机生成的数字范围内，取其中小于40的部分，实际上也是符合随机分布的。所以在得到`rand49()`的结果后，我们只要小于40的数字，这样计算得到的`rand10()`就是等概率的。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
// The rand7() API is already defined for you.
// int rand7();
// @return a random integer in the range 1 to 7

class Solution {
public:
    int rand10() {
        while (1) {
            int rand49 = (rand7() - 1) * 7 + rand7();
            if (rand49 <= 40) {
                return rand49 % 10 + 1;
            }
        }
    }
};
```

------

## Summary

&emsp;&emsp;这道随机数的题目看上去容易，但是实现起来背后的数学逻辑还是非常丰富的，还牵涉到一些概率学的内容。这道题的分析到这里，谢谢！
