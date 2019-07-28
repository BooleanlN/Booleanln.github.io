---
title: mysql_6_索引机制
date: 2019-04-07 21:35:28
tags:
 - mysql
---

### Mysql_索引机制

#### 索引概述

所有MySQL列类型都可以被索引，使用索引可以提高select操作性能。每种存储引擎对每个表至少支持16个索引，总索引长度至少为256字节，大多数存储引擎有更高的限制。

MyISAM和InnoDB存储引擎的表默认创建的都是BTree索引，MEMORY存储引擎默认使用hash索引，但也支持BTree索引。

<!--more-->

创建索引语法：

```mysql
create [unique|fulltext|spatial] index index_name [using index_type] 
			on tbl_name (index_col_name,...)
index_col_name:
    col_name [(length)][ASC|DESC];
drop index index_name on tbl_name;

 -- 索引创建使用删除
create index salaryMon on salary(salary(5));
explain select * from salary where salary<5000;
drop index salaryMon on salary;
```

#### 索引设计原则

- 最适合索引的列是出现在WHERE子句中的列，或连接子句中的指定的列
- 使用唯一索引，索引咧中基数越大，效果越好
- 使用短索引，指定前缀长度
- 利用最左前缀
- 不要过度索引，索引会占用磁盘空间，并降低写操作性能，在修改表时，索引也必须要更新或重构
- InnoDB会保存主键键值，所以主键键值尽可能选择较短的数据类型

#### BTREE索引和HASH索引

##### Btree索引

可以使用<、>、>=、<=、BETWEEN、!=、<>、LIKE 'pattern'

##### Hash索引

只用于使用=或<=>操作符的等式比较