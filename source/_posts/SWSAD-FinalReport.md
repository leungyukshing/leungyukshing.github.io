---
title: 系统分析与设计——项目个人总结
tags:
  - 系统分析与设计
abbrlink: 46663
date: 2019-06-19 00:55:12
---

## Brief Summary

&emsp;&emsp;我们小组开发的是[挣闲钱应用平台](<https://make-money-sysu.github.io/>)。项目开发的时间从3月份开始一直到现在，时间跨度很大，从一开始的设计到后面各个功能模块的逐步实现、前后端对接，我们小组都付了许多时间和精力。我们很好地完成了预期的功能，能够交付一个可用的服务平台。

<!-- more -->

&emsp;&emsp;我负责的部分主要是前端，集中在问卷功能的实现，在后期也参与到后端的部分开发与测试中。

&emsp;&emsp;问卷功能是平台两大功能的其中之一，属于系统的核心功能。一开始我们的设计就是自己写问卷的系统，这包含了非常多的内容：编辑问卷、发布问卷、填写问卷等。界面的设计、数据的存储都是经过了长时间的讨论才确定下来，并在后续的开发中不断调整。前期的开发是相对独立的，因此比较多也是我自己写一些测试数据对页面进行测试，这个过程大概有2个月，包括了UI的设计，逻辑的测试。到后面与后端服务器对接的时候，需要和后端开发的成员进行紧密地沟通。因为之前与他们有过许多项目开发的经历，也得益于他们优质的api文档与设计接口，这一过程遇到的问题虽然也不少，但是也能够很快地解决。因为问卷功能完成的比较早，因此我也参与了部分后端测试与开发，帮助测试了许多api，然后写好demo给其他前端的组员。最后整个应用很快就能够工作了。再到后面就是一些小修小改与压力测试等工作。

&emsp;&emsp;前端技术上我们统一使用了vuejs框架，问卷的所有页面均是自己设计，没有用其他的组件。后端使用的是Golang，自己使用了beego在本地进行了数据传输的测试部分。

---

## PSP2.1 Table

|                                         | Personal Software Process Stages         | 预估耗时（小时） | 实际耗时（小时） |
| --------------------------------------- | ---------------------------------------- | ---------------- | ---------------- |
| Planning                                | 计划                                     | 1                | 0.5              |
| - Estimate                              | - 估计这个任务需要多少时间               | 0.6              | 0.5              |
| Development                             | 开发                                     | 48               | 36               |
| - Analysis                              | - 需求分析（包括学习新技术）             | 0.5              | 0.5              |
| - Design Spec                           | - 生成设计文档                           | 0.5              | 1                |
| - Design Review                         | - 设计复审（和同事审核设计文档）         | 0.5              | 1                |
| - Coding Standard                       | - 代码规范（为目前的开发制定合适的规范） | 2                | 1                |
| - Design                                | - 具体设计                               | 1                | 0.8              |
| - Coding                                | - 具体编码                               | 72               | 80               |
| - Code Review                           | - 代码复审                               | 2                | 0.5              |
| - Test                                  | - 测试（自我测试、修改代码、提交修改）   | 48               | 30               |
| Reporting                               | 报告                                     | 1                | 2                |
| - Test Report                           | - 测试报告                               | 1                | 1.5              |
| - Size Measurement                      | - 计算工作量                             | 0.5              | 1                |
| - Postmortem & Process Improvement Plan | - 事后总结，并提出过程改进计划           | 1                | 3                |
|                                         | 合计                                     | 179.6            | 159.3            |

---

## Working List

+ **最有创意**：完全自己实现了问卷功能，包括数据设计和页面展示
+ **最有价值**：能够实现问卷问题的多元化，包括单选、多选、填空，支持数据库存储。同时也对后端的代码进行了少量的更正。
+ **最辛苦**：完成了四个页面的功能设计与UI设计，代码量比较大。也做了很多API测试，解决网络访问的问题。

---

## Contribution On Git

### Contribution on Frontend

![Frontend Contribution](/images/contribution_frontend.png)

### Contribution on Backend

![Backend Contribution](/images/contribution_backend.png)

### Contribution on Kanban

![Kanban Contribution](/images/contribution_kanban.png)

---

## Relevant Blog Links

1. [TAPD使用经历](<http://leungyukshing.cn/archives/TAPD.html>)
2. [Vue+Go网络访问CORS问题](http://leungyukshing.cn/archives/CORS.html)
3. [预检网络请求OPTION问题](http://leungyukshing.cn/archives/OPTION-Request.html)
4. [前端处理JSON的正确方法](http://leungyukshing.cn/archives/Frontend-JSON.html)

---

## Acknowledge

+ 前端开发人员：[梁颖霖](https://github.com/dick20)和[梁俊华](https://github.com/Alva112358)
+ 后端开发人员：[刘恒伟](https://github.com/52hz11)和[梁庭](https://github.com/ltstriker)
+ PM：[刘硕](https://github.com/LovelyBuggies)
+ 其他大佬：[Fluent](<https://github.com/x-h-h>)、[Carey](https://github.com/SYSUcarey)

&emsp;&emsp;感谢小组的所有成员在开发过程中的紧密合作，感谢所有在开发过程中给我提供技术支持的人。**It is we that make it!**