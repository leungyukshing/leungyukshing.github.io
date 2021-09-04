---
title: LeetCode解题报告（399) -- 4. Median of Two Sorted Arrays
mathjax: true
abbrlink: 48744
date: 2021-09-03 22:44:05
---

## Problem

Given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively, return **the median** of the two sorted arrays.

The overall run time complexity should be `O(log (m+n))`.

<!-- more -->

**Example 1:**

```
Input: nums1 = [1,3], nums2 = [2]
Output: 2.00000
Explanation: merged array = [1,2,3] and median is 2.
```

**Example 2:**

```
Input: nums1 = [1,2], nums2 = [3,4]
Output: 2.50000
Explanation: merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5.
```

**Example 3:**

```
Input: nums1 = [0,0], nums2 = [0,0]
Output: 0.00000
```

**Example 4:**

```
Input: nums1 = [], nums2 = [1]
Output: 1.00000
```

**Example 5:**

```
Input: nums1 = [2], nums2 = []
Output: 2.00000
```

**Constraints:**

- `nums1.length == m`
- `nums2.length == n`
- `0 <= m <= 1000`
- `0 <= n <= 1000`
- `1 <= m + n <= 2000`
- `-106 <= nums1[i], nums2[i] <= 106`

------

## Analysis

&emsp;&emsp;这是一道求中位数的题目，一看题目难度是hard就感觉不太妙，因为简单的求中位数，为什么会是hard？题目给出了两个有序的数组，要求找出这两个有序数组的中位数。最暴力的解法就是把这两个数组都合并，然后求中位数。但是题目还给了一个时间复杂度限制，要求是$O(log(m + n))$，这就说明上面这种暴力的解法不可行，要换一种思路。

&emsp;&emsp;直接从复杂度的要求下手，因为有log，而且这道题目还是数组，所以可以直接往二分的方向去思考。如何通过二分去找到中位数呢？所谓中位数就直观的理解就是有序数组中位置排在最中间的数字，假设我们现在有`A`（长度为`m`）和`B`（长度为`n`）两个有序数组，我们把`A`切分成两部分`[A[0], A[i - 1]]`和`[A[i], A[m - 1]]`，把`B`切分成两部分`[B[0], B[j - 1]]`和`[B[j], B[n - 1]]`。然后把`[A[0], A[i - 1]]`和`[B[0], B[j - 1]]`这两部分统称为`left`，把`[A[i], A[m - 1]]`和`[B[j], B[n - 1]]`统称为`right`。

&emsp;&emsp;当`m + n `为偶数时，如果满足以下两个条件：

```
len(left) = len(right) => i + j = m - i + n - j => j = (m + n) / 2 - i
max(left) <= min(right) => max(A[i - 1], B[j - 1]) <= min(A[i], B[j])
```

&emsp;&emsp;则中位数为`(max(left) + min(right)) / 2`，即`(max(A[i - 1], B[j - 1])) / 2`。上面第一个条件保证了两边的元素数量相等，第二个条件保证了`left`整体都比`right`要小，在这两个条件满足后，取左边的最大值和右边的最小值计算平均值就是两个数组的中位数。

&emsp;&emsp;同理，当`m + n `为奇数时，如果满足以下两个条件：

```
len(left) = len(right) + 1 => j = (m + n + 1) / 2 - i
max(left) <= min(right) => max(A[i - 1], B[j - 1]) <= min(A[i], B[j])
```

&emsp;&emsp;则中位数为`max(left) = max(A[i - 1], B[j - 1])`。第二个条件和上面偶数情况是一样的，不同的地方在于第一个条件，因为奇数的情况下，我们把多出来的一个元素划分的`left`，因此`left`最大的那个元素就是多出来的那个元素，也就是中位数。

&emsp;&emsp;在coding过程中，我们可以统一第一个条件，即`j = (m + n + 1) / 2 - i`，因为在偶数情况下，由于`1/2 = 0.5`，在C/C++中结果是0，所以并不影响。

&emsp;&emsp;这里还需要注意，因为`0 <= j <= n`，所以还需要计算出`m`和`n`的大小关系，只需要把上面`j = (m + n + 1) / 2 - i`代入到不等式中就能得出结果。这里直接给出答案：`n >= m`。所以我们在处理时要保证`A`的长度较小，如果有不符合要求的，交换一下即可。

&emsp;&emsp;然后我们再来看第二个条件，要保证`max(A[i - 1], B[j - 1]) <= min(A[i], B[j])`。由于`A`和`B`都是分别有序的，所以天然地有`A[i - 1] < A[i]`以及`B[j - 1] < B[j]`。因此在运算中我们要保证的是`A[i - 1] < B[j]`和`B[j - 1] < A[i]`。所以有以下几种情况：

1. `A[i - 1] > B[j]`：此时说明`i`太大了，因此需要减小`i`；
2. `B[j - 1] > A[i]`：此时说明`i`太小了，因此需要增加`i`；
3. `i = 0`，因为左边是`[A[0], A[i - 1]]`，所以当`i = 0`意味着左边没有任何元素，因此`max(left) = B[j - 1]`；
4. `i == m`，因为右边是`A[i], A[m - 1]`，所以当`i == m`意味着右边没有任何元素，因此`min(right) = B[j]`；
5. `j == 0`，因为左边是`[B[0], B[j - 1]]`，所以当`j == 0`意味着左边没有任何元素，因此`max(left) = A[i - 1]`；
6. `j == n`，因为右边是`[B[j], B[n - 1]]`，所以当`j == n`意味着右边没有任何元素，因此`min(right) = A[i]`

&emsp;&emsp;这样一来两个条件我们都能维护了，最后的问题就是怎么去遍历。实际上这就是一个调整`i`的过程（也是调整`j`，但因为可以用`i`表示`j`，所以相当于我们只有一个变量），而且题目也说了要log，很明显就是二分了。因为上面保证了`A`是较小的，同时我们只对`A`做二分，因此时间复杂度就是$O(log(min(m, n)))$。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    double findMedianSortedArrays(vector<int>& A, vector<int>& B) {
        int m = A.size();
        int n = B.size();
        if (m > n) {
            // ensure n >= m
            return findMedianSortedArrays(B, A);
        }
        
        int iLeft = 0, iRight = m;
        while (iLeft <= iRight) {
            int i = (iLeft + iRight) / 2;
            int j = (m + n + 1) / 2 - i;
            if (j != 0 && i != m && B[j - 1] > A[i]) {
                iLeft = i + 1;
            } else if (i != 0 && j != n && A[i - 1] > B[j]) {
                iRight = i - 1;
            } else {
                int maxLeft = 0;
                if (i == 0) {
                    maxLeft = B[j - 1];
                } else if (j == 0) {
                    maxLeft = A[i - 1];
                } else {
                    maxLeft = max(A[i - 1], B[j - 1]);
                }
                
                // odd
                if ((m + n) % 2 == 1) {
                    return maxLeft;
                }
                
                int minRight = 0;
                if (i == m) {
                    minRight = B[j];
                } else if (j == n) {
                    minRight = A[i];
                } else {
                    minRight = min(A[i], B[j]);
                }
                return (maxLeft + minRight) / 2.0;
            }
        }
        return 0.0;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目非常有意思，实际上还有一种解法，就是采用找第`k`大数字来实现的，其本质是在两个有序数组中做二分查找，每次在某个数组中排除掉`k / 2`个元素，所以这种做法的复杂度也是题目的最低要求$O(log(m + n))$。这道题目的分享到这里，感谢你的支持！
