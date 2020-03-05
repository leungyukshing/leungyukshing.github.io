---
title: MD5算法设计及C++实现
abbrlink: 32383
date: 2019-01-20 14:14:06
tags:
---

## 算法原理概述

&emsp;&emsp;MD5加密算法（MD5，Message-Digest Alogorithm)，全称为MD5消息摘要算法，是一种被广泛使用的密码散列函数，应用于不定长的输入，可以产生出一个128位（16bytes）的散列值（hash value），用于确保信息传输完成一直。经过程序流程，生成四个32位（4bytes）数据，最后联合起来成为一个128bits的散列。基本方式为：填充、分块、缓冲区初始化、循环压缩，最后得出结果。

<!-- more -->

---

## 总体结构

![MD5总体结构](/images/MD5_structure.png)

---

## 模块分解

&emsp;&emsp;整个算法按照步骤以及编程习惯，分为如下几部分：

1. 构造函数：用于初始化变量以及寄存器区域（A，B，C，D）。注意这里寄存器使用小端存储模式。
2. 轮函数g（生成函数）：该函数是在4轮循环中使用的轮函数，每一个函数都是一个32位非线性逻辑函数，因为每一轮都有不同的定义，因此干脆把它们分成四个函数来写。里面的位运算使用C++提供的运算符就可以实现了。
3. MD5压缩函数transform：这个是对每一个64bytes分组进行压缩的函数，其中包括了对A进行迭代以及对缓冲区进行循环轮换。它实质上是调用轮函数g，并作相应的运算逻辑操作。注意在最后要把值加回到缓冲区中。
4. update连接函数：这个函数用于连接一部分数据（包括padding和消息尾部的64bits）到当前缓冲区中，压缩变换就是在这里面进行调用的。它需要更新当前消息位置的位数，然后不断地对每一个分组进行压缩。最后写到buffer中。
5. 其余辅助函数：包括有用于char到int类型转换的函数、算术循环左移函数和输出函数。

---

## 数据结构

全局定义用于迭代计算的T表和各次迭代运算采用的左循环移位的s值：

```c++
const unsigned int T[] = {0xd76aa478, 0xe8c7b756, 0x242070db, 0xc1bdceee,
                      0xf57c0faf, 0x4787c62a, 0xa8304613, 0xfd469501,
                      0x698098d8, 0x8b44f7af, 0xffff5bb1, 0x895cd7be,
                      0x6b901122, 0xfd987193, 0xa679438e, 0x49b40821,
                      0xf61e2562, 0xc040b340, 0x265e5a51, 0xe9b6c7aa,
                      0xd62f105d, 0x02441453, 0xd8a1e681, 0xe7d3fbc8,
                      0x21e1cde6, 0xc33707d6, 0xf4d50d87, 0x455a14ed,
                      0xa9e3e905, 0xfcefa3f8, 0x676f02d9, 0x8d2a4c8a,
                      0xfffa3942, 0x8771f681, 0x6d9d6122, 0xfde5380c,
                      0xa4beea44, 0x4bdecfa9, 0xf6bb4b60, 0xbebfbc70,
                      0x289b7ec6, 0xeaa127fa, 0xd4ef3085, 0x04881d05,
                      0xd9d4d039, 0xe6db99e5, 0x1fa27cf8, 0xc4ac5665,
                      0xf4292244, 0x432aff97, 0xab9423a7, 0xfc93a039,
                      0x655b59c3, 0x8f0ccc92, 0xffeff47d, 0x85845dd1,
                      0x6fa87e4f, 0xfe2ce6e0, 0xa3014314, 0x4e0811a1,
                      0xf7537e82, 0xbd3af235, 0x2ad7d2bb, 0xeb86d391};

const  unsigned int s[] = { 7, 12, 17, 22,  7, 12, 17, 22,  7, 12, 17, 22,  7, 12, 17, 22,
                       5,  9, 14, 20,  5,  9, 14, 20,  5,  9, 14, 20,  5,  9, 14, 20,
                       4, 11, 16, 23,  4, 11, 16, 23,  4, 11, 16, 23,  4, 11, 16, 23,
                       6, 10, 15, 21,  6, 10, 15, 21,  6, 10, 15, 21,  6, 10, 15, 21 };
```

