---
title: ISBN号码
tags:
  - CCF
abbrlink: 60541
date: 2018-03-15 11:56:28
---
## ISBN号码问题
### 问题描述
&emsp;每一本正式出版的图书都有一个ISBN号码与之对应，ISBN码包括9位数字、1位识别码和3位分隔符，其规定格式如“x-xxx-xxxxx-x”，其中符号“-”是分隔符（键盘上的减号），最后一位是识别码，例如0-670-82162-4就是一个标准的ISBN码。ISBN码的首位数字表示书籍的出版语言，例如0代表英语；第一个分隔符“-”之后的三位数字代表出版社，例如670代表维京出版社；第二个分隔之后的五位数字代表该书在出版社的编号；最后一位为识别码。
&emsp;识别码的计算方法如下：
&emsp;首位数字乘以1加上次位数字乘以2……以此类推，用所得的结果mod 11，所得的余数即为识别码，如果余数为10，则识别码为大写字母X。例如ISBN号码0-670-82162-4中的识别码4是这样得到的：对067082162这9个数字，从左至右，分别乘以1，2，…，9，再求和，即0×1+6×2+……+2×9=158，然后取158 mod 11的结果4作为识别码。
&emsp;编写程序判断输入的ISBN号码中识别码是否正确，如果正确，则仅输出“Right”；如果错误，则输出是正确的ISBN号码。
<!-- more -->

### 输入格式
&emsp;输入只有一行，是一个字符序列，表示一本书的ISBN号码（保证输入符合ISBN号码的格式要求）。

### 输出格式
&emsp;输出一行，假如输入的ISBN号码的识别码正确，那么输出“Right”，否则，按照规定的格式，输出正确的ISBN号码（包括分隔符“-”）。

### 样例输入1
> 0-670-82162-4

### 样例输出1
> Right

### 样例输入2
> 0-670-82162-0

### 样例输出2
> 0-670-82162-4

## 解决方法
### 分析
&emsp;首先我们将除**识别码**外的所有数字数字提取出来，放入一个**vector**中，然后通过一个遍历求出**识别码**，再和题目的输入进行对比，输出答案即可。注意int和char类型之间的转换即可。

### 代码实现
```C++
#include <iostream>
#include <string>
#include <vector>
using namespace std;

bool isNumber(char ch) {
  if (ch >= '0' && ch <= '9')
    return true;

  return false;
}

int main() {
  string str;
  cin >> str;

  std::vector<char> v;
  for (int i = 0; i < str.size() - 1; i++) {
    if (isNumber(str[i])) {
      v.push_back(str[i]);
    }
  }

  int sum = 0;
  for (int i = 0; i < v.size(); i++) {
    sum += (v[i] - '0') * (i + 1);
  }
  int code = sum % 11;
  if (code == 10) {
    if (str[str.size() - 1] == 'X') {
      cout << "Right" << endl;
    }
    else {
      str[str.size() - 1] = 'X';
      cout << str << endl;
    }
  }
  else {
    if (str[str.size() - 1] == code + '0') {
      cout << "Right" << endl;
    }
    else {
      str[str.size() - 1] = code + '0';
      cout << str << endl;
    }
  }
  return 0;
}
```
本题的讲解到这里结束了， 谢谢！
