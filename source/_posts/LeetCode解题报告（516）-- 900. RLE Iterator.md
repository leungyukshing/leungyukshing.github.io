---
title: LeetCode解题报告（516）-- 900. RLE Iterator
mathjax: true
date: 2022-01-13 23:30:13
---

## Problem

We can use run-length encoding (i.e., **RLE**) to encode a sequence of integers. In a run-length encoded array of even length `encoding` (**0-indexed**), for all even `i`, `encoding[i]` tells us the number of times that the non-negative integer value `encoding[i + 1]` is repeated in the sequence.

- For example, the sequence `arr = [8,8,8,5,5]` can be encoded to be `encoding = [3,8,2,5]`. `encoding = [3,8,0,9,2,5]` and `encoding = [2,8,1,8,2,5]` are also valid **RLE** of `arr`.

Given a run-length encoded array, design an iterator that iterates through it.

Implement the `RLEIterator` class:

- `RLEIterator(int[] encoded)` Initializes the object with the encoded array `encoded`.
- `int next(int n)` Exhausts the next `n` elements and returns the last element exhausted in this way. If there is no element left to exhaust, return `-1` instead.

<!-- more -->

**Example 1:**

```
Input
["RLEIterator", "next", "next", "next", "next"]
[[[3, 8, 0, 9, 2, 5]], [2], [1], [1], [2]]
Output
[null, 8, 8, 5, -1]

Explanation
RLEIterator rLEIterator = new RLEIterator([3, 8, 0, 9, 2, 5]); // This maps to the sequence [8,8,8,5,5].
rLEIterator.next(2); // exhausts 2 terms of the sequence, returning 8. The remaining sequence is now [8, 5, 5].
rLEIterator.next(1); // exhausts 1 term of the sequence, returning 8. The remaining sequence is now [5, 5].
rLEIterator.next(1); // exhausts 1 term of the sequence, returning 5. The remaining sequence is now [5].
rLEIterator.next(2); // exhausts 2 terms, returning -1. This is because the first term exhausted was 5,
but the second term did not exist. Since the last term exhausted does not exist, we return -1.
```



**Constraints:**

- `2 <= encoding.length <= 1000`
- `encoding.length` is even.
- `0 <= encoding[i] <= 109`
- `1 <= n <= 109`
- At most `1000` calls will be made to `next`.

---

## Analysis

&emsp;&emsp;这是一道数组的iterator题目，题目给出一个`encoding`数据，长度是偶数，`encoding[i]`表示的是`encoding[i + 1]`出现的次数，所以可以理解为相邻两个数字配对出现，前一个是次数，后一个是数字本身。

&emsp;&emsp;因此对于每对组合，我只需要把下标定位在第一个位置就可以得到两个信息了，而且这个下标只会指向偶数下标的位置，所以判断结束的条件是`idx > size - 2`。然后我们来看关键的逻辑是`next(int n)`这个方法，它要求的是向后遍历`n`个数字并返回最后的那个数字，那这里有两种情况，第一种是`n`比当前这个数字出现的次数要小，这个处理很容易，返回的结果就是当前的数字，然后更新下频率为`arr[idx] -= n`就可以了；第二种比较麻烦，因为当前这个数字的次数不足`n`，所以`idx`要往后移动，每次移动2位，移动前要把当前的频率`arr[idx]`置为0，**同时每次移动完都要判断是否已经越界**。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class RLEIterator {
public:
    RLEIterator(vector<int>& encoding) {
        arr = encoding;
        idx = 0;
    }
    
    int next(int n) {
        if (idx > arr.size() - 2) {
            return -1;
        }
        
        while (1) {
            if (n > arr[idx]) {
                n -= arr[idx];
                arr[idx] = 0;
                idx += 2;
                
                if (idx > arr.size() - 2) {
                    return -1;
                }
            } else {
                arr[idx] -= n;
                return arr[idx + 1];
            }
        }
    }
private:
    vector<int> arr;
    int idx;
};

/**
 * Your RLEIterator object will be instantiated and called as such:
 * RLEIterator* obj = new RLEIterator(encoding);
 * int param_1 = obj->next(n);
 */
```

------

## Summary

&emsp;&emsp;这道题目难度比前两道要小，主要就是处理`n`比当前元素的频率大的情况，需要向后移动`idx`。这道题目的分享到这里，感谢你的支持！