类中定义用到的数据结构：

```c++
private:
  unsigned int count[2];
  unsigned int state[4]; // 寄存器缓冲区
  unsigned char buffer[1000];
  unsigned char digest_[16] = {0}; // 结果输出
```

---

## C++代码实现

`MD5.hpp`:

```c++
#ifndef MD5_H
#define MD5_H

#include <iostream>
#include <string>
#include <cstring>
using namespace std;


const unsigned int T[] = {0xd76aa478, 0xe8c7b756, 0x242070db, 0xc1bdceee,
                      0xf57c0faf, 0x4787c62a, 0xa8304613, 0xfd469501,
                      0x698098d8, 0x8b44f7af, 0xffff5bb1, 0x895cd7be,
                      0x6b901122, 0xfd987193, 0xa679438e, 0x49b40821,
                      0xf61e2562, 0xc040b340, 0x265e5a51, 0xe9b6c7aa,
                      0xd62f105d, 0x02441453, 0xd8a1e681, 0xe7d3fbc8,
                      0x21e1cde6, 0xc33707d6, 0xf4d50d87, 0x455a14ed,
                      0xa9e3e905, 0xfcefa3f8, 0x676f02d9, 0x8d2a4c8a,
                      0xfffa3942, 0x8771f681, 0x6d9d6122, 0xfde5380c,
                      0xa4beea44, 0x4bdecfa9, 0xf6bb4b60, 0xbebfbc70,
                      0x289b7ec6, 0xeaa127fa, 0xd4ef3085, 0x04881d05,
                      0xd9d4d039, 0xe6db99e5, 0x1fa27cf8, 0xc4ac5665,
                      0xf4292244, 0x432aff97, 0xab9423a7, 0xfc93a039,
                      0x655b59c3, 0x8f0ccc92, 0xffeff47d, 0x85845dd1,
                      0x6fa87e4f, 0xfe2ce6e0, 0xa3014314, 0x4e0811a1,
                      0xf7537e82, 0xbd3af235, 0x2ad7d2bb, 0xeb86d391};

const  unsigned int s[] = { 7, 12, 17, 22,  7, 12, 17, 22,  7, 12, 17, 22,  7, 12, 17, 22,
                       5,  9, 14, 20,  5,  9, 14, 20,  5,  9, 14, 20,  5,  9, 14, 20,
                       4, 11, 16, 23,  4, 11, 16, 23,  4, 11, 16, 23,  4, 11, 16, 23,
                       6, 10, 15, 21,  6, 10, 15, 21,  6, 10, 15, 21,  6, 10, 15, 21 };


class MD5 {
public:
  MD5(string str);
  void print(char *buff) const;

private:
  unsigned int count[2];
  unsigned int state[4]; // 寄存器缓冲区
  unsigned char buffer[1000];
  unsigned char digest_[16] = {0}; // 结果输出

  // 轮函数g
  // 轮次1
  unsigned int F(unsigned int b, unsigned int c, unsigned int d);
  // 轮次2
  unsigned int G(unsigned int b, unsigned int c, unsigned int d);
  // 轮次3
  unsigned int H(unsigned int b, unsigned int c, unsigned int d);
  // 轮次4
  unsigned int I(unsigned int b, unsigned int c, unsigned int d);

  // 每轮对A迭代
  // 轮次1
  void F_iter(unsigned int &a, unsigned int b, unsigned int c, unsigned int d, unsigned int xk, unsigned int ti, unsigned int s);
  // 轮次2
  void G_iter(unsigned int &a, unsigned int b, unsigned int c, unsigned int d, unsigned int xk, unsigned int ti, unsigned int s);
  // 轮次3
  void H_iter(unsigned int &a, unsigned int b, unsigned int c, unsigned int d, unsigned int xk, unsigned int ti, unsigned int s);
  // 轮次4
  void I_iter(unsigned int &a, unsigned int b, unsigned int c, unsigned int d, unsigned int xk, unsigned int ti, unsigned int s);

  // 缓冲区循环轮换
  unsigned int shiftLeft(unsigned int num, unsigned int shift);

  // 循环压缩
  void transform(const unsigned char block[64]);

  // 连接两块
  void update(const unsigned char* input, unsigned int inputLen);

  void update(const char* input, unsigned int inputLen);

  void encode(unsigned char *output, unsigned int* input, const unsigned int len);

  void decode (unsigned int* output, const unsigned char* input, unsigned int len);
  
  void final();
};

#endif
```

