---
layout: article
title: 概览
---

Go库`database/sql`通过 `sql.DB`来访问数据库。你可以利用`sql.DB`提供的方法来创建语句(statement)和事务(transaction)，执行查询以及获取结果。

首先你必须理解： **`sql.DB`不是数据库连接**，同时`sql.DB`也没有局限于任何特定的数据库或模式(schema)概念。`sql.DB`本质是数据库的一个抽象的接口。这里的数据库可以指一个本地文件，可以是通过网络连接，也可指存在于内存中的数据库。

`sql.DB`在底层做了很多重要的工作：

* 通过驱动(driver)打开或者关闭实际的底层数据库的连接
* 按需管理数据库连接池，比如实现之前提到的`sql.DB`的各种方法。

`sql.DB`所作的抽象避免你迷失在如何管理并发访问底层数据库之类的问题中。当你使用某个数据库连接时，`sql.DB`把这个数据库标注为“使用中”，当你不再使用这个连接时，又把这个连接释放回连接池中。不过与此同时`sql.DB`这个特性也带来一个问题：**当你使用完连接后忘记及时释放连接时，可能会导致`db.SQL`打开了很多数据库连接**，这很可能造成资源耗尽（太多连接意味着太多打开的文件，意味着太多网络端口被占用等等）。我们之后会详细分析这种情况。

在创建一个`sql.DB`变量后，你就可以通过它查询它背后的数据库，也可以用其创建语句或者事务。

**Next: [导入数据库驱动](zh-CN/importing.html)**