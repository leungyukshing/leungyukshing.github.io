---
title: LeetCode解题报告（410) -- 273. Integer to English Words
mathjax: true
date: 2021-12-11 16:06:42
---

## Problem

Convert a non-negative integer `num` to its English words representation.

<!-- more -->

**Example 1:**

```
Input: num = 123
Output: "One Hundred Twenty Three"
```

**Example 2:**

```
Input: num = 12345
Output: "Twelve Thousand Three Hundred Forty Five"
```

**Example 3:**

```
Input: num = 1234567
Output: "One Million Two Hundred Thirty Four Thousand Five Hundred Sixty Seven"
```

**Example 4:**

```
Input: num = 1234567891
Output: "One Billion Two Hundred Thirty Four Million Five Hundred Sixty Seven Thousand Eight Hundred Ninety One"
```

**Constraints:**

- `0 <= num <= 231 - 1`

------

## Analysis

&emsp;&emsp;这道题目要求把一个数字转化为英语的文字，是一道和字符串拼接相关的题目。一开始没什么好的突破点，就先直接看看hint了。提示给的信息很明确，分成三位数来做，因为每个三位数都有自己的读法，然后再加上这个三位数所在的位（Thousand、Million和Billion）就是最后正确的读法。比如example2，我们把`12345`分为`12`和`345`，那么对于`12`来说，它本身的读法是Twelve，加上所在的位是Thousand，连起来就是Twelve Thousand了。这个突破口找到了，接下来我们来看细节。数字转英文读法最麻烦的就是十位的读法，这里有个规律：**小于20的话是有特殊的读法的，而大于等于20则是无脑直接拼凑十位和个位**。例如13是Thirteen，而23是Twenty Three。

&emsp;&emsp;分析到这里我们就可以动手写了，我们从最低的三位开始处理，通过取余的方法获取最低的三位。假设我们已经处理完最低的三位了，我们接着处理第二个三位，然后每两段之间进行连接，加上当前三位对应的位（Thousand，Million和Billion），最后去除掉最后的空格已经判断是否为空即可。

&emsp;&emsp;然后我们再focus回三位数的处理逻辑。我们首先拿到百位的数字`a = num / 100`，然后拿到十位和个位的数字`b = num % 100`（注意必须是十位和个位一起，用来判断是否超过20），最后还要拿到单独个位的数字`c = num % 10`。接着判断`b`和20的大小关系决定读法。最后，如果`a`不为0，还需要加上百位的读法。

## Solution

&emsp;&emsp;这道题目因为有许多需要通过数字去找到英文读法，所以一个很好的代码实现就是通过数组去做一个mapping，快捷又清晰。

------

## Code

```c++
class Solution {
public:
    string numberToWords(int num) {
        string result = helper(num % 1000);
        
        vector<string> v  = {"Thousand", "Million", "Billion"};
        for (int i = 0; i < 3; ++i) {
            num /= 1000;
            result = num % 1000 ? helper(num % 1000) + " " + v[i] + " " + result: result;
        }
        while (result.back() == ' ') {
            result.pop_back();
        }
        return result.empty() ? "Zero": result;
    }
private:
    string helper(int num) {
        vector<string> v1 = {"", "One", "Two", "Three", "Four", "Five", "Six", "Seven", "Eight", "Nine", "Ten",  "Eleven", "Twelve", "Thirteen", "Fourteen", "Fifteen", "Sixteen", "Seventeen", "Eighteen", "Nineteen"};
        vector<string> v2 = {"", "", "Twenty", "Thirty", "Forty", "Fifty", "Sixty", "Seventy", "Eighty", "Ninety"};
        
        string result = "";
        int a = num / 100, b = num % 100, c = num % 10;
        result = b < 20 ? v1[b]: v2[b / 10] + (c ? " " + v1[c] : "");
        if (a > 0) {
            result = v1[a] + " Hundred" + (b ? " " + result: "");
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目难度是hard，但其实算法上并没有很难的地方，这道题目关键的地方就在于分成3位处理的思路，以及对十位数的特殊处理。这道题目的分享到这里，感谢你的支持！

