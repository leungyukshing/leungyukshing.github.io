---
title: LeetCode解题报告（265）-- 1649. Create Sorted Array through Instructions
tags:
  - LeetCode
mathjax: true
date: 2021-01-13 16:47:19

---

## Problem

Given an integer array `instructions`, you are asked to create a sorted array from the elements in `instructions`. You start with an empty container `nums`. For each element from **left to right** in `instructions`, insert it into `nums`. The **cost** of each insertion is the **minimum** of the following:

- The number of elements currently in `nums` that are **strictly less than** `instructions[i]`.
- The number of elements currently in `nums` that are **strictly greater than** `instructions[i]`.

For example, if inserting element `3` into `nums = [1,2,3,5]`, the **cost** of insertion is `min(2, 1)` (elements `1` and `2` are less than `3`, element `5` is greater than `3`) and `nums` will become `[1,2,3,3,5]`.

Return *the **total cost** to insert all elements from* `instructions` *into* `nums`. Since the answer may be large, return it **modulo** `109 + 7`

<!-- more -->

**Example 1:**

```
Input: instructions = [1,5,6,2]
Output: 1
Explanation: Begin with nums = [].
Insert 1 with cost min(0, 0) = 0, now nums = [1].
Insert 5 with cost min(1, 0) = 0, now nums = [1,5].
Insert 6 with cost min(2, 0) = 0, now nums = [1,5,6].
Insert 2 with cost min(1, 2) = 1, now nums = [1,2,5,6].
The total cost is 0 + 0 + 0 + 1 = 1.
```

**Example 2:**

```
Input: instructions = [1,2,3,6,5,4]
Output: 3
Explanation: Begin with nums = [].
Insert 1 with cost min(0, 0) = 0, now nums = [1].
Insert 2 with cost min(1, 0) = 0, now nums = [1,2].
Insert 3 with cost min(2, 0) = 0, now nums = [1,2,3].
Insert 6 with cost min(3, 0) = 0, now nums = [1,2,3,6].
Insert 5 with cost min(3, 1) = 1, now nums = [1,2,3,5,6].
Insert 4 with cost min(3, 2) = 2, now nums = [1,2,3,4,5,6].
The total cost is 0 + 0 + 0 + 0 + 1 + 2 = 3.
```

**Example 3:**

```
Input: instructions = [1,3,3,3,2,4,2,1,2]
Output: 4
Explanation: Begin with nums = [].
Insert 1 with cost min(0, 0) = 0, now nums = [1].
Insert 3 with cost min(1, 0) = 0, now nums = [1,3].
Insert 3 with cost min(1, 0) = 0, now nums = [1,3,3].
Insert 3 with cost min(1, 0) = 0, now nums = [1,3,3,3].
Insert 2 with cost min(1, 3) = 1, now nums = [1,2,3,3,3].
Insert 4 with cost min(5, 0) = 0, now nums = [1,2,3,3,3,4].
Insert 2 with cost min(1, 4) = 1, now nums = [1,2,2,3,3,3,4].
Insert 1 with cost min(0, 6) = 0, now nums = [1,1,2,2,3,3,3,4].
Insert 2 with cost min(2, 4) = 2, now nums = [1,1,2,2,2,3,3,3,4].
The total cost is 0 + 0 + 0 + 0 + 1 + 0 + 1 + 0 + 2 = 4.
```

**Constraints:**

- `1 <= instructions.length <= 105`
- `1 <= instructions[i] <= 105`

------

## Analysis

&emsp;&emsp;这是一道hard题目，可以说算是hard里面比较难的，这道题目有两种解法，这两种解法分别都是算法领域中非常重要的算法，这里我就简单介绍一下如何求解。

&emsp;&emsp;首先看一下题目的要求，把复杂的描述简化一下，就是给定一个数组，要求每个位置的cost，对`cost[i]`的定义是`min(在这个位置之前比nums[i]小的数的个数,在这个位置之前比nums[i]大的数的个数)`。而对于某个位置来说，只要我们知道前面有多少个数字比它小，还有知道有多少个数字和它一样大，那么就能计算出有多少个数字比它大，因此计算前者即可。所以求解的问题转化为：对每个位置，计算在这个位置之前比这个数字小的数的个数。

### 分治法

&emsp;&emsp;涉及到大小比较，我们就会想到能不能先进行排序。直观上看好像不可以，因为对整体排序的话，就丢失了位置的信息。整体排序不行的话，部分排序呢？比如说`[1, 5, 8, 9]`和`[2, 3, 6, 16, 20]`这两个部分，虽然整体上并没有顺序，但是它们各自有序，这个时候对我们查找在前面有多少个数字比该数字小就有很大帮助。以6为例，它只需要到前面一个数组中找到第一个比6大的数字，往前的都肯定是比6小的。所以它找到了8，下标是2，所以比6小的数字个数就有两个。这个时候看似能解决一部分问题，但是会有另一个疑问了，这样的方法能够解决在不同部分的，那和6在同一个部分的数字呢？（比如例子中的2和3）这还不简单，已经分成了两半，各自有序，这就是经典的**归并排序**！所以在上一步的时候，我们就已经统计好比6小的数字个数是2，然后在这一步，**再加上2**，所以记得这里是累加！

