---
title: MySQL的一些细节
date: 2020-02-28 11:22:25
---

# MySQL细节讨论

&emsp;&emsp;MySQL是一个普遍使用的数据库管理系统，虽然入门很简单，但是其中还有许多细节值得探讨。这些细节在软件工程实践中非常重要，有可能对性能和稳定性有着重要的影响。在这篇博客，我将讨论一些MySQL的细节，长期更新，欢迎订阅。

<!-- more -->

## MySQL删除数据后磁盘空间不释放

**问题**：在删除大量数据后，会发现MySQL占用的磁盘空间并没有明显的减少，这背后发生了什么？

我们分几种情况讨论：

1. `drop table table_name`，删除表的操作会立即释放磁盘空间。InnoDB和MyISAM表现一样。
2. `truncate table table_name`，会立即释放空间。truncate操作可以理解为drop + create。但是create过程做了优化，因为表的结果是已知的，所以truncate的速度接近于drop。
3. `delete from table_name`，MyISAM会立即释放空间，InnoDB不会立即释放空间。
4. `delete from table_name where xxx`，带条件的删除，不会立即释放空间。InnoDB和MyISAM表现一样。
5. delete后`optimize table table_name`，会释放磁盘空间。
6. `delete from`之后虽然不会直接释放空间，但是之后插入数据的时候，仍然可以使用这部分空间。也就是说，这一部分空间实际上是可用的，但是不会释放，可以重复利用。所以对于写频繁的表格，应该定期optimize，释放掉占用的空间。