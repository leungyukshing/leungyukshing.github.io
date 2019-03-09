---
title: 系统分析与设计作业1
tags:
  - 系统分析与设计
abbrlink: 35244
date: 2019-03-09 21:03:48
---

## 软件的本质与软件工程科学

课程网页：https://sysu-swsad.github.io/swad-guide/01-nature-software

<!-- more -->

---

## 简答题

1. 软件工程的定义

   答：Software engineering is (1)the application of a systematic, disciplined, quantifiable approach to the development, operation, and maintenance of software, that is, the application of engineering to software, and (2)the study of approaches as in (1)

2. 解释导致software crisis本质原因、表现，述说克服软件危机的方法.

   答：Software crisis is a term used in the early days of computing science for the difficulty of writing useful and efficient computer programs in the required time. It was due to the **rapid increases in computer power and the complexity of the problems that could not be tackled**.

   The essential reason is that the machines have become several orders of magnitude more powerful.(软件复杂性越来越高)

   Manifestation:

   + Projects running over-budget(项目超预算完成)
   + Projects running over-time(项目超时完成)
   + Software was very inefficient(软件效率低下)
   + Software was of low quality(软件质量低下)
   + Software often did not meet requirements(软件无法满足用户要求)
   + Projects were unmanageable and code difficult to maintain(软件不可维护)
   + Software was never delivered(软件无法交付使用)

   解决方法：使用软件工程的方法

3. 软件生命周期

   答：A software develpment process is the process of dividing software development work into distinct phases to improve design, product, management, and project management.

   Methodologies:

   + Agile development(敏捷模型)
   + Waterfall development(瀑布模型)
   + Spiral development(螺旋模型)

4. SWeBok的15个知识域（An Overview of the SWEBOK Guide 请中文翻译其名称与简短说明）

   答：

   + Knowledge Areas Characterizing the Pratice of Software Engineering(软件工程实践知识域，共11个)
     + Sofrware Requirement(软件需求)：关注软件需求的启发、协商、分析、规范和验证。软件需求表达了对软件产品的需求和限制，这些需求和约束有助于解决一些现实问题。
     + Software Design(软件设计)：定义系统或组建的体系结构、组件、接口和其他特征的过程以及该过程的结果。软件设计过程是软件工程生命周期活动，其中分析软件需求以产生软件内部结构及其行为的描述，其将作为其构造的基础。软件设计必须描述软件体系结构——即软件如何分解和组成组件以及他们之间的接口。它必须描述组建细节以致于能够构建。
     + Software Construction(软件构造)：通过结合详细设计，编码，单元测试，集成测试，调试和验证来详细创建工作软件。
     + Software Testing(软件测试)：评估产品质量并通过识别缺陷来改进产品质量。
     + Software Maintenance(软件维护)：增强现有功能，调整软件以在新的和修改的操作环境中运行，以及纠正缺陷。
     + Software Configuration Management(软件配置管理)：硬件，固件，软件及其组合的功能和物理特征。
     + Software Engineering Management(软件工程管理)：规划，协调，测量，报告和控制项目或程序，以确保软件的开发和维护是系统化的，规范化的和量化的。
     + Software Engineering Management(软件工程管理)
     + Software Engineering Process(软件工程过程)：关注软件生命周期过程的定义，实施，评估，测量，管理和改进。
     + Software Engineering Models and Methods(软件工程模型和方法)：建模、 模型类型、分析、和软件开发方法。
     + Software Quality(软件质量)：解决普遍存在的软件生命周期问题。
     + Software Engineering Professional Practice(软件工程职业实践)：关注软件工程师必须具备的专业，负责和符合伦理的软件工程知识，技能和态度。
   + Knowledge Areas Characterizing the Educational Requirements of Software Engineering(软件工程教育知识域，共4个)
     + SoftWare Engineering Economics(软件工程经济学)：关注在业务环境中做出决策，以使技术决策与组织的业务目标保持一致。涵盖的主题包括软件工程经济学的基本原理（提案，现金流量，货币时间价值，计划视野，通货膨胀，折旧，替代和退休决策）; 非营利性决策（成本效益分析，优化分析）; 估计，经济风险和不确定性（估算技术，风险决策和不确定性）; 和多属性决策（价值和衡量尺度，补偿和非补偿技术）。
     + Computing Foundations(计算基础)：提供软件工程实践所需的计算背景。涵盖的主题包括问题解决技术，抽象，算法和复杂性，编程基础，并行和分布式计算的基础知识，计算机组织，操作系统和网络通信。
     + Mathematical Foundations(数学基础)：提供软件工程实践所必需的数学背景。涵盖的主题包括集合，关系和功能; 基本命题和谓词逻辑; 证明技术; 图形和树木; 离散概率; 语法和有限状态机; 和数论。
     + Engineering Foundations(工程基础)：盖了提供软件工程实践所必需的工程背景。涵盖的主题包括经验方法和实验技术; 统计分析; 测量和指标; 工程设计; 仿真与建模; 和根本原因分析。

5. 简单解释CMMI的五个级别。例如：Level 1 - Initial：无序，自发生成模式。

   答：CMMI的五个级别：

   + Level 1 - Initial(初始级)：软件过程是无序的，有时甚至是混乱的，对过程几乎没有定义，成功取决于个人努力。管理是反应式的。
   + Level 2 - Managed(已管理级)：建立了基本的项目管理过程来跟踪费用、进度和功能特性。制定了必要的过程纪律，能重复早先类似应用项目取得的成功经验。
   + Level 3 - Defined(已定义级)：已将软件管理和工程两方面的过程文档化、标准化，并综合成该组织的标准软件过程。所有项目均使用经批准、剪裁的标准软件过程来开发和维护软件，软件产品的生产在整个软件过程是可见的。
   + Level 4 - Quatitaviely Managed(量化管理级)：分析对软件过程和产品质量的详细度量数据，对软件过程和产品都有定量的理解与控制。管理有一个作出结论的客观依据，管理能够在定量的范围内预测性能。
   + Level 5 - Optimizing(优化管理级)：过程的量化反馈和先进的新思想、新技术促使过程持续不断改进。

6. 用自己语言简述SWEBok或CMMI（约200字）

   答：CMMI，能力成熟度模型集成，其目的是帮助软件企业对软件工程过程进行管理和改进，增强开发与改进能力，从而能够按时地、不超预算地开发出高质量的软件。其基本思想是：只要集中精力持续努力去建立有效的软件工程过程的基础结构，不断地进行管理的实践和过程的改进，就可以克服软件开发中的困难。它重点关注四个方面：成本效益、明确重点、过程集中和灵活性。这样一个框架的优点在于，它为改进一个组织的各种过程提供了一个单一的集成化框架，这个新的继承模型框架消除了各个模型的不一致性，减少了模型间的重复，增加了透明度和理解，建立了一个自动的、可拓展的框架，因而能够从总体上改进组织的质量和效率。

## Reference

1. https://en.wikipedia.org/wiki/Software_development_process
2. https://www.sebokwiki.org/wiki/An_Overview_of_the_SWEBOK_Guide
3. https://en.wikipedia.org/wiki/Capability_Maturity_Model_Integration

