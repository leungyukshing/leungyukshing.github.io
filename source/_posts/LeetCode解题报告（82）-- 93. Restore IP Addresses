---
title: LeetCode解题报告（82）-- 93. Restore IP Addresses
tags:
  - LeetCode
date: 2020-05-22 11:29:33
mathjax: true

---

## Problem

Given a string containing only digits, restore it by returning all possible valid IP address combinations.

A valid IP address consists of exactly four integers (each integer is between 0 and 255) separated by single points.

<!-- more -->

**Example:**

```
Input: "25525511135"
Output: ["255.255.11.135", "255.255.111.35"]
```

------

## Analysis

&emsp;&emsp;这道题目是与字符串相关的，又和IP地址有一定的结合。看到这道题第一反应就是遍历的复杂度很高，因为每一段的位数都不是固定的，所以后面的结果会依赖于前面的结果，例如example中后两段，如果第三段用了11，那么第四段就是135；而如果第三段用了111，第四段就是35。所以了解了题目的情况后，我觉得大概的方向是递归，把大问题切分为小问题解决。

&emsp;&emsp;我们来看切分后的小问题是什么？是给定一个字符串，判断它能否构成一个小段。构成小段的检查条件就是数值是否在0-255之间，**且不能以0开头**。判断的方法就是从头开始，每次截出一个小段，然后把剩下的部分交给下一个迭代做，当段数为4的时候，如果还有多余的字符串，则说明给出的字符串过长。

------

## Solution

&emsp;&emsp;实现的思路就是维护两个字符串和一个计数，两个字符串一个是已经判断过的合法的IP地址，另一个是待判断的。计数是标记当前在第几个段。每次都从待判断的字符串中往后截取出一个段，截取的区间的限制对应小段的检查条件。

&emsp;&emsp;需要特别注意的是拼接的时候第一段前面不需要加上`.`，截取的范围不能超过3位。

------

## Code

```c++
class Solution {
public:
    vector<string> restoreIpAddresses(string s) {
        vector<string> result;
        string ip;
        dfs(0, s, ip, result);
        return result;
    }
    
    void dfs(int idx, string remains, string ip, vector<string> &result) {
        int l = remains.size();
        if (idx == 4) {
            // last section
            if (l == 0) {
                // no more remains
                result.push_back(ip);
            }
            return;
        }
        
        for (int i = 1; i <= min(3, remains[0] == '0' ? 1: l); i++) {
            string temp = remains.substr(0, i);
            if (i == 3 && stoi(temp) > 255) {
                // invalid section value
                return;
            }
            dfs(idx + 1, remains.substr(i), ip + (idx == 0 ? "": ".") + temp, result);
        }
    }
};
```

------

## Summary

 &emsp;&emsp;这类题目看上去很好理解，但是实际解决还是需要技巧。考虑到IP地址的四段都是有相同的条件，所以可以考虑递归的方法来解决。希望这篇博客能够帮助到大家，谢谢！
