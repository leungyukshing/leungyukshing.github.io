---
title: 软件测试作业6
abbrlink: 22868
date: 2019-05-01 17:17:37
tags:
  - SoftwareTest
mathjax: true
---

## 软件测试作业6

1. 分析Chap.5.1([Lec.17](/download/Lec17.pdf))自动售货机软件例子生成的判定表图例的第6列和第23列，分别给出：

   （1）输入条件的自然语义陈述；

   （2）输出结果的自然语义陈述；

   （3）用命题逻辑形式描述实现上述输入-输出过程所应用的判定规则，并写出获得输出结果的推理演算过程。

<!-- more -->

------

## Answer

### 自动售货机软件的测试用例

（1）分析需求说明，列出原因和结果清单

- 原因清单（输入条件）
  - C1售货机可找零
  - C2投入1元硬币
  - C3投入5角硬币
  - C4按下【橙汁】按钮
  - C5按下【啤酒】按钮
- 结果清单（输出结果）
  - E21【零钱找完】灯亮
  - E22 退还1元硬币
  - E23 退还5角硬币
  - E24 送出橙汁饮料
  - E25 送出啤酒饮料
- 建立中间节点，表示处理的中间状态
  - T11 投入1元硬币且按下饮料按钮
  - T12 按下【橙汁】或【啤酒】按钮
  - T13 应当找5角零钱并且售货机有零钱找
  - T14 钱已付清

（2）画出因果图

- 所有原因结点列在左边

- 所有结果结点列在右边

- 所有中间结点列在中间

- 所有因果关系表示为连接图解

- 加上必要的互斥约束条件E

  - C2与C3、C4与C5不能同时发生

  ![因果图](/images/因果图.png)

（3）因果图转换成判定表

- 按照因果图建立规则库，对输入条件C1-C5的全部解释计算输出结果，得到$2^5=32$列的判定表。

  ![判定表](/images/判定表.png)

- 判定表中可以删去的列：阴影部分表示违反约束条件的不可能出现的情况；第16列和第32列对应的输入条件C2、C3、C4、C5为0（黄色部分），表示操作者没有动作。

  ![判定表1](/images/判定表1.png)

- 余下的16列用绿色标识，作为确定测试用例的依据。

### 分析

1. 分析第6列。

   - **输入条件的自然语义陈述**：输入11010，表示C1售货机可找零、C2投入1元硬币、C4按下【橙汁】按钮

   - **输出结果的自然语义陈述**：输出00110，表示E23退还5角硬币、E24送出橙汁饮料

   - **命题逻辑形式描述实现上述输入-输出过程所应用的判定规则，并写出获得输出结果的推理演算过程**：中间结果1111，表示T11投入1元硬币且按下饮料按钮、T12按下【橙汁】或【啤酒】按钮、T13应当找5角零钱并且售货机有零钱找、T14钱已付清。推理过程如下：
     $$
     C4 \vee C5 \Rightarrow T12 \\
     C2 \wedge T12 \Rightarrow T11 \\
     C1 \wedge T11 \Rightarrow T13 \\
     T13 \Rightarrow E23 \\
     C3 \vee T13 \Rightarrow T14 \\
     C4 \wedge T14 \Rightarrow E24
     $$

2. 分析第23列。

   - **输入条件的自然语义陈述**：输入01001，表示C2投入1元硬币、C5按下【啤酒】按钮

   - **输出结果的自然语义陈述**：输出11000，表示E21【零钱找完】灯亮、E22退还1元硬币

   - **命题逻辑形式描述实现上述输入-输出过程所应用的判定规则，并写出获得输出结果的推理演算过程**：中间结果1100，表示T11投入1元硬币且按下饮料按钮、T12按下【橙汁】或【啤酒】按钮。推理过程如下：
     $$
     C4 \vee C5 \Rightarrow T12 \\
     C2 \wedge T12 \Rightarrow T11 \\
     \neg C1 \Rightarrow E21 \\
     \neg C1 \wedge T11 \Rightarrow E22
     $$

------

## Reference

1. <https://en.wikipedia.org/wiki/Decision_table>
2. [https://en.wikipedia.org/wiki/Cause%E2%80%93effect_graph](https://en.wikipedia.org/wiki/Cause–effect_graph)
3. [Lec.17 by Prof. Guoyang Cai](/download/Lec17.pdf)