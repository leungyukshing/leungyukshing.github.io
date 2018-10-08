---
title: LeetCode解题报告（十八）-- 76. Minimum Window Substring
tags:
  - LeetCode
abbrlink: 42166
date: 2018-10-08 15:07:26
---
## Problem
Given a string S and a string T, find the minimum window in S which will contain all the characters in T in complexity O(n).

**Example**:
```
Input: S = "ADOBECODEBANC", T = "ABC"
Output: "BANC"
```

**Note：**
  + If there is no such window in S that covers all characters in T, return the empty string `""`.
  + If there is such window, you are guaranteed that there will always be only one unique minimum window in S.
<!-- more -->

## Analysis
&emsp;&emsp;这道题考察的知识点是字符串的处理。第一眼看上去有点像字符串匹配的题目，但是难点在于要找到**最小**的匹配字符串。
&emsp;&emsp;第一种想法就是，找到所有匹配的，然后取长度最小的一个就是答案了。但是找到所有匹配的字符串难度很大，因为匹配的字符串可能会相互叠加，因此找到所有匹配的字符串需要很大的复杂度，还需要另外找一个容器存储，无论是时间还是空间都将消耗很多的资源。
&emsp;&emsp;于是我想到了第二种方法，就是一直顺着找，找到了匹配的之后再进行**裁剪**就可以了，每次裁剪后判断当前这个串是否最短，如果是，则保存下来，遍历结束后我们就能得到答案了。当然，在裁剪过程中为了保证`t`中的字符始终被匹配了，我们还需要另外的一些容器来记录这些信息。但基本思路很明确：遍历`s`，找到匹配串，进行裁剪，记录长度，比较长度，最后就可以得出答案。
&emsp;&emsp;记录匹配字符，我选择使用map这个数据结构，如果你对[My Calendar III](https://leungyukshing.github.io/archives/LeetCode%E8%A7%A3%E9%A2%98%E6%8A%A5%E5%91%8A%EF%BC%88%E5%85%AD%EF%BC%89--%20732.%20My%20Calendar%20III.html)还有印象的话，对这题的解决是很有帮助的。我们匹配一个字符就跟**打卡**是一个道理，匹配了就相当于**退场**，被裁剪掉了就相当于**进场**。有了这样一个计数的机制，我们就可以在匹配的时候和裁剪的时候保证匹配完整。

## Solution
&emsp;&emsp;首先使用map来存储匹配信息，每个record的`value`初始化为1，表明所有的字符均**未匹配**。当`s`中的一个字符被匹配后，对应的`value`减1；当`s`中的一个匹配字符被裁剪后，对应的`value`加1。
&emsp;&emsp;根据上述计数规则，我们遍历`s`。只有当当前字符是匹配字符的时候，才进行操作，因为检测到其他字符的时候不可能是最短匹配字符串的结尾，所以就直接将右游标`right_index++`。
&emsp;&emsp;当当前字符是匹配字符的时候，根据计数规则，首先将对应的`value--`。然后判断`value`是否**大于等于0**，也就是判断该字符是否被重复匹配，这里主要是用于统计**已匹配字符数**`matchedNum`。如果`matchedNum`等于`t`的大小，说明匹配已经完整，这个时候就需要对匹配出来的字符串进行**裁剪**。
&emsp;&emsp;**裁剪**从左游标`left_index`开始，使用`while`循环进行裁剪，`left_index`不断向右移。首先要判断当前字符串长度是否为最小，这里用`min`记录当前长度最小的字符床长度，将`right_index - left_index + 1`与`min`比较来得出最小的长度，然后使用`substr()`来获得最短匹配字符串，使用`result`记录。
&emsp;&emsp;裁剪的循环需要一个结束的判断，我们可以知道，裁剪**非匹配字符**并不会导致裁剪结束，只有当前字符是匹配字符，且这个字符被裁剪后将**恰好不满足条件**，这个临界点才是结束的情况。我们对匹配字符的`value++`，如果`value`大于0，则说明它肯定是1，说明回到了初始状态（未匹配状态），这个时候通过`matchedNum--`来退出`while`循环。

## Code
```C++
class Solution {
public:
    string minWindow(string s, string t) {
        map<char, int> counter;
        int matchedNum = 0;
        int left_index = 0;
        int min = s.size()+1;
        string result;
        for (int i = 0; i < t.size(); i++) {
            counter[t[i]]++;
        }

        for (int right_index = 0; right_index < s.size(); right_index++) {
            // matched
            if (counter.find(s[right_index]) != counter.end()) {
                counter[s[right_index]]--;

                // Ignore duplicate matched characters
                if (counter[s[right_index]] >= 0)
                    matchedNum++;

                while (matchedNum == t.size()) {
                    // reduce left side
                    if (right_index - left_index + 1 < min) {
                        min = right_index - left_index + 1;
                        result = s.substr(left_index, min);
                    }
                    // Set exit condition
                    if (counter.find(s[left_index]) != counter.end()) {
                        counter[s[left_index]]++;
                        if (counter[s[left_index]] > 0)
                            matchedNum--;
                    }
                    left_index++;
                }

            }
        }
        return result;
    }
};
```
**运行时间：**约16ms，超过55.51%的CPP代码。

## Summary
&emsp;&emsp;这题的难度不小，综合了匹配字符、最小长度等常见的算法题的考点。解决这题花费了不少时间，我的方法是先在纸上演算，主要找到一些关键的**转折点**，如这题中的**裁剪的结束点**等，把这些把握住了，其他的也就迎刃而解。本题的分析到这里了，谢谢您的支持！
