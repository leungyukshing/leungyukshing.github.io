---
title: LeetCode解题报告（十五）-- 556. Next Greater Element III
tags:
  - LeetCode
abbrlink: 33558
date: 2018-10-02 15:12:35
---
## Problem
Given a positive **32-bit** integer **n**, you need to find the smallest **32-bit** integer which has exactly the same digits existing in the integer **n** and is greater in value than n. If no such positive **32-bit** integer exists, you need to return -1.

**Example 1**:
```
Input: 12
Output: 21
```

**Example 2**:
```
Input: 21
Output: -1
```
<!-- more -->

## Analysis
&emsp;&emsp;这道题虽然是`Next Greater Element`系列的第三题，但是解题思路与方向和前两题有很大的不同。这道题要解决的问题是，利用给出的数中的数字，组成一个比输入大的数，且这个数是最小的。简单地理解就是，对给出的数作全排列，然后在这个全排列中找一个最小的比输入大的数。但是手动实现全排列显然效率很低，因此我考虑了另一种方法。
&emsp;&emsp;对于一个数而言，如果它本身从高位到低位是降序排列的（如54321），那么按照题目要求则找不到答案；如果它不是降序的，则存在一个**转折点**，即我们可以通过交换转折点与一个更大的数字来获取一个更大的数。那么我们如何确保这个数是最小的呢？很简单，由于我们修改了**转折点**，后面的数字部分将不会影响这个数与原来输入的大小关系，只需要作升序排序则是最小的了。
&emsp;&emsp;问题便转化为如何找到这个转折点？并且它和什么交换？前面提到，存在**转折点**的前提条件是低位有比它大的数，那么我们就可以将这个比它大的位和**转折点**相互交换，以获得一个更大的数，记录下**转折点**位置，后面部分作升序排序即可。

## Solution
&emsp;&emsp;考虑到算法中有很多逐位比较和排序，因此使用字符串来处理更加方便。我们的遍历都是从低位开始，这样确保了转折点的存在性。使用`index1`来记录第一个小于它低位的元素下标（即**转折点**），使用`index2`来记录第一个比**转折点**大的元素下标，然后交换二者，对`index1`后的部分进行排序，这样就完成了。
&emsp;&emsp;需要在第一遍找**转折点**的遍历结束后判断是否存在，若不存在（`index1 == -1`），则直接返回`-1`。

## Code
```C++
class Solution {
public:
    int nextGreaterElement(int n) {
        string num = to_string(n);

        int size = num.size();
        int index1, index2;
        // index1 is the first number smaller than its right
        // index2 is the first number that bigger than num[index1];
        for (index1 = size - 2 ; index1 >= 0; index1--) {
            if (num[index1] < num[index1 + 1]) {
                break;
            }
        }

        // greater element is not existed!
        if (index1 == -1) {
            return -1;
        }

        for (index2 = size - 1; index2 >= index1 + 1; index2--) {
            if (num[index2] > num[index1]) {
                char temp = num[index1];
                num[index1] = num[index2];
                num[index2] = temp;
                break;
            }
        }
        sort(num.begin() + index1 + 1, num.end());
        long result = stoll(num);
        if (result > INT_MAX) {
            return -1;
        }
        return result;
    }
};
```
**运行时间**：约0ms，超过约100%的CPP代码。

## Summary
&emsp;&emsp;能顺利解决这道题得益于之前碰到过类似的题目，使用这种位交换方法显然比全排列快很多。这道题的解法非常地巧妙，原理很基本，就是关于数位的一些比较，但是在算法中非常地有效，自己收获不少。本题的题析到这里，谢谢您的支持！