&emsp;&emsp;完成归并后我们就有了每个位置前比该位置小的数字的个数这个信息了，然后我们再遍历一边原数组，计算每个位置的cost。我们用一个数组去统计相同数字出现的次数，减去在这个位置前比这个数字小的数字的个数和相同数字出现的次数，得到的结果就是在这个位置前比这个数字大的数字的个数，最后两者取较小值即可。

### 树状数组（Binary Indexed Tree）

&emsp;&emsp;上面介绍了使用归并排序的思路去求解这道题目，理解起来是比较直观的，但是效率上还不是最优。这里再给大家介绍一种数据结构——树状数组BIT，这种数据结构就是专门解决这种类型的题目。

&emsp;&emsp;BIT的原理和实现细节比较复杂，我会在另外一篇博客和大家分享，这里只谈用法。BIT实际上是一个数组（**特别注意这个数组是1-index的**），数组的下标是数值，对应的值是这个数字出现的次数。比如1出现了2次，则`bitArray[1] = 2`。结合这道题目，假设我们知道`bitArr[1] = 2`，`bitArr[5] = 10`，如果要求数字8前面比它小的数字的个数，那么就是`2 + 10 = 12`。而BIT的特点就是能够高效地求出这个前缀和。

&emsp;&emsp;然后我们看看BIT提供了什么接口，主要有两个`update(int idx, int val)`和`sumRange(int i, int j)`。

- `update(int idx, int val)`：往BIT中插入一个元素，如果5出现了一次，则`update(5, 1)`；
- `sumRange(int i, intj)`：计算区间`[i,j]`的和。

&emsp;&emsp;所以回到这道题目，实际上我们只需要一次遍历数组，然后每次都update一下当前元素，出现的次数+1，然后计算`sumRange(0, nums[i] - 1)`和`sumRange(nums[i] + 1, size)`两者的极小值，最后累加起来即可。

------

## Solution

### 分治法

&emsp;&emsp;由于我们在求解过程中需要排序，因此需要多开一个空间`sorted`去存放排序后的结果。同时，我们求解的答案并不是最终有序的数组，而是一个能够记录每个位置前比该位置元素小的数字的个数，所以这里也需要开一个空间`cost`去存放。

### 树状数组

&emsp;&emsp;树状数组的实现不在这里详细解释，在打题时建议直接套用模版。模版封装好了两个接口`update()`和`sumRange()`。

------

## Code

### 分治

```c++
class Solution {
    int smaller[100001];
    int sorted[100001];
    int count[100001];
    int temp[100001];
    int M = 1e9 + 7;
public:
    int createSortedArray(vector<int>& instructions) {
        int size = instructions.size();
        
        for (int i = 0; i < size; i++) {
            sorted[i] = instructions[i];
        }
        helper(instructions, 0, size - 1);
        long long result = 0;
        for (int i = 0; i < size; i++) {
            result += min(smaller[i], i - smaller[i] - count[instructions[i]]);
            result %= M;
            count[instructions[i]]++;
        }
        return result;
    }
private:
    void helper(vector<int>& nums, int a, int b) {
        if (a >= b) {
            return;
        }
        int mid = (a + b) / 2;
        helper(nums, a, mid);
        helper(nums, mid + 1, b);
        
        for (int i = mid + 1; i <= b; i++) {
            auto iter = lower_bound(sorted + a, sorted + mid + 1, nums[i]);
            smaller[i] += iter - (sorted + a);
        }
        
        // merge
        int i = a, j = mid + 1, p = 0;
        while (i <= mid && j <= b) {
            if (sorted[i] <= sorted[j]) {
                temp[p] = sorted[i];
                i++;
            } else {
                temp[p] = sorted[j];
                j++;
            }
            p++;
        }
        while (i <= mid) {
            temp[p] = sorted[i];
            i++;
            p++;
        }
        while (j <= b) {
            temp[p] = sorted[j];
            j++;
            p++;
        }
        for (int i = 0; i < b - a + 1; i++) {
            sorted[a + i] = temp[i];
        }
    }
};
```

### 树状数组

```c++
const int MAX_N = 1e5;

class Solution {
public:
    int createSortedArray(vector<int>& instructions) {
        long long result = 0;
        int size = instructions.size();
        for (int i = 0; i < size; i++) {
            update(instructions[i], 1);
            long long a = sumRange(1, instructions[i] - 1);
            long long b = sumRange(instructions[i] + 1, MAX_N);
            result += min(a, b);
            result %= M;
        }
        return result;
    }
private:
    int bitArr[MAX_N + 1]; // 1-index
    
    int lowbit(int x) { return x & (-x); }
    int M = 1e9 + 7;
    void update(int i, long long d) {
        while (i <= MAX_N) {
            bitArr[i] += d;
            bitArr[i] %= M;
            i += lowbit(i);
        }
    }
    
    long long queryPreSum(int i) {
        long long ans = 0;
        while (i) {
            ans += bitArr[i];
            ans %= M;
            i -= lowbit(i);
        }
        return ans;
    }
    
    long long sumRange(int i, int j) {
        return queryPreSum(j) - queryPreSum(i - 1);
    }
};
```

------

## Summary

&emsp;&emsp;这道题目的两种解法都是非常著名的算法，第一种解法是归并排序，虽然这个方法听得很多，但实际能应用的场景不多，这道题算是为数不多能够应用的场景。第二种解法时BIT，这是一个非常有趣的数据结构，因为原理比较复杂没办法在这里一次过和大家分享，之后一定会出一篇有关BIT的博客，希望大家持续关注！这道题这道题目的分享到这里，谢谢您的支持！
