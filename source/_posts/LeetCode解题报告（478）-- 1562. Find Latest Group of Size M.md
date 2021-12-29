---
title: LeetCode解题报告（478）-- 1562. Find Latest Group of Size M
mathjax: true
date: 2021-12-29 17:44:57
---

## Problem

Given an array `arr` that represents a permutation of numbers from `1` to `n`.

You have a binary string of size `n` that initially has all its bits set to zero. At each step `i` (assuming both the binary string and `arr` are 1-indexed) from `1` to `n`, the bit at position `arr[i]` is set to `1`.

You are also given an integer `m`. Find the latest step at which there exists a group of ones of length `m`. A group of ones is a contiguous substring of `1`'s such that it cannot be extended in either direction.

Return *the latest step at which there exists a group of ones of length **exactly*** `m`. *If no such group exists, return* `-1`.

<!-- more -->

**Example 1:**

```
Input: arr = [3,5,1,2,4], m = 1
Output: 4
Explanation: 
Step 1: "00100", groups: ["1"]
Step 2: "00101", groups: ["1", "1"]
Step 3: "10101", groups: ["1", "1", "1"]
Step 4: "11101", groups: ["111", "1"]
Step 5: "11111", groups: ["11111"]
The latest step at which there exists a group of size 1 is step 4.
```

**Example 2:**

```
Input: arr = [3,1,5,4,2], m = 2
Output: -1
Explanation: 
Step 1: "00100", groups: ["1"]
Step 2: "10100", groups: ["1", "1"]
Step 3: "10101", groups: ["1", "1", "1"]
Step 4: "10111", groups: ["1", "111"]
Step 5: "11111", groups: ["11111"]
No group of size 2 exists during any step.
```



**Constraints:**

- `n == arr.length`
- `1 <= m <= n <= 105`
- `1 <= arr[i] <= n`
- All integers in `arr` are **distinct**.

---

## Analysis

&emsp;&emsp;这道题目给出一个数组`arr`，包含`[1,n]`一共`n`个元素，还有一个值`m`。题目的操作是从一个长度为`n`的全0字符串开始，每次把第`arr[i]`上的0改为1，问第几步之后（latest，即最新）存在连续`m`个1的substring。这个是一个和子串以及长度有关的题目，但因为我们的操作顺序是按照`arr`，也不一定是连续的，所以很难用sliding window。

&emsp;&emsp;那么突破口就只能是在长度上做文章了，我们可以用一个数组记录子串的长度，如何用一个数组记录子串长度呢？这里有一个技巧就是只更新`leftMost`和`rightMost`。比如说`0100`，那么对应的长度就是`[0, 1, 0, 0]`。如果我把`i = 2`改成1，那么就更新为`[0, 2, 2, 0]`，更general一点就是，当我改变`a = arr[i]`这个位置是，我先找到左边连续1的个数是`left = length[a - 1]`，以及右边连续1的个数`right = length[a + 1]`。加上当前的这个位置，整个子串的左边界就是`a - left`，有边界就是`a + right`，更新这两个位置为`left + right + 1`。

&emsp;&emsp;上面说了怎么维护长度信息，那怎么判断是否存在有长度为`m`的连续1子串呢？我们只需要每次反转一个位置后，检查先反转完有没有生成长度为`m`的即可，因为要维护latest，所以只要有都更新，从前往后的遍历顺序保证了答案是最近的步骤。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    int findLatestStep(vector<int>& arr, int m) {
        int result = -1;
        int n = arr.size();
        vector<int> length(n + 2, 0), count(n + 1, 0);
        for (int i = 0; i < n; ++i) {
            int a = arr[i];
            int left = length[a - 1], right = length[a + 1];
            length[a] = length[a - left] = length[a + right] = left + right + 1;
            --count[left];
            --count[right];
            ++count[length[a]];
            
            if (count[m]) {
                result = i + 1;
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目的技巧之前用的比较少，所以我认为是非常值得总结的，通过维护子串两端的值去维护长度。这道题目的分享到这里，感谢你的支持！

