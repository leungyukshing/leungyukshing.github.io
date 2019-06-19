---
title: 系统分析与设计作业6
tags:
  - 系统分析与设计
abbrlink: 19437
date: 2019-06-14 13:54:48
---

## 领域建模 - 对象状态

### 课程网页

<https://sysu-swsad.github.io/swad-guide/09-domain-modeling>

<!-- more -->

### 问题

练习资源：[Asg-RH.pdf](/download/Asg_RH.pdf)

使用**UMLet**建模：

1. 使用类图，分别对 Asg_RH 文档中 Make Reservation 用例以及 Payment 用例开展领域建模。然后，根据上述模型，给出建议的数据表以及主要字段，特别是主键（key）和外键（Fkey）
   + 注意事项：
     + 对象必须是名词、特别是技术名词、报表、描述类的处理；
     + 关联必须有多重性、部分有名称与导航方向
     + 属性要注意计算字段
   + 数据建模，为了简化描述仅需要给出表清单，例如：
     + Hotel（ID/Key，Name，LoctionID/Fkey，Address…..）
2. 使用 UML State Model，对每个订单对象生命周期建模
   - 建模对象： 参考 Asg_RH 文档， 对 Reservation/Order 对象建模。
   - 建模要求： 参考练习不能提供足够信息帮助你对订单对象建模，请参考现在 定旅馆 的旅游网站，尽可能分析围绕订单发生的各种情况，直到订单通过销售事件（柜台销售）结束订单。

------

## Answer

1. MakeReservation用例领域建模

   ![MakeReservationDomainModel](/images/MakeReservationDomainModel.png)

   数据建模：

   + Hotel(HotelID/key, Name, LocationID/Fkey, address, star-rating, info)
   + Reservation(ID/key, HotelID/Fkey, CustomerID/Fkey, ShoppingBasketID, PaymentID/Fkey, RoomItemsID/Fkey, check-in-date, num-of-night, customer-full-name, customer-smoking, contack-email)
   + Customer(CustomerID/key)
   + ShoppingBasket(ShoppingBasketID/key)
   + Payment(PaymentID/key)
   + RoomItems(RoomItemsID/key, RoomID/Fkey, type, adults, children, age-range)
   + Room(RoomID/key, RoomDescriptionID/Fkey, type, date, price, available-num, reserved-num)
   + RoomDescription(RoomDescriptionID/key, RoomTypeID/Fkey, type, list-price, info)
   + RoomType(RoomTypeID/key)

2. Payment用例领域建模

   ![PaymentDomainModel](/images/PaymentDomainModel.png)

   数据建模：

   + Customer(CustomerID/key)
   + CreditCard(CreditCardID/key, Customer/Fkey, CardHolderID/Fkey, card-number, card-security-code, expiry-date)
   + CardHolder(CardHolderID/key, firstname, secondname, address1, address2, city, country/state, postcode, daytime-telephone, evening-telephone)
   + ShoppingBasket(ShoppingBasketID/key, CustomerID/Fkey)
   + PaymentItem(PaymentItemID/key, ShoppingBasketID/Fkey, BookingDetailsID/Fkey, price)
   + BookingDetails(BookingDetailsID/key, BookingDetailsDescriptionID/Fkey, type, check-in-date)
   + BookingDetailsDescription(BookingDetailsDescriptionID, details)

3. Reservation/Order状态建模

   ![ReservationStateModel](/images/ReservationStateModel.png)