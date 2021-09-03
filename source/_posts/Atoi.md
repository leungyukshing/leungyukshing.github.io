---
title: C++实现Atoi
mathjax: true
date: 2021-09-02 22:46:11
tags:
---

# Introduction

&emsp;&emsp;相信熟悉C/C++的朋友都知道，如果我们把一个字符串转为数字，我们常常会使用到`atoi()`这个函数。那么今天我们就来自己动手实现一个`atoi()`，这个也是面试中经常会考的题目。

<!-- more -->

# Analysis

&emsp;&emsp;`atoi()`这个函数最大的作用就是字符串转数字，这个相信大家都会，实现`atoi()`的难点并不在于数字的计算，而在于非数字的部分，以及边界条件。我们先借用leetcode的一道题目去看看`atoi()`有一些什么基本的要求：

1. 开头的空格需要忽略；
2. 如果下一个字符是`+/-`，那么就需要记录下来，最后用于计算正负；
3. 不断读取下一个字符，直至出现非数字字符，后续部分不用考虑；
4. 把字符串转为数值，如果没有任何的数字，则返回0；
5. 如果数值的范围超过了32位有符号整数的表达范围，取边界值代替。即，如果小于$2^{31}$，则返回$2^{31}$；如果大于$2^{31} - 1$，则返回$2^{31} - 1$

&emsp;&emsp;针对第1点，我们可以用一个布尔变量`start`来表示是否已经开始计算数字，如果还没开始就可以忽略掉先导的0。然后处理正负号，有了正负号就算是正式开始计数，所以要把`start`置为true，同时再使用一个布尔变量`sign`记录正负值。第3点是比较简单的，只需要判断字符在0到9之间即可，不在这个范围内直接break。

&emsp;&emsp;这里要重点注意第五点，因为我们在计算过程中，采用的是按位计算，计算表达式为：`result = result * 10 + (s[i] - '0')`。这里就存在越界的情况：

+ 无论是正数还是负数，当result大于214748364再乘上10，一定越界，所以必须根据正负号返回边界值；
+ 如果是正数，并且result == 214748364，则还要看最后一位，正数的边界是2147483647，所以大于等于7都要返回边界值；
+ 同理，如果是负数，并且result == 214748364，也需要看最后一位，负数的边界是2147483648，所以大于等于8的都要返回边界值；
+ 这里还有一个坑，上面表达式中的括号一定不能少，这是因为如果result == 214748364，而最后一位字符是`6`，那么按道理是可以直接运算的。但如果不加括号的话，会先加上`s[i]`的ASCII，再减去`0`的ASCII，前一步就已经越界，这是个小坑，别踩！

# Code

```c++
#include<iostream>
#include <string>
using namespace std;

int atoi(string s) {
    int result = 0;
    bool sign = false; // false for positive, true for negative
    bool start = false;
    int size = s.size();
    for (int i = 0; i < size; i++) {
        if (!start && s[i] == ' ') {
            continue;
        }
        if (!start && (s[i] == '+' || s[i] == '-')) {
            sign = s[i] == '-';
            start = true;
            continue;
        }
            
        if (s[i] >= '0' && s[i] <= '9') {
            start = true;
            // handle overflow
            if (!sign) {
                if (result > 214748364 || (result == 214748364 && s[i] - '0' >= 7)) {
                    return INT_MAX;
                }
            } else {
                if (result > 214748364 || (result == 214748364 && s[i] - '0' >= 8)) {
                    return INT_MIN;
                }
            }
            result = result * 10 + (s[i] - '0');
        } else {
            break;
        }
    }
    if (sign) {
        result *= -1;
    }
    return result;
}


int main() {
    string str;
    cin >> str;
    cout << "result: " << atoi(str) << endl;
}
```

&emsp;&emsp;理清楚思路后，coding并不算很困难，下面我提供一些test case给大家测试程序的健壮性。

```
123
-123
010
+00131204
2147483647
2147483648
  -2147483649
23a8f
+4488
\n\n 123
0+123
```

# Reference

1. [LeetCode 8.String to Integer (atoi)](https://leetcode.com/problems/string-to-integer-atoi/)