---
title: 系统分析与设计作业5
tags:
  - 系统分析与设计
abbrlink: 19117
date: 2019-05-19 22:32:51
---

## 用例建模-业务建模

### 课程网页

<https://sysu-swsad.github.io/swad-guide/07-usecase-modeling>

<!-- more -->

### 问题

使用**UMLet**建模：

1. 根据订旅馆建模文档，[Asg-RH.pdf](/download/Asg_RH.pdf):
   + 绘制用例图模型（到子用例）
   + 给出make reservation用例的活动图
2. 根据课程练习“投递员使用投递箱给收件人快递包裹”的业务场景
   + 分别用多泳道图建模三个场景的业务过程
   + 根据上述流程，给出快递柜系统最终的用例图模型
     + 用正常色彩表示第一个业务流程反映的用例
     + 用绿色背景表述第二个业务场景添加或修改的用例，以及支持Actor
     + 用黄色背景表述第三个业务场景添加或修改的用例，以及支持Actor

---

## Answer

1. 根据订旅馆建模文档，绘制如下图：

   + 用例图模型

     ![ReserveHotel](/images/ReserveHotel.png)

   + make reservation用例的活动图

     ![HotelProgress](/images/hotel_progress.png)

2. 例子：投递员使用投递箱给收件人快递包裹业务

   + 多泳道图

     + 场景1：x科技公司发明了快递柜，它们自建了投递柜以及远程控制系统。注册的投递员在推广期免费使用投递柜。由于缺乏资源，仅能使用y移动平台向客户发送短信通知。

       ![swimlane1](/images/swimlanes1.png)

     + 场景2：随着产品推广，x公司与各大快递z公司达成协议。x公司在快递柜上添加了二维码扫描装置，z公司的快递员不仅可以在快递柜上登录（由z公司提供认证服务），且可以扫描快递单号，投递入柜后自动由z公司发短信给客户。客户取件后，自动发送给z公司投递完成。

       ![swimlane2](/images/swimlanes2.png)

     + 场景3：x公司进一步优化服务，开发了微信小程序实现扫码取快递。如果用户关注了该公司公众号，直接通过公众号推送给用户取件码等信息，不再发送短信。

       ![swimlane3](/images/swimlanes3.png)

   + 快递柜系统最终的用例图模型

     ![DeliveryLockers](/images/DeliveryLockers.png)

## Reference

1. <https://en.wikipedia.org/wiki/Swim_lane>