---
title: LeetCode解题报告（461）-- 2094. Finding 3-Digit Even Numbers
mathjax: true
date: 2021-12-26 22:13:16
---

## Problem

You are given an integer array `digits`, where each element is a digit. The array may contain duplicates.

You need to find **all** the **unique** integers that follow the given requirements:

- The integer consists of the **concatenation** of **three** elements from `digits` in **any** arbitrary order.
- The integer does not have **leading zeros**.
- The integer is **even**.

For example, if the given `digits` were `[1, 2, 3]`, integers `132` and `312` follow the requirements.

Return *a **sorted** array of the unique integers.*

<!-- more -->

**Example 1:**

```
Input: digits = [2,1,3,0]
Output: [102,120,130,132,210,230,302,310,312,320]
Explanation: All the possible integers that follow the requirements are in the output array. 
Notice that there are no odd integers or integers with leading zeros.
```

**Example 2:**

```
Input: digits = [2,2,8,8,2]
Output: [222,228,282,288,822,828,882]
Explanation: The same digit can be used as many times as it appears in digits. 
In this example, the digit 8 is used twice each time in 288, 828, and 882. 
```

**Example 3:**

```
Input: digits = [3,7,5]
Output: []
Explanation: No even integers can be formed using the given digits.
```



**Constraints:**

- `3 <= digits.length <= 100`
- `0 <= digits[i] <= 9`

---

## Analysis

&emsp;&emsp;题目给出了一个数组`digits`，要求我们用里面的数组去组成三位偶数，要求把所有的可能都返回出来。这个问题第一反应使用backtrace，但是这是一道easy题啊，肯定有更加快速的方法。首先因为是三位数而且是偶数，本身解的范围就很小，我们只需要遍历一遍，看看这些数是否符合要求就可以了。

&emsp;&emsp;问题就变成了哪些数是符合要求的呢？因为要用`digits`中的数字组成，而且里面可能有重复出现的数字，所以这里我们直接用一个map先统计freq，然后对于遍历的每一个数也计算freq，对比两个freq就能判断是否合法。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    vector<int> findEvenNumbers(vector<int>& digits) {
        vector<int> count(10, 0);
        for (int &digit: digits) {
            ++count[digit];
        }
        
        vector<int> result;
        
        for (int i = 100; i < 999; i += 2) {
            int temp = i;
            vector<int> freq(10, 0);
            
            while (temp) {
                ++freq[temp % 10];
                temp /= 10;
            }
            
            bool flag = true;
            for (int j = 0; j < 10; ++j) {
                if (freq[j] > count[j]) {
                    flag = false;
                    break;
                }
            }
            
            if (flag) {
                result.push_back(i);
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目主要是使用了map来解决数字的出现问题。这道题目的分享到这里，感谢你的支持！
