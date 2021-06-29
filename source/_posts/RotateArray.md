---
title: 旋转数组的二分查找
abbrlink: 28301
date: 2020-04-18 14:18:37
tags:
---

## 题目描述

&emsp;&emsp;二分查找相信大家都不陌生，它是一种针对于**有序数组**的高效搜索算法。然而对于另一种特殊的数据——**旋转数组**，尽管里面并不是一个有序数组，但是我们仍然可以利用其**部分有序性**，使用二分查找。

<!-- more -->

&emsp;&emsp;定义旋转数组为：已知有序数组`a[N]`，从中间某个为`k`分开，然后将前后两部分互换，得到的新的数组。如`a[] = {1, 2, 5, 7, 9, 10, 15}`，从`k = 4`处分开，得到旋转数组为`a[] = {9, 10, 15, 1, 2, 5, 7}`。

---

## 思路

&emsp;&emsp;我们可以利用旋转数组中的部分有序性来进行二分查找。依然是从中间一分为二，可以知道至少有一半是有序的。如果元素落在了有序的一半，那么就按照二分的方式继续查找；反之，则继续对无序的部分进行二分，直至找到有序的部分。

---

## 实现

```c++
#include <iostream>
#include <vector>
using namespace std;

int binarySearchInRotateArray(vector<int> arr, int target) {
  int low = 0, high = arr.size() - 1, mid;
  while (low <= high) {
    mid = low + ((high - low) >> 1);
    if (arr[mid] == target) {
      return mid;
    }
    if (arr[mid] >= arr[low]) {
      if (target < arr[mid] && target >= arr[low]) {
        high = mid - 1;
      }
      else {
        low = mid + 1;
      }
    }
    else {
      if (target > arr[mid] && target <= arr[high]) {
        low = mid + 1;
      }
      else {
        high = mid - 1;
      }
    }
  }
  return -1;
}

int main() {
  int a[7] = {9, 10, 15, 1, 2, 5, 7};
  vector<int> v(a, a + 7);
  int result = binarySearchInRotateArray(v, 2);
  cout << "result: " << result << endl;
  return 0;
}
```

---

# 小结

&emsp;&emsp;二分查找相信大家都非常熟悉，但是旋转数组的二分查找并不是这么普遍，有许多面试都会考到。希望这篇分享能够帮助到你。最后，欢迎大家转发、评论，谢谢！