`MD5.cpp`:

```c++
#include "MD5.hpp"
MD5::MD5(string str) {
  // 初始化变量
  this->count[0] = 0; // 总长度bits
  this->count[1] = 0;
  // 初始化寄存器（小端模式）
  this->state[0] = 0x67452301;
  this->state[1] = 0xefcdab89;
  this->state[2] = 0x98badcfe;
  this->state[3] = 0x10325476;

  update(str.c_str(), str.size());
  final();
}

// 轮函数g
// 轮次1
unsigned int MD5::F(unsigned int b, unsigned int c, unsigned int d) {
  return (b & c) | (~b & d);
}
// 轮次2
unsigned int MD5::G(unsigned int b, unsigned int c, unsigned int d) {
  return (b & d) | (c & ~d);
}
// 轮次3
unsigned int MD5::H(unsigned int b, unsigned int c, unsigned int d) {
  return b ^ c ^ d;
}
// 轮次4
unsigned int MD5::I(unsigned int b, unsigned int c, unsigned int d) {
  return c ^ (b | ~d);
}

// 每轮对A迭代
// 轮次1
void MD5::F_iter(unsigned int &a, unsigned int b, unsigned int c, unsigned int d, unsigned int xk, unsigned int ti, unsigned int s) {
  unsigned int tmp = a + F(b, c, d) + xk + ti;
  a = b + shiftLeft(tmp, s);
}
// 轮次2
void MD5::G_iter(unsigned int &a, unsigned int b, unsigned int c, unsigned int d, unsigned int xk, unsigned int ti, unsigned int s) {
  unsigned int tmp = a + G(b, c, d) + xk + ti;
  a = b + shiftLeft(tmp, s);
}
// 轮次3
void MD5::H_iter(unsigned int &a, unsigned int b, unsigned int c, unsigned int d, unsigned int xk, unsigned int ti, unsigned int s) {
  unsigned int tmp = a + H(b, c, d) + xk + ti;
  a = b + shiftLeft(tmp, s);
}
// 轮次4
void MD5::I_iter(unsigned int &a, unsigned int b, unsigned int c, unsigned int d, unsigned int xk, unsigned int ti, unsigned int s) {
  unsigned int tmp = a + I(b, c, d) + xk + ti;
  a = b + shiftLeft(tmp, s);
}

// 缓冲区循环轮换
unsigned int MD5::shiftLeft(unsigned int num, unsigned int shift) {
  return (num << shift) | (num >> (32 - shift));
}

// 更换接口
void MD5::update(const char* input, unsigned int inputLen) {
  update(reinterpret_cast<const unsigned char*>(input), inputLen);
}

void MD5::update(const unsigned char* input, unsigned int inputLen) {
  unsigned int i = 0, index = 0, partlen = 0;
  

  index = (this->count[0] >> 3) & 0x3f;// 字节数

  this->count[0] += inputLen << 3; // 加上新的位数，更新总位数bits
  
  if (this->count[0] < (inputLen << 3)) {
    this->count[1]++; 
  }
  this->count[1] += inputLen >> 29;

  partlen = 64 - index;

  if (inputLen >= partlen) {
    // 复制数据到buffer中
    memcpy(&this->buffer[index], input, partlen);
    // 对一组进行压缩变换
    transform(this->buffer);
    
    for (i = partlen; i + 64 <= inputLen; i+=64) {
      transform(&input[i]);
    }
    index = 0;
  }
  else {
    i = 0;
  }
  memcpy(&this->buffer[index], &input[i], inputLen - i);
}

// 压缩迭代
void MD5::transform(const unsigned char block[64]) {
  unsigned int a = this->state[0];
  unsigned int b = this->state[1];
  unsigned int c = this->state[2];
  unsigned int d = this->state[3];
  unsigned int x[16];

  decode(x, block, 64);

  F_iter(a, b, c, d, x[0], T[0], s[0]);
  F_iter(d, a, b, c, x[1], T[1], s[1]);
  F_iter(c, d, a, b, x[2], T[2], s[2]);
  F_iter(b, c, d, a, x[3], T[3], s[3]);
  F_iter(a, b, c, d, x[4], T[4], s[4]);
  F_iter(d, a, b, c, x[5], T[5], s[5]);
  F_iter(c, d, a, b, x[6], T[6], s[6]);
  F_iter(b, c, d, a, x[7], T[7], s[7]);
  F_iter(a, b, c, d, x[8], T[8], s[8]);
  F_iter(d, a, b, c, x[9], T[9], s[9]);
  F_iter(c, d, a, b, x[10], T[10], s[10]);
  F_iter(b, c, d, a, x[11], T[11], s[11]);
  F_iter(a, b, c, d, x[12], T[12], s[12]);
  F_iter(d, a, b, c, x[13], T[13], s[13]);
  F_iter(c, d, a, b, x[14], T[14], s[14]);
  F_iter(b, c, d, a, x[15], T[15], s[15]);

  G_iter(a, b, c, d, x[1], T[16], s[16]);
  G_iter(d, a, b, c, x[6], T[17], s[17]);
  G_iter(c, d, a, b, x[11], T[18], s[18]);
  G_iter(b, c, d, a, x[0], T[19], s[19]);
  G_iter(a, b, c, d, x[5], T[20], s[20]);
  G_iter(d, a, b, c, x[10], T[21], s[21]);
  G_iter(c, d, a, b, x[15], T[22], s[22]);
  G_iter(b, c, d, a, x[4], T[23], s[23]);
  G_iter(a, b, c, d, x[9], T[24], s[24]);
  G_iter(d, a, b, c, x[14], T[25], s[25]);
  G_iter(c, d, a, b, x[3], T[26], s[26]);
  G_iter(b, c, d, a, x[8], T[27], s[27]);
  G_iter(a, b, c, d, x[13], T[28], s[28]);
  G_iter(d, a, b, c, x[2], T[29], s[29]);
  G_iter(c, d, a, b, x[7], T[30], s[30]);
  G_iter(b, c, d, a, x[12], T[31], s[31]);

  H_iter(a, b, c, d, x[5], T[32], s[32]);
  H_iter(d, a, b, c, x[8], T[33], s[33]);
  H_iter(c, d, a, b, x[11], T[34], s[34]);
  H_iter(b, c, d, a, x[14], T[35], s[35]);
  H_iter(a, b, c, d, x[1], T[36], s[36]);
  H_iter(d, a, b, c, x[4], T[37], s[37]);
  H_iter(c, d, a, b, x[7], T[38], s[38]);
  H_iter(b, c, d, a, x[10], T[39], s[39]);
  H_iter(a, b, c, d, x[13], T[40], s[40]);
  H_iter(d, a, b, c, x[0], T[41], s[41]);
  H_iter(c, d, a, b, x[3], T[42], s[42]);
  H_iter(b, c, d, a, x[6], T[43], s[43]);
  H_iter(a, b, c, d, x[9], T[44], s[44]);
  H_iter(d, a, b, c, x[12], T[45], s[45]);
  H_iter(c, d, a, b, x[15], T[46], s[46]);
  H_iter(b, c, d, a, x[2], T[47], s[47]);

  I_iter(a, b, c, d, x[0], T[48], s[48]);
  I_iter(d, a, b, c, x[7], T[49], s[49]);
  I_iter(c, d, a, b, x[14], T[50], s[50]);
  I_iter(b, c, d, a, x[5], T[51], s[51]);
  I_iter(a, b, c, d, x[12], T[52], s[52]);
  I_iter(d, a, b, c, x[3], T[53], s[53]);
  I_iter(c, d, a, b, x[10], T[54], s[54]);
  I_iter(b, c, d, a, x[1], T[55], s[55]);
  I_iter(a, b, c, d, x[8], T[56], s[56]);
  I_iter(d, a, b, c, x[15], T[57], s[57]);
  I_iter(c, d, a, b, x[6], T[58], s[58]);
  I_iter(b, c, d, a, x[13], T[59], s[59]);
  I_iter(a, b, c, d, x[4], T[60], s[60]);
  I_iter(d, a, b, c, x[11], T[61], s[61]);
  I_iter(c, d, a, b, x[2], T[62], s[62]);
  I_iter(b, c, d, a, x[9], T[63], s[63]);

  this->state[0] += a;
  this->state[1] += b;
  this->state[2] += c;
  this->state[3] += d;

  memset(x, 0, sizeof(x));
}

// int to char
void MD5::encode(unsigned char *output, unsigned int* input, const unsigned int len) {
  for (int i = 0, j = 0; j < len; i++, j+=4) {
    output[j] = input[i] & 0xff;
    output[j+1] = (input[i] >> 8) & 0xff;
    output[j+2] = (input[i] >> 16) & 0xff;
    output[j+3] = (input[i] >> 24) & 0xff;
  }
  cout << endl;
}

// char to int
void MD5::decode (unsigned int* output, const unsigned char* input, unsigned int len) {
  for (int i = 0, j = 0; j < len; i++, j+=4) {
    output[i] = input[j] | (input[j+1] << 8) | (input[j+2] << 16) | (input[j+3] << 24);
  }
  
}

// 总操作函数
void MD5::final() {
  static const unsigned char padding[64] = {0x80};
  
  unsigned int index = 0, padlen = 0;
  unsigned char bits[8];

  // 数据长度，用于最后填充
  encode(bits, this->count, 8);

  index = (this->count[0] >> 3) & 0x3f; // 除8得字节数
  
  // 以448为界，小于448为一个，多于448分开两个
  padlen = (index < 56) ? (56 - index) : (120 - index);

  // 添加padding，padding长度由padlen决定
  update(padding, padlen);

  // 将数据长度填充进最后64位（8bytes）
  update(bits, 8);

  // 将缓冲区四个数据连在一起，共（16bytes）
  encode(digest_, this->state, 16);

  memset(this->buffer, 0, sizeof(this->buffer));
  memset(this->count, 0, sizeof(this->count));
}

// 输出函数
void MD5::print(char *buff) const {
  // 16*8=128最终输出
  for (int i = 0; i < 16; i++) {
    // 补齐两位格式化输出
    sprintf(buff + i * 2, "%02x", digest_[i]);
  }
  buff[32] = '\0';
}
```

