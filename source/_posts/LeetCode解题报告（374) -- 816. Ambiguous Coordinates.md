---
title: LeetCode解题报告（374) -- 816. Ambiguous Coordinates
tags:
  - LeetCode
mathjax: true
date: 2021-05-16 14:05:32

---

## Problem

We had some 2-dimensional coordinates, like `"(1, 3)"` or `"(2, 0.5)"`. Then, we removed all commas, decimal points, and spaces and ended up with the string s.

- For example, `"(1, 3)"` becomes `s = "(13)"` and `"(2, 0.5)"` becomes `s = "(205)"`.

Return *a list of strings representing all possibilities for what our original coordinates could have been*.

Our original representation never had extraneous zeroes, so we never started with numbers like `"00"`, `"0.0"`, `"0.00"`, `"1.0"`, `"001"`, `"00.01"`, or any other number that can be represented with fewer digits. Also, a decimal point within a number never occurs without at least one digit occurring before it, so we never started with numbers like `".1"`.

The final answer list can be returned in any order. All coordinates in the final answer have exactly one space between them (occurring after the comma.)

<!-- more -->

**Example 1:**

```
Input: s = "(123)"
Output: ["(1, 2.3)","(1, 23)","(1.2, 3)","(12, 3)"]
```

**Example 2:**

```
Input: s = "(0123)"
Output: ["(0, 1.23)","(0, 12.3)","(0, 123)","(0.1, 2.3)","(0.1, 23)","(0.12, 3)"]
Explanation: 0.0, 00, 0001 or 00.01 are not allowed.
```

**Example 3:**

```
Input: s = "(00011)"
Output: ["(0, 0.011)","(0.001, 1)"]
```

**Example 4:**

```
Input: s = "(100)"
Output: ["(10, 0)"]
Explanation: 1.0 is not allowed.
```



**Constraints:**

- `4 <= s.length <= 12`
- `s[0] == '('` and `s[s.length - 1] == ')'`.
- The rest of `s` are digits.

------

## Analysis

&emsp;&emsp;这是一道字符串类型的题目，题目给出一个字符串，要求我们通过添加`,`和小数点，构造出不同的坐标。要求不能有多余的0、小数点前面必须有一位数字等。这种题目的难点在于考虑很多边界的case，如何对字符串进行合理的拆分。根据题目的一些限制条件我们知道，比较麻烦的是0，因为0放在开头和结尾都有很多的限制。我们先来看下面这些edge case：

1. 不能有0开头的，长度大于1的整数，既不能有00、01、001这种；
2. 不能有0结尾的小数，如0.0、0.10这种；
3. 小数的整数位，如果以0开头，则只能有0一个数字；

&emsp;&emsp;总结上面几种情况：

1. 如果字符串长度为空，直接返回空；
2. 如果字符串长度大于1，第一位是0，最后一位不是0，那么必须在第一个0后面加小数点返回；
3. 如果字符串长度大于1，首尾都是0，那么就不可以分；
4. 如果长度大于1，第一位不是0，最后一位是0，则必须在最后一个0点的前面添加小数点返回；
5. 一般情况，我们把中间的每个位置都添加一个小数点。

&emsp;&emsp;上面解决了数字合法性的问题，然后就来看如何组成坐标了。实际上把题目给出的字符串拆成两部分，给自去产生合法的数组，然后中间用`,`就可以组成坐标了。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    vector<string> ambiguousCoordinates(string S) {
        vector<string> res;
        int n = S.size();
        for (int i = 1; i < n - 2; ++i) {
            vector<string> A = findAll(S.substr(1, i)), B = findAll(S.substr(i + 1, n - 2 - i));
            for (auto &a : A) {
                for (auto &b : B) {
                    res.push_back("(" + a + ", " + b + ")");
                }
            }
        }
        return res;
    }
    vector<string> findAll(string S) {
        int n = S.size();
        if (n == 0 || (n > 1 && S[0] == '0' && S[n - 1] == '0')) return {};
        if (n > 1 && S[0] == '0') return {"0." + S.substr(1)};
        if (S[n - 1] == '0') return {S};
        vector<string> res{S};
        for (int i = 1; i < n; ++i) res.push_back(S.substr(0, i) + "." + S.substr(i));
        return res;
    }
};
```

------

## Summary

&emsp;&emsp;这类型题目主要考查的还是对题目理解，往往这类题目都需要积累，解题时主要的精力花在了找edge case上。这道题目的分享到这里，感谢你的支持！
