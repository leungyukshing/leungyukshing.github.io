---
title: 火车购票问题
tags:
  - CCF
abbrlink: 51266
date: 2018-03-08 10:52:21
---
## 火车购票问题
### 问题描述
&emsp;请实现一个铁路购票系统的简单座位分配算法，来处理一节车厢的座位分配。
&emsp;假设一节车厢有20排、每一排5个座位。为方便起见，我们用1到100来给所有的座位编号，第一排是1到5号，第二排是6到10号，依次类推，第20排是96到100号。
&emsp;购票时，一个人可能购一张或多张票，最多不超过5张。如果这几张票可以安排在同一排编号相邻的座位，则应该安排在编号最小的相邻座位。否则应该安排在编号最小的几个空座位中（不考虑是否相邻）。
&emsp;假设初始时车票全部未被购买，现在给了一些购票指令，请你处理这些指令。
<!-- more -->

### 输入格式
&emsp;输入的第一行包含一个整数n，表示购票指令的数量。
&emsp;第二行包含n个整数，每个整数p在1到5之间，表示要购入的票数，相邻的两个数之间使用一个空格分隔。

### 输出格式
&emsp;输出n行，每行对应一条指令的处理结果。
&emsp;对于购票指令p，输出p张车票的编号，按从小到大排序。

### 样例输入
> 4
> 2 5 4 2

### 样例输出
> 1 2
> 6 7 8 9 10
> 11 12 13 14
> 3 4

### 样例说明
&emsp;1) 购2张票，得到座位1、2。
&emsp;2) 购5张票，得到座位6至10。
&emsp;3) 购4张票，得到座位11至14。
&emsp;4) 购2张票，得到座位3、4。

### 评测用例规模与约定
对于所有评测用例，1 ≤ n ≤ 100，所有购票数量之和不超过100。

## 解决方法
### 分析
建立20个队列用于表示每一行，车票卖出后就输出该位置的号码并pop出元素。分配座位的规则如下：
  + 如果能都安排在一行，则安排在同一行
  + 如果不能安排在同一行，则从编号小的开始安排座位。

### 代码实现
```C++
#include <iostream>
#include <queue>
using namespace std;

int main() {
  queue<int> q[21];
  int no = 1;
  for (int i = 1; i <= 20; i++) {
    q[i].push(no++);
    q[i].push(no++);
    q[i].push(no++);
    q[i].push(no++);
    q[i].push(no++);
  }

  int n;
  cin >> n;
  while (n--) {
    int num;
    cin >> num;
    bool finish = false;

    // find consecutive seats
    for (int row = 1; row <= 20; row++) {
      if (q[row].size() >= num) {
        for (int j = 0; j < num; j++) {
          cout << q[row].front() << " ";
          q[row].pop();
        }
        finish = true;
        break;
      }
    }
    // find inconsecutive seats
    if (!finish) {
      for (int row = 1; row <= 20 && num > 0; row++) {
        while (num > 0 && !q[row].empty()) {
          cout << q[row].front() << " ";
          q[row].pop();
          num--;
        }
      }
    }
    cout << endl;
  }

  return 0;
}
```