`main.cpp`:

```c++
#include "MD5.hpp"

int main() {
  char ch[1000];
  cout << "Welcome to Use MD5!" << endl;
  cout << "Please Input text: ";
  cin.getline(ch, 1000);
  string str(ch);
  MD5 m(str);
  char bu[33];
  m.print(bu);
  cout << bu << endl;
  return 0;
}
```

---

## 运行及测试

使用Wikipedia提供的三个例子作为测试样例。

1. MD5(“The quick brown fox jumps over the lazy dog”) = 9e107d9d372bb6826bd81d3542a419d6

   运行结果：

   ![MD5测试1](/images/MD5_test1.png)

2. MD5(“The
   quick brown fox jumps over the lazy cog”) = 1055d3e698d289f2af8663725127bd4b

   运行结果：

   ![MD5测试2](/images/MD5_test2.png)

3. MD5(“”) = d41d8cd98f00b204e9800998ecf8427e

   运行结果：

   ![MD5测试3](/images/MD5_test3.png)

---

## 总结

&emsp;&emsp;MD5算法是属于非对称密钥体系的，它加密后的密文是不可逆的，也就是说我们几乎无法从密文推出原文。同时，即使明文只改变一个字符，或者增加一个空格，经过MD5加密后的密文也会有很大的不同，这也说明了MD5加密算法是比较可靠的。MD5算法的有关分享就到这里，谢谢您的支持！