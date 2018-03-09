---
title: Json格式查询
tags:
  - CCF
abbrlink: 43575
date: 2018-03-09 22:43:22
---
## JSON查询问题
### 问题描述
&emsp;JSON (JavaScript Object Notation) 是一种轻量级的数据交换格式，可以用来描述半结构化的数据。JSON 格式中的基本单元是值 (value)，出于简化的目的本题只涉及 2 种类型的值：
  + 字符串 (string)：字符串是由双引号 " 括起来的一组字符（可以为空）。如果字符串的内容中出现双引号 "，在双引号前面加反斜杠，也就是用 \" 表示；如果出现反斜杠 \，则用两个反斜杠 \\ 表示。反斜杠后面不能出现 " 和 \ 以外的字符。例如：""、"hello"、"\"\\"。
  + 对象 (object)：对象是一组键值对的无序集合（可以为空）。键值对表示对象的属性，键是属性名，值是属性的内容。对象以左花括号 { 开始，右花括号 } 结束，键值对之间以逗号 , 分隔。一个键值对的键和值之间以冒号 : 分隔。键必须是字符串，同一个对象所有键值对的键必须两两都不相同；值可以是字符串，也可以是另一个对象。例如：{}、{"foo": "bar"}、{"Mon": "weekday", "Tue": "weekday", "Sun": "weekend"}。

&emsp;除了字符串内部的位置，其他位置都可以插入一个或多个空格使得 JSON 的呈现更加美观，也可以在一些地方换行，不会影响所表示的数据内容。例如，上面举例的最后一个 JSON 数据也可以写成如下形式。
&emsp;&emsp;{
&emsp;&emsp;"Mon": "weekday",
&emsp;&emsp;"Tue": "weekday",
&emsp;&emsp;"Sun": "weekend"
&emsp;&emsp;}
&emsp;给出一个 JSON 格式描述的数据，以及若干查询，编程返回这些查询的结果。
<!-- more -->

### 输入格式
&emsp;第一行是两个正整数 n 和 m，分别表示 JSON 数据的行数和查询的个数。
&emsp;接下来 n 行，描述一个 JSON 数据，保证输入是一个合法的 JSON 对象。
&emsp;&emsp;接下来 m 行，每行描述一个查询。给出要查询的属性名，要求返回对应属性的内容。需要支持多层查询，各层的属性名之间用小数点 . 连接。保证查询的格式都是合法的。

### 输出格式
&emsp;对于输入的每一个查询，按顺序输出查询结果，每个结果占一行。
  + 如果查询结果是一个字符串，则输出 STRING < string >，其中 < string > 是字符串的值，中间用一个空格分隔。
  + 如果查询结果是一个对象，则输出 OBJECT，不需要输出对象的内容。
  + 如果查询结果不存在，则输出 NOTEXIST。

### 样例输入
> 10 5
> {
> "firstName": "John",
> "lastName": "Smith",
> "address": {
> "streetAddress": "2ndStreet",
> "city": "NewYork",
> "state": "NY"
> },
> "esc\\aped": "\"hello\""
> }
> firstName
> address
> address.city
> address.postal
> esc\aped

### 样例输出
> STRING John
> OBJECT
> STRING NewYork
> NOTEXIST
> STRING "hello"

### 评测用例规模与约定
&emsp;n ≤ 100，每行不超过 80 个字符。
&emsp;m ≤ 100，每个查询的长度不超过 80 个字符。
&emsp;字符串中的字符均为 ASCII 码 33-126 的可打印字符，不会出现空格。所有字符串都不是空串。
&emsp;所有作为键的字符串不会包含小数点 .。查询时键的大小写敏感。
&emsp;50%的评测用例输入的对象只有 1 层结构，80%的评测用例输入的对象结构层数不超过 2 层。举例来说，{"a": "b"} 是一层结构的对象，{"a": {"b": "c"}} 是二层结构的对象，以此类推。

## 解决方法
### 分析
&emsp;本题的实质是解析Json格式的数据并储存。首先我们考虑使用map来储存，这是因为Json格式是基于键值对的形式的。
&emsp;然后我们就要考虑解析的内容，分为字符串和对象。其中字符串又是对象中的一部分。
&emsp;首先是字符串处理，主要是要对""和里面的转义字符进行特殊处理，其他并没有什么难度。
&emsp;对象的处理要稍微复杂一点，首先要使用一个**strType**变量来判断当前处理的是**key**还是**value**，处理后形成pair存入map中。注意，**key**要加入特殊的前缀用于访问对象中的属性。还要注意的是如果是一个二级结构，即对象中包含另一个对象，这个时候需要递归调用解析。

### 代码实现
```C++
#include <iostream>
#include <map>
#include <string>
#include <cassert>
using namespace std;

/*从str的i位置开始解析字符串*/
string parseString(string &str, int &i) {
  string temp;
  if (str[i] == '"') {
    i++;
  }
  else {
    assert(0);
  }

  while (i < str.size()) {
    /* 出现一个 \ , 跳过并记录下过一个符号*/
    if (str[i] == '\\') {
      i++;
      temp += str[i];
      i++;
    }
    // 结束
    else if (str[i] == '"') {
      break;
    }
    // 中间内容
    else {
      temp += str[i];
      i++;
    }
  }

  if (str[i] == '"') {
    i++;
  }
  else {
    assert(0);
  }

  return temp;
}

void paserObject(string &str, string prefix, map<string, string> &dict, int &i) {
  if (str[i] == '{') {
    i++;
  }
  else {
    assert(0);
  }

  string key, value;
  bool strType = false; // false: key, true: value
  while (i < str.size()) {
    if (str[i] == '"') {
      string temp = parseString(str, i);
      // temp是处理value,仅当处理完value后才保存进map中
      if(strType) {
        value = temp;
        dict[key] = value;
      }
      // temp是处理key
      /*Object 的 prefix 是"", Object中的String的prefix是含.*/
      else {
        key = prefix + (prefix == "" ? "" : ".") + temp;
      }
    }
    // 从key切换到value
    else if (str[i] == ':') {
      strType = true;
      i++;
    }
    // 从value切换到key
    else if (str[i] == ',') {
      strType = false;
      i++;
    }
    else if (str[i] == '{') {
      dict[key] = "";
      paserObject(str, key, dict, i);
    }
    else if (str[i] == '}') {
      break;
    }
    else {
      i++;
    }
  }

  if (str[i] == '}') {
    i++;
  }
  else {
    assert(0);
  }
}
int main() {
  int N, M;
  cin >> N >> M;
  string json;
  if (cin.peek() == '\n') {
    cin.ignore();
  }
  for (int n = 0; n < N; n++) {
    string tmp;
    getline(cin, tmp);
    json += tmp;
  }

  map<string, string> dict;
  int i = 0;
  paserObject(json, "", dict, i);
  string query;
  for (int m = 0; m < M; m++) {
    getline(cin, query);
    // Not found
    if (dict.find(query) == dict.end()) {
      cout << "NOTEXIST" << endl;
    }
    // Found
    else {
      // Object
      if (dict[query] == "") {
        cout << "OBJECT" << endl;
      }
      // String
      else {
        cout << "STRING " << dict[query] << endl;
      }
    }
  }
  return 0;
}
```
&emsp;这题虽然看起来很复杂，但是当静下心来搞懂Json格式的特点后，找到规律之后处理起来就简单多了。本题的讲解也到此结束，谢谢！
