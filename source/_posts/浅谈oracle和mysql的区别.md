---
title: 浅谈oracle和mysql的区别
abbrlink: 51090
date: 2020-01-12 00:00:11
---

## 浅谈oracle与mysql的区别

&emsp;&emsp;我们经常会接触到各种类型的数据库，其中有两大巨头Oracle和MySQL，这两者有什么区别呢？各自又有什么优缺点？在这篇博客，我将与大家简单分享一下。

<!-- more -->

## 对比

&emsp;&emsp;首先给大家一个感官上的对比，MySQL是中小型的数据库，而Oracle是大型数据库。以下是一些技术特性的对比：

+ 内存占用：Oracle占用空间大；MySQL占用空间少
+ 高并发访问：
  + Oracle支持大并发访问量，是OLTP（操作型数据库）最好的工具；Oracle使用**行级锁**，对资源锁定的粒度较小，不加锁匙加在数据库中的数据行上，不依赖索引，所以对并发性支持很好。
  + MySQL并发小。MySQL以**表级锁**为主，对资源锁定的粒度较大，如果一个session对一个表加了锁，那么其他session就无法访问到这个表。虽然InnoDB引擎的表支持行级锁，但是这个行级锁的机制依赖于表的索引，如果表没有索引，或者SQL语句没有索引，那么仍然使用的是表级锁。因此MySQL面对高并发场景需要做分表分库进行优化
+ 对事物的提交：Oracle默认不自动提交，需要用户手动提交commit；MySQL默认是自动提交commit
+ 分页查询：Oracle需要用到伪列ROWNUM和嵌套查询来做到分页；MySQL则可以直接通过limit实现分页
+ 事务隔离级别：
  + Oracle是**repeatable read**的隔离级别，查询时如果对应的数据块发生变化，会在undo表空间为这个session构造它查询时的旧数据块的快照；
  + MySQL是**read commited**级别，一个session读取数据时，其他session不能更改数据，但可以在表的最后插入数据。session更新数据时，会加上排它锁，所以其他session无法访问到该数据。
+ 事务的支持：Oracle完全支持事务，MySQL在InnoDB存储引擎的行级锁的情况下才支持事务
+ 数据备份：Oracle逻辑备份时不锁定数据，所以备份的数据是一致的；MySQL备份时要锁定数据，才能保证备份的数据是一致的。
+ 权限：
  + Oracle：表是属于用户的，用户属于数据库。所以一个数据库可以拥有多张同名的表，儿一个用户只能有一个名字的表
  + MySQL：表是属于数据库的，数据库是属于用户的。所以一个用户下可以拥有多张同名的表，一个数据库下只能有一个名字的表

## 总结

&emsp;&emsp;以上就是我对Oracle和MySQL的一些对比，如果有失误之处，欢迎大家指出与补充，感谢您的支持！