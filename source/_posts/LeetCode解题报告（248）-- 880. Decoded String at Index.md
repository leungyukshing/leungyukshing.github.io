---
title: LeetCode解题报告（248）-- 880. Decoded String at Index
tags:
  - LeetCode
mathjax: true
abbrlink: 32373
date: 2020-12-20 22:34:21
---

## Problem

An encoded string `S` is given.  To find and write the *decoded* string to a tape, the encoded string is read **one character at a time** and the following steps are taken:

- If the character read is a letter, that letter is written onto the tape.
- If the character read is a digit (say `d`), the entire current tape is repeatedly written `d-1` more times in total.

Now for some encoded string `S`, and an index `K`, find and return the `K`-th letter (1 indexed) in the decoded string.

<!-- more -->

**Example 1:**

```
Input: S = "leet2code3", K = 10
Output: "o"
Explanation: 
The decoded string is "leetleetcodeleetleetcodeleetleetcode".
The 10th letter in the string is "o".
```

**Example 2:**

```
Input: S = "ha22", K = 5
Output: "h"
Explanation: 
The decoded string is "hahahaha".  The 5th letter is "h".
```

**Example 3:**

```
Input: S = "a2345678999999999999999", K = 1
Output: "a"
Explanation: 
The decoded string is "a" repeated 8301530446056247680 times.  The 1st letter is "a".
```

**Constraints:**

- `2 <= S.length <= 100`
- `S` will only contain lowercase letters and digits `2` through `9`.
- `S` starts with a letter.
- `1 <= K <= 10^9`
- It's guaranteed that `K` is less than or equal to the length of the decoded string.
- The decoded string is guaranteed to have less than `2^63` letters.

------

## Analysis

&emsp;&emsp;这是一道字符串解密的题目，题目给出字符串解密的方法，然后要求我们找到解密后的第`K`个字符。解密的方法也很简单，第一反应就是直接decode出来然后选第`K`个。这个想法太天真了，oj直接内存爆了，所以还要想一个优化的方法。

&emsp;&emsp;首先我们没有必要把解密后的整个字符串都记录下来，因为我们最后只是用到一个字符，所以在存储上可以优化。然后我们来看如何通过这个`K`来定位到字符。根据decode的特点，我们会发现实际上解密后的字符串是各种字符串的重复，这给我们定位字符带来很大的方便。例如字符串`abc2`解密后就是`abcabc`，如果我要在后者找到第`K = 5`个字符的话，因为字符串`abc`的长度是3，重复两次，所以用$5 \% 3$，就能找到实际上是在`abc`中的第二个字符，也就是`b`。回过头来看，我们怎么知道字符串的长度是3呢？这是因为我们首先要正向解密，计算到最后的字符串长度是6，然后再逆向，找到第一个是2，这就意味着前面部分重复了2次，所以$6 / 2$就是前面部分的长度了。

&emsp;&emsp;用上面这个例子就说明了整个思路，首先是从前向后进行解密，当解密到长度超过了`K`，后面的就不需要处理。因为这个长度往往都会大于`K`，所以我们需要往前再走一遍，定位到`K`在原字符串对应的字符。处理的规则是：

1. 遇到数字的时候，首先用当前长度`count`除以重复次数，得到前面字符串的长度`count_net`，然后用`K % count_new`得到`K`在前一个字符串中对应的下标位置；
2. 遇到字母时，如果`K % count == 0`，说明找到了`K`在最原始的字符串中的字符，直接返回；
3. 否则`count--`向前寻找。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    string decodeAtIndex(string S, int K) {
        long i = 0, count = 0; // length
        for (; count < K; i++) {
            count = isdigit(S[i]) ? count * (S[i] - '0') : count + 1;
        }
        
        while (i--) {
            if (isdigit(S[i])) {
                count /= (S[i] - '0');
                K %= count;
            } else {
                if (K % count == 0) {
                    return string(1, S[i]);
                } else {
                    count--;
                }
            }
        }
        return "not_found";
    }
};
```

------

## Summary

&emsp;&emsp;这道题是比较难的字符串题目，难点在于限制了存储空间之后，需要通过下标来寻找对应的字符。我们需要从decode的特点入手，找到字符串具有重复的特点，然后结合除法和取余来定位。这道题目的分享到这里，谢谢您的支持！
