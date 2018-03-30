---
title: CCF--钥匙盒问题
tags:
  - CCF
abbrlink: 39430
date: 2018-03-07 21:52:44
---
## 钥匙盒问题
### 问题描述
&emsp;有一个学校的老师共用N个教室，按照规定，所有的钥匙都必须放在公共钥匙盒里，老师不能带钥匙回家。每次老师上课前，都从公共钥匙盒里找到自己上课的教室的钥匙去开门，上完课后，再将钥匙放回到钥匙盒中。
&emsp;钥匙盒一共有N个挂钩，从左到右排成一排，用来挂N个教室的钥匙。一串钥匙没有固定的悬挂位置，但钥匙上有标识，所以老师们不会弄混钥匙。
&emsp;每次取钥匙的时候，老师们都会找到自己所需要的钥匙将其取走，而不会移动其他钥匙。每次还钥匙的时候，还钥匙的老师会找到最左边的空的挂钩，将钥匙挂在这个挂钩上。如果有多位老师还钥匙，则他们按钥匙编号从小到大的顺序还。如果同一时刻既有老师还钥匙又有老师取钥匙，则老师们会先将钥匙全还回去再取出。
&emsp;今天开始的时候钥匙是按编号从小到大的顺序放在钥匙盒里的。有K位老师要上课，给出每位老师所需要的钥匙、开始上课的时间和上课的时长，假设下课时间就是还钥匙时间，请问最终钥匙盒里面钥匙的顺序是怎样的？
<!-- more -->

### 输入格式
&emsp;输入的第一行包含两个整数N, K。
&emsp;接下来K行，每行三个整数w, s, c，分别表示一位老师要使用的钥匙编号、开始上课的时间和上课的时长。可能有多位老师使用同一把钥匙，但是老师使用钥匙的时间不会重叠。
&emsp;保证输入数据满足输入格式，你不用检查数据合法性。

### 输出格式
输出一行，包含N个整数，相邻整数间用一个空格分隔，依次表示每个挂钩上挂的钥匙编号。

### 样例输入
> 5 2
> 4 3 3
> 2 2 7

### 样例输出
> 1 4 3 2 5

### 样例说明
&emsp;第一位老师从时刻3开始使用4号教室的钥匙，使用3单位时间，所以在时刻6还钥匙。第二位老师从时刻2开始使用钥匙，使用7单位时间，所以在时刻9还钥匙。
&emsp;每个关键时刻后的钥匙状态如下（X表示空）：
&emsp;&emsp;时刻2后为1X345；
&emsp;&emsp;时刻3后为1X3X5；
&emsp;&emsp;时刻6后为143X5；
&emsp;&emsp;时刻9后为14325。

### 样例输入
> 5 7
> 1 1 14
> 3 3 12
> 1 15 12
> 2 7 20
> 3 18 12
> 4 21 19
> 5 30 9

### 样例输出
> 1 2 3 5 4

### 测评用例规模与约定
对于30%的评测用例，1 ≤ N, K ≤ 10, 1 ≤ w ≤ N, 1 ≤ s, c ≤ 30；
对于60%的评测用例，1 ≤ N, K ≤ 50，1 ≤ w ≤ N，1 ≤ s ≤ 300，1 ≤ c ≤ 50；
对于所有评测用例，1 ≤ N, K ≤ 1000，1 ≤ w ≤ N，1 ≤ s ≤ 10000，1 ≤ c ≤ 100。

## 解决方法
### 分析
&emsp;这是一个对特定东西拿和取的问题，对于钥匙最后的顺序，是由前面一系列操作决定的。因此我们要充分利用OOP，把对钥匙的操作分离出来。封装Action这个类，关键是定义好operator<，这个是用于后续对各种操作的优先级排序的。
从题目分析可以知道，对于两个操作，其优先级定义为：
  + 时间先者为先；
  + 时间相同时，放回优先；
  + 时间相同，动作相同时，号码较小的优先；

这样，我们就定义了这个Action排序时候的优先级。通过对数据的读入，建立所有的Action并进行排序。随后从优先级较高的Action开始处理：
  + 若是放回操作，则从左到右找到空位就放回；
  + 若是取出操作，则找到对应的钥匙号码就取出；

### 代码实现
```C++
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;
class Action {
public:
  int room;
  int time;
  int operation;
  Action(int _room, int _time, int _operation) {
    room = _room;
    time = _time;
    operation = _operation;
  }

  bool operator<(const Action &other) const {
    if (time < other.time)
      return true;
    else if (time == other.time && operation < other.operation)
      return true;
    else if (time == other.time && operation == other.operation && room < other.room)
      return true;

    return false;
  }
};

int main() {
  int N, K;
  cin >> N >> K;

  std::vector<Action> actions;
  std::vector<int> keys(N+1);

  for (int i = 1; i <= N; i++) {
    keys[i] = i;
  }

  while (K--) {
    int room, begin, length;
    cin >> room >> begin >> length;
    actions.push_back(Action(room, begin, 1));
    actions.push_back(Action(room, begin+length, 0));
  }

  sort(actions.begin(), actions.end());

  for (int i = 0; i < actions.size(); i++) {
    Action act = actions[i];
    if (act.operation == 0) {
      /*靠左放置钥匙*/
      for (int n = 1; n <= N; n++) {
        if (keys[n] == -1) {
          keys[n] = act.room;
          break;
        }
      }
    }
    else {
      /*找到指定钥匙拿走*/
      for (int n = 1; n <= N; n++) {
        if (keys[n] == act.room) {
          keys[n] = -1;
          break;
        }
      }
    }
  }

  for (int n = 1; n < N; n++) {
    cout << keys[n] << " ";
  }
  cout << keys[N] << endl;

  return 0;
}
```
至此，本题的解析已经讲述完，谢谢！
