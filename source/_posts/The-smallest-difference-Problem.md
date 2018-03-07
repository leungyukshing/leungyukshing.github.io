---
title: 最小差值问题
tags:
  - CCF
abbrlink: 64172
date: 2018-03-07 12:43:26
---
## 最小差值问题
### 问题描述
&emsp;给定*n*个数，请找出其中相差（差的绝对值）最小的两个数，输出它们的差值的绝对值。
### 输入格式
&emsp;输入第一行包含一个整数*n*。
第二行包含*n*个正整数，相邻整数之间使用一个空格分隔。
### 输出格式
输出一个整数，表示答案。
<!-- more -->

### 样例输入
> 5
> 1 5 4 8 20

### 样例说明
相差最小的两个数是5和4，它们之间的差值是1。
### 样例输入
> 5
> 9 3 6 1 3

### 样例输出
> 0

### 样例说明
有两个相同的数3，它们之间的差值是0。
### 数据规模和约定
对于所有评测用例，2≤ n ≤1000，每个给定的整数都是不超过10000的正整数。

## 解决方法
&emsp;主要思路：首先通过排序将数组中的数据有序化，由于最小的差值只会出现在有序数组的两个相邻的数中，因此，只需要求出有序数组中每两个相邻的数的差值即可，再从中找出最小的一个。此种算法也是效率最高的一种。

实现代码
```C++
#include <iostream>
#include <algorithm>
using namespace std;

int main() {
  int num;
  cin >> num;
  int arr[num];
  int i = 0;
  int n = num;
  while (n--) {
    cin >> arr[i++];
  }

  sort(arr, arr+num);

  int diff[num - 1];
  for (int j = 0; j < num-1; j++) {
    diff[j] = arr[j+1] - arr[j];
  }

  int min;
  if (num > 2) {
    min = arr[1] - arr[0];
    for (int k = 0; k < num-1; k++) {
      if (diff[k] < min)
        min = diff[k];
    }
  }
  else {
    min = diff[0];
  }

  cout << min << endl;
  return 0;
}
```

最小差值问题的讲解到这里结束了，谢谢！
