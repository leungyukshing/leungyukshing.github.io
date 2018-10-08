---
title: LeetCode解题报告（十四）-- 503. Next Greater Element II
tags:
  - LeetCode
abbrlink: 27357
date: 2018-10-01 08:53:02
---
## Problem
Given a circular array (the next element of the last element is the first element of the array), print the Next Greater Number for every element. The Next Greater Number of a number x is the first greater number to its traversing-order next in the array, which means you could search circularly to find its next greater number. If it doesn't exist, output -1 for this number.

**Example 1**:
```
Input: [1,2,1]
Output: [2,-1,2]
Explanation: The first 1's next greater number is 2;
The number 2 can't find next greater number;
The second 1's next greater number needs to search circularly, which is also 2.
```

**Note**: The length of given array won't exceed 10000.
<!-- more -->

## Analysis
&emsp;&emsp;这是[Next Greater Element](https://leungyukshing.github.io/archives/LeetCode%E8%A7%A3%E9%A2%98%E6%8A%A5%E5%91%8A%EF%BC%88%E5%8D%81%E4%B8%89%EF%BC%89--%20496.%20Next%20Greater%20Element%20I.html)系列的第二题，与第一题相比，改进的地方在于题目给出的数组的**循环的**，其余部分与第一题基本一样，那么问题就转变为如何实现**循环遍历**以及确定**终止条件**。循环遍历可以操作下标，当下标到尾部是，手动设置回到头，终止条件就是再次访问到自己本身。记录的方式与第一题一样，同样使用`map`，非常简单。

## Solution
&emsp;&emsp;循环遍历的实现基于对下标的操作，当`j==num.size()`时，`j`设为0即可。终止条件就是`i != j`，这代表在数组中找不到比自己大的元素了。然后将每一个元素的`nextGreaterNumber`存入`map`中即可。

## Code
```C++
class Solution {
public:
    vector<int> nextGreaterElements(vector<int>& nums) {
        for (int i = 0; i < nums.size(); i++) {
            int nextGreaterNumber = -1;
            int j = i + 1;
            while (i != j) {
                if (j == nums.size()) {
                    j = 0;
                }
                if (i == j) {
                    break;
                }
                if (nums[j] > nums[i]) {
                    nextGreaterNumber = nums[j];
                    break;
                }
                j++;
            }
            m[i] = nextGreaterNumber;
        }

        vector<int> result;
        for (int i = 0; i < nums.size(); i++) {
            result.emplace_back(m[i]);
        }
        return result;
    }
private:
    map<int, int> m;
};
```

## Improved Solution
&emsp;&emsp;把自己的代码提交后，发现运行时间居然是最优代码的两倍，于是很好奇地去看了看大神写的代码。原来他对遍历过程做了优化，使用了栈做遍历，使用vector来记录对应关系。其代码如下：
```C++
class Solution {
public:
    vector<int> nextGreaterElements(vector<int>& nums) {
        int n = nums.size();
        vector<int> res(n, -1);
        stack<int> st;
        for (int i = 0; i < 2 * n; ++i) {
            int num = nums[i % n];
            while (!st.empty() && nums[st.top()] < num) {
                res[st.top()] = num; st.pop();
            }
            if (i < n) st.push(i);
        }
        return res;
    }
};
```
&emsp;&emsp;首先这里设置的遍历次数是整个数组的两倍，因为对于任何一个元素而言，其遍历的元素也是在这个数组里面，因此两次遍历可以覆盖所有情况。然后用到了栈，栈是用来存放数组的元素的，而`num`相当于用来寻找比当前元素大的一个变量。这样做的好处在于，使用栈可以提高效率，遍历的次数也将大大减少，不愧是一种好方法！

## Summary
&emsp;&emsp;这道题主要考察了循环遍历一个数组，自己实现的方法比较常规，虽然也能得出正确答案，但是效率和别人的相比还是差了不少。在理解完别人的代码后，也学到了使用栈的方式进行遍历，收获不少。这道题的分析就到这里，谢谢！
