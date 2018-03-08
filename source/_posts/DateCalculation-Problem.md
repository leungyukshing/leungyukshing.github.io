---
title: 日期计算
tags:
  - CCF
abbrlink: 45999
date: 2018-03-08 13:57:13
---
## 日期计算问题
### 问题描述
&emsp;给定一个年份y和一个整数d，问这一年的第d天是几月几日？
&emsp;注意闰年的2月有29天。满足下面条件之一的是闰年：
&emsp;1） 年份是4的整数倍，而且不是100的整数倍；
&emsp;2） 年份是400的整数倍。
<!-- more -->

### 输入格式
&emsp;输入的第一行包含一个整数y，表示年份，年份在1900到2015之间（包含1900和2015）。
&emsp;输入的第二行包含一个整数d，d在1至365之间。

### 输出格式
输出两行，每行一个整数，分别表示答案的月份和日期。

### 样例输入
> 2015
> 80

### 样例输出
> 3
> 21

### 样例输入
> 2000
> 40

### 样例输出
> 2
> 9

## 解决方法
### 分析
这个问题分为两部分，其一是判断闰年问题；第二个计算月份问题。判断闰年问题较简单，这里着重分析月份问题。
我们需要通过计算每个月之前共有多少天来判断已经过去的天数，同时，要注意边界情况，比如说1月1号，2月28号这些特殊的情况。
### 代码实现
```C++
#include <iostream>
using namespace std;

int main() {
  int year, day;
  cin >> year >> day;

  bool isLeapYear = false;
  if (year % 4 == 0  && year % 100 != 0) {
    isLeapYear = true;
  }
  if (year % 400 == 0) {
    isLeapYear = true;
  }

  if (isLeapYear) {
    int daysInMonth[] = {0, 31, 60, 91, 121, 152, 182, 213, 244, 274, 305, 335, 366};
    for (int i = 0; i <= 12; i++) {
      if (day == daysInMonth[i]) {
        cout << i << endl;
        cout << daysInMonth[i] - daysInMonth[i-1];
        break;
      }
      if (day >= daysInMonth[i] && day < daysInMonth[i+1]) {
        cout << i+1 << endl;
        cout << day - daysInMonth[i];
        break;
      }
    }
  }
  else {
    int daysInMonth[] = {0, 31, 59, 90, 120, 151, 181, 212, 243, 273, 304, 334, 365};
    for (int i = 0; i <= 12; i++) {
      if (day == daysInMonth[i]) {
        cout << i << endl;
        cout << daysInMonth[i] - daysInMonth[i-1];
        break;
      }
      if (day > daysInMonth[i] && day < daysInMonth[i+1]) {
        cout << i+1 << endl;
        cout << day - daysInMonth[i];
        break;
      }
    }
  }
  return 0;
}
```

这题的讲解到这里结束了，谢谢！